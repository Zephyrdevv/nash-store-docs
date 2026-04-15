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
