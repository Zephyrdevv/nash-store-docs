# Server Exports

This section provides all available server exports for Nash Banking. These functions allow you to interact directly with the banking system from the server side, enabling custom features and integrations with other resources.

All server exports live under the `nash_banking` resource namespace and use `Bridge.*` internally — they are framework-agnostic (ESX, QBCore, QBOX).

<details>

<summary>CardPayment</summary>

Triggers an animated TPE payment on the player's screen, asks them to pick a card, then debits their bank account. This is the main integration point for other resources (shops, services, etc.).

**Signature**

```lua
local success, message = exports.nash_banking:CardPayment(source, amount, description)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `source` | `number` | Player server ID |
| `amount` | `number` | Amount to charge (integer, > 0) |
| `description` | `string` | Payment description shown on the TPE and saved in the transaction history |

**Returns**

| Name | Type | Description |
|---|---|---|
| `success` | `boolean` | `true` if the payment was confirmed and debited |
| `message` | `string?` | Error key when `success = false` (e.g. `tpe_timeout`, `insufficient_funds`, `tpe_no_card`, `tpe_limit_exceeded`) |

**Behavior**

- Opens the wallet UI with all the player's active (non-frozen, delivered) cards.
- The player selects a card and confirms / cancels.
- Timeout after 60 seconds → returns `false, 'tpe_timeout'`.
- Enforces daily / single spending limits from `Config.Cards.Limits` (per card tier).
- Writes a transaction to `nash_transactions` with type `card_payment`.

**Example — Integration in a shop**

```lua
RegisterNetEvent('my_shop:buyItem', function(itemId, price)
    local src = source
    local success, err = exports.nash_banking:CardPayment(src, price, ('Shop purchase: %s'):format(itemId))
    if not success then
        TriggerClientEvent('my_shop:notify', src, 'error', err or 'Payment failed')
        return
    end
    -- Give the item
    exports.ox_inventory:AddItem(src, itemId, 1)
end)
```

</details>

<details>

<summary>HasActiveCard</summary>

Checks whether a player has at least one non-frozen, delivered card.

**Signature**

```lua
local hasCard = exports.nash_banking:HasActiveCard(source)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `source` | `number` | Player server ID |

**Returns**

| Name | Type | Description |
|---|---|---|
| `hasCard` | `boolean` | `true` if the player owns at least one active card |

**Example — Gate an action behind card ownership**

```lua
if not exports.nash_banking:HasActiveCard(source) then
    TriggerClientEvent('ox_lib:notify', source, { type = 'error', description = 'You need a bank card' })
    return
end
```

</details>

<details>

<summary>InspectCard</summary>

Opens the card inspection UI on the client from metadata. Used internally by the physical card item (`nash_card_physical`) but also exposed so external resources can trigger the inspect overlay.

**Signature**

```lua
exports.nash_banking:InspectCard(source, metadata)
```

**Parameters**

| Name | Type | Description |
|---|---|---|
| `source` | `number` | Player server ID |
| `metadata` | `table` | Card metadata (see fields below) |

**Metadata fields**

| Field | Type | Description |
|---|---|---|
| `card_number` | `string` | 16-digit card number |
| `cvv` | `string` | 3-digit CVV |
| `expiry` | `string` | MM/YY |
| `pin` | `string` | 4-digit PIN |
| `card_type` | `string` | `standard`, `plus` or `premium` |
| `cardholder` | `string` | Name displayed on the card |
| `frozen` | `boolean` | Shows a frozen overlay if `true` |
| `description` | `string` | Optional description |

**Example**

```lua
exports.nash_banking:InspectCard(source, {
    card_number = '4242 4242 4242 4242',
    cvv = '123',
    expiry = '12/28',
    pin = '0000',
    card_type = 'premium',
    cardholder = 'John Doe',
    frozen = false,
})
```

</details>
