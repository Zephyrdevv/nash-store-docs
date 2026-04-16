# Common Errors

Fast fixes for the issues we see most. Each entry lists the symptom, the cause, and the fix.

## Framework detection

### `[Nash Banking] No supported framework detected`

**Cause** — `Config.Framework = 'auto'` but none of `es_extended`, `qb-core`, `qbx_core` is `'started'` at the time Nash Banking boots.

**Fix**

1. In `server.cfg`, make sure your framework is started **before** `nash_banking`:

    ```cfg
    ensure es_extended
    ensure nash_banking
    ```

2. Or force detection explicitly in `shared/config.lua`:

    ```lua
    Config.Framework = 'esx'  -- or 'qbcore' / 'qbox'
    ```

### `[Nash Banking] Framework detected: esx (obj: MISSING)`

**Cause** — the framework was detected but `getSharedObject()` / `GetCoreObject()` threw an error.

**Fix** — verify the framework is fully loaded (look earlier in the console for errors). A `could not find export getSharedObject` usually means your ESX install is broken. Restart the framework first, then `restart nash_banking`.

### `Unknown Player` appears in the UI

**Cause** — the framework returns an empty `charinfo` / `firstName` for the player (common on very fresh characters or on broken character-creation scripts).

**Fix** — the bridge uses the following fallback order on QBCore / QBOX: `charinfo` table → `charinfo` JSON string → `PlayerData.name` → `'Unknown'`. If you still see `Unknown`, run:

```
select citizenid, charinfo from players where citizenid = 'YOUR_ID';
```

If `charinfo` is `{}` or `null`, the character was never properly created. Re-run the char-creation flow.

## TPE

### Nothing happens when I use the TPE item

**Cause** — the `nash_tpe` item is missing the `client.export` declaration in `ox_inventory/data/items.lua`.

**Fix** — add the export field (see [Installation › ox_inventory](installation/ox-inventory.md)):

```lua
['nash_tpe'] = {
    label = 'Payment Terminal',
    weight = 500,
    stack = false,
    close = true,
    client = { export = 'nash_banking.nash_tpe' },
},
```

Restart `ox_inventory` and `nash_banking`.

### The TPE opens but shows "No player nearby"

**Cause** — no other player is within `Config.TPE.PlayerToPlayer.Distance` meters (default `3.0`) when the merchant confirms the amount.

**Fix** — step closer to the buyer, or bump the distance in `shared/config.business.lua`:

```lua
Config.TPE.PlayerToPlayer.Distance = 5.0
```

### Buyer receives the TPE but can't confirm

**Cause** — the buyer has no active card (all cards are frozen or still in `delivery_status = 'pending'`).

**Fix** — run:

```sql
SELECT id, identifier, card_type, frozen, delivery_status FROM nash_cards WHERE identifier = 'BUYER_ID';
```

If all rows show `frozen = 1` → unfreeze one via the cards page, or ask the player to order a new card. If `delivery_status = 'pending'`, the card needs to be picked up at a delivery location.

## Cards

### The card prop does not spawn in the player's hand

**Cause** — streamed archetypes not loaded. Usually a first-connect issue or another resource declaring the same archetype name.

**Fix**

1. Reconnect once after the first start of `nash_banking` (the `DLC_ITYP_REQUEST` registers for the next session).
2. Grep your resources for `bzzz_prop_payment_terminal` and `nash_banking.ytyp` — if another resource ships the same file, rename one side.
3. Confirm the `data_file` lines are present in `fxmanifest.lua`.

### Avatar disappears after server restart

**Cause** — the avatar URL was rejected by server-side validation (max 500 chars, must start with `http://` or `https://`). A rejected update clears the row.

**Fix** — host the avatar on a public CDN (Discord CDN, imgur, etc.), keep the URL short, and don't use `data:` URIs.

## Savings

### Savings balance on the home differs from the Accounts modal

**Cause** — fixed in the current release. Older versions computed the home-screen savings as `balance * 0.038` (a mock value). Both views now read `savingsBalance` from the server.

**Fix** — make sure your `nash_banking/web/` is rebuilt from the latest source. `web/src/components/home/AccountsModal.tsx` should read `savingsBalance` from the `useBank()` context, not compute from `balance`.

## Phone

### LB Phone notifications are not received

**Cause** — `Config.PhoneNotifications = false` in `shared/config.lua`, or LB Phone is not started before `nash_banking`.

**Fix** — set `Config.PhoneNotifications = true` and verify in `server.cfg`:

```cfg
ensure lb-phone
ensure nash_banking
ensure nash_banking_phone
```

### The Nash apps don't appear in LB Phone

**Cause** — `nash_banking_phone` started before `lb-phone`. The app waits up to a full minute for LB Phone, but if you then restart `lb-phone`, the app re-registers automatically.

**Fix**

1. Confirm the order in `server.cfg` (lb-phone → nash_banking → nash_banking_phone).
2. If LB Phone was restarted manually, you should see `[Nash Banking Phone] Could not add app: ...` (on error) or nothing (on success) in the console.
3. Test with `defaultApp = true` (the default) to avoid the "install from app store" step.

## Database

### A table is missing after import

**Cause** — the SQL file is idempotent (`CREATE TABLE IF NOT EXISTS`), but you may have imported only part of the file if your client aborted on a warning.

**Fix** — re-run `nash_banking.sql` in full. Missing tables will be created; existing ones will be skipped. No data is lost.

### `Error 1062: Duplicate entry … for key 'PRIMARY'` when re-importing

**Cause** — you imported a second SQL containing `INSERT` statements for an existing row (typically `nash_stock_prices`).

**Fix** — Nash Banking seeds stock / crypto prices at runtime, not via SQL. Re-run only `nash_banking.sql` (the schema file), not any stray seed scripts.

### `Unknown column 'avatar' in field list`

**Cause** — an old version of `nash_players` is still in the DB.

**Fix**

```sql
ALTER TABLE nash_players
  ADD COLUMN IF NOT EXISTS avatar VARCHAR(500) DEFAULT NULL;
```
