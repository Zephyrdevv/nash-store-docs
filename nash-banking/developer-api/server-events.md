# Server Events

Net events registered on the server. These are triggered from the client with `TriggerServerEvent`. All of them are rate-limited (2s cooldown) to prevent abuse.

<details>

<summary>nash_banking:tpeConfirm</summary>

Fired by the buyer's client when they confirm a TPE payment with a selected card.

```lua
TriggerServerEvent('nash_banking:tpeConfirm', selectedCardId)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `selectedCardId` | `number` | Database ID of the card picked in the wallet UI |

**Guards**

- Rate limit: 2s per player
- Validates the card belongs to the player and is active (not frozen, delivered)
- Re-checks bank balance and spending limits server-side (anti-cheat)

**Fires back**

- `nash_banking:tpeResult` → `{ success, message? }`
- `nash_banking:updateBalance` on success

</details>

<details>

<summary>nash_banking:tpeCancel</summary>

Fired by the buyer's client to cancel an ongoing TPE payment.

```lua
TriggerServerEvent('nash_banking:tpeCancel')
```

**Parameters** — none

**Behavior** — Resolves the pending payment with `success = false` and `message = 'cancelled'`.

</details>

<details>

<summary>nash_banking:p2pInitiate</summary>

Fired by the merchant's client to start a player-to-player TPE payment. The server finds the buyer, opens the TPE overlay on their screen, and routes the funds (to personal or business account).

```lua
TriggerServerEvent('nash_banking:p2pInitiate', targetServerId, amount, description, businessId)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `targetServerId` | `number` | Buyer's server ID (nearest player picked client-side) |
| `amount` | `number` | Amount to charge (0 < amount ≤ `Config.TPE.PlayerToPlayer.MaxAmount`) |
| `description` | `string` | Payment description |
| `businessId` | `number?` | Optional business ID — if set and the merchant is a registered employee, the funds go to the business account. Otherwise to the merchant's personal account. |

**Guards**

- Rate limit: 2s per player
- `Config.TPE.PlayerToPlayer.Enabled` must be `true`
- `amount` capped by `Config.TPE.PlayerToPlayer.MaxAmount` (default 50 000)
- Buyer must have at least one active card
- Business employment is verified against `nash_business_employees`
- A fee (`Config.TPE.PlayerToPlayer.Fee`, default 2 %) is deducted from the merchant's receipt

**Fires back**

- Seller: `nash_banking:sellerTransferOut`, then `nash_banking:p2pResult`
- Buyer: `nash_banking:buyerReceiveTPE`, then `nash_banking:tpeResult`

</details>

---

## Integration hooks

Server-side events fired by the banking script (or that your own resource is expected to trigger) for inter-resource wiring. These are **not** client-triggered RPCs — they're plain `TriggerEvent` / `AddEventHandler` server-to-server hooks.

<details>

<summary>nash_banking:itemRemoved</summary>

Normalized "banking item was removed from a player's inventory" event. Fired **by the banking script** whenever the physical bank card item (`nash_card_physical`) or any Nash item leaves a player's inventory. Used internally by the `Config.PhysicalCard.FreezeOnLoss` feature to auto-freeze the corresponding card in the database.

```lua
TriggerEvent('nash_banking:itemRemoved', source, itemName, count, metadata, slot)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `source` | `number` | Server ID of the player who lost the item |
| `itemName` | `string` | Item name (e.g. `nash_card_physical`, `nash_tpe`) |
| `count` | `number` | Amount removed |
| `metadata` | `table` | Item metadata at removal time — for `nash_card_physical` this contains `{ card_id = ... }` |
| `slot` | `number` | Slot from which the item was removed |

**How it gets fired**

- **`ox_inventory`** — wired automatically. `bridge/inventory/server.lua` listens for `ox_inventory:removedItem` and re-emits it as `nash_banking:itemRemoved`.
- **Other inventories (`qs-inventory`, `qb-inventory`, custom)** — the banking script cannot listen to your inventory's hook by itself. Wire it yourself, in your own resource:

  ```lua
  AddEventHandler('your-inventory:server:removedItem', function(source, item, count, metadata, slot)
      TriggerEvent('nash_banking:itemRemoved', source, item, count, metadata, slot)
  end)
  ```

If your inventory has no such hook, skip it — the rest of the banking still works, only the auto-freeze-on-loss feature becomes inert.

**Listening yourself**

You can also listen to this event from your own resource — useful e.g. to log every physical card loss, or to award something when a specific banking item is lost:

```lua
AddEventHandler('nash_banking:itemRemoved', function(source, itemName, count, metadata, slot)
    if itemName == 'nash_card_physical' and metadata and metadata.card_id then
        print(('Player %d lost card #%s'):format(source, metadata.card_id))
    end
end)
```

</details>
