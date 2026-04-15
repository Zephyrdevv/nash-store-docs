# Client Exports

This section provides all available client exports for Nash Banking. These exports are primarily used by `ox_inventory` to trigger item behaviors (using the TPE, inspecting a physical card). Each export includes a short description and usage example.

<details>

<summary>nash_tpe</summary>

```lua
-- Called by ox_inventory when a player uses the nash_tpe item
-- Declared in ox_inventory/data/items.lua via client.export = 'nash_banking.nash_tpe'
exports('nash_tpe', function(data, slot)
    -- spawns the terminal prop and opens the seller POS UI
end)
```

_Signature, metadata format, example — filled in Pass 2._

</details>

<details>

<summary>nash_card_physical</summary>

```lua
-- Called by ox_inventory when a player uses the nash_card_physical item
-- Declared in ox_inventory/data/items.lua via client.export = 'nash_banking.nash_card_physical'
exports('nash_card_physical', function(data, slot)
    -- opens the card inspection UI with slot.metadata
end)
```

_Signature, metadata format, example — filled in Pass 2._

</details>

<details>

<summary>Other exports</summary>

_Additional client exports will be documented in Pass 2._

</details>
