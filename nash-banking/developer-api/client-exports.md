# Client Exports

Client-side exports exposed by Nash Banking. The two item exports (`nash_tpe`, `nash_card_physical`) are **mandatory** and must be declared in `ox_inventory/data/items.lua` for the items to work — see [Installation › ox_inventory](../installation/ox-inventory.md).

<details>

<summary>nash_tpe</summary>

Called by `ox_inventory` when a player uses the `nash_tpe` item. Spawns the terminal prop in the player's hand and opens the seller POS overlay.

**Declaration in ox_inventory**

```lua
['nash_tpe'] = {
    label = 'Payment Terminal',
    weight = 500,
    stack = false,
    close = true,
    description = 'Nash Banking TPE - Player-to-Player payments',
    client = {
        export = 'nash_banking.nash_tpe',
    },
},
```

**Signature**

```lua
exports('nash_tpe', function(data, slot)
    -- data: item data from ox_inventory
    -- slot: inventory slot (slot.metadata is forwarded as item metadata)
end)
```

**Metadata fields (optional)**

| Field | Type | Description |
|---|---|---|
| `business_id` | `number?` | If set, the P2P payment is credited to this business instead of the seller's personal account |

**Flow**

1. Spawns the terminal prop.
2. Opens the POS NUI (amount input).
3. On confirm → finds nearest player → triggers `nash_banking:p2pInitiate` server event.

</details>

<details>

<summary>nash_card_physical</summary>

Called by `ox_inventory` when a player uses the `nash_card_physical` item. Opens the card inspection UI with the data stored in `slot.metadata`.

**Declaration in ox_inventory**

```lua
['nash_card_physical'] = {
    label = 'Bank Card',
    weight = 10,
    stack = false,
    close = true,
    description = 'Physical bank card',
    client = {
        export = 'nash_banking.nash_card_physical',
    },
},
```

**Signature**

```lua
exports('nash_card_physical', function(data, slot)
    -- slot.metadata contains card_number, cvv, expiry, pin, card_type, cardholder, frozen
end)
```

**Metadata fields**

| Field | Type |
|---|---|
| `card_number` | `string` |
| `cvv` | `string` |
| `expiry` | `string` |
| `pin` | `string` |
| `card_type` | `'standard' \| 'plus' \| 'premium'` |
| `cardholder` | `string` |
| `frozen` | `boolean` |
| `description` | `string?` |
| `card_id` | `number` (DB id — used for auto-freeze on item loss) |

</details>

<details>

<summary>openBank</summary>

Opens the banking NUI programmatically (same result as using the `/bank` command or interacting with a bank NPC).

**Signature**

```lua
exports.nash_banking:openBank()
```

**Example**

```lua
-- Custom hotkey to open the bank
RegisterCommand('mybank', function()
    exports.nash_banking:openBank()
end)
```

</details>

<details>

<summary>closeBank</summary>

Force-closes the banking NUI.

**Signature**

```lua
exports.nash_banking:closeBank()
```

</details>
