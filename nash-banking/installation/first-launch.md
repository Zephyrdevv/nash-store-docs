# First Launch

## Console checks

On server start, you should see these lines in the console (framework line varies):

```
[Nash Banking] Framework detected: esx (obj: OK)
```

Other boot messages to expect:

| Line | Meaning |
|---|---|
| `[Nash Banking] Framework detected: esx (obj: OK)` | Bridge loaded — replace `esx` with `qbcore` / `qbox` depending on your framework. `obj: MISSING` means the framework object fetch failed (check resource start order). |
| `[Nash Banking Phone] Could not add app: …` | Appears only on error — LB Phone app registration failed. |
| `[Nash Banking] No supported framework detected` | Fatal — no supported framework is running. Check `es_extended` / `qb-core` / `qbx_core` resource state. |

The resource prints **no money / sensitive logs** on boot by default. Transaction logs go to Discord webhooks (see [`config.discord.lua`](../config/config-discord.md)).

## Start order in `server.cfg`

```cfg
# Dependencies first
ensure oxmysql
ensure ox_lib
ensure ox_inventory
ensure ox_target

# Framework
ensure es_extended     # or qb-core / qbx_core

# Optional: LB Phone + apps
ensure lb-phone

# Nash Banking resources
ensure nash_banking
ensure nash_banking_phone
ensure nash_businessbanking_phone
```

`oxmysql` **must** start before `nash_banking` — the banking database layer uses `MySQL.*.await`.

## In-game checks

1. **Bank agency** — walk to a bank NPC (default: `149.47, -1042.06`). With `Config.InteractionType = 'ox_target'`, aim at the ped and pick "Accéder à la banque". The full desktop banking UI opens.
2. **ATM** — approach any Fleeca / standalone ATM prop. Press `E` (or use `ox_target`). The ATM UI opens with a card insertion animation.
3. **TPE test** — run `/testTPE` in chat. A 150€ test payment appears on your screen; pick a card and confirm — your bank balance drops by 150€.
4. **LB Phone** — open the phone. The `Nash Banking` app should be listed (pre-installed if `Config.LBPhone.DefaultApp = true`).
5. **Admin** — as an admin, run `/bankadmin`. The admin dashboard opens with stats and player search.

## Database check

After a player registers for the first time, verify the rows appear:

```sql
SELECT identifier, firstname, lastname FROM nash_players;
SELECT id, identifier, card_type, card_number FROM nash_cards;
SELECT identifier, tier FROM nash_subscriptions;
```

The registration flow automatically creates:

- one row in `nash_players` (with the form data),
- one row in `nash_cards` (the default `standard` card, `card_number` randomly generated),
- one row in `nash_subscriptions` (tier = `standard`).

## If something doesn't work

Head over to [Common Errors](../common-errors.md).
