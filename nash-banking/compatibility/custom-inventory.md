# Custom Inventory

If you use an inventory that is not auto-detected (`ox_inventory`, `qs-inventory`, `qb-inventory`), you can wire it into Nash Banking through the inventory bridge.

## Files to edit

Both files below are in `escrow_ignore` — you can edit them directly on your server.

| File | Purpose |
|---|---|
| `bridge/inventory/config.lua` | Mode selection (`Config.Inventory`) + `Config.CustomInventory` stub |
| `bridge/inventory/server.lua` | Dispatch to `Config.CustomInventory` when the mode is `'custom'` — you normally don't need to touch this file |

## Enabling custom mode

In `bridge/inventory/config.lua`:

```lua
Config.Inventory = 'custom'
```

Accepted values: `'auto'` (default), `'ox_inventory'`, `'qs-inventory'`, `'qb-inventory'`, `'custom'`.

## The 4 functions to implement

Fill in each function inside `Config.CustomInventory`. All run **on the server**.

### `AddItem(source, itemName, count, metadata)`

Add `count` of `itemName` to the player's inventory with metadata. Returns truthy on success, falsy on failure (e.g. inventory full).

Used to give physical bank cards (`nash_card_physical`) and TPE items (`nash_tpe`).

```lua
AddItem = function(source, itemName, count, metadata)
    return exports['your-inventory']:AddItem(source, itemName, count, metadata)
end,
```

### `RemoveItem(source, itemName, count, metadata, slot)`

Remove `count` of `itemName` from the player's inventory. `slot` and `metadata` are optional filters — when provided, only that specific slot or matching metadata is removed.

```lua
RemoveItem = function(source, itemName, count, metadata, slot)
    return exports['your-inventory']:RemoveItem(source, itemName, count, slot)
end,
```

### `Search(source, itemName)`

Search the player's inventory for every slot holding `itemName`.

{% hint style="warning" %}
**MUST return an ARRAY** of tables shaped like:
```lua
{
    { slot = 3, count = 1, metadata = { card_id = 42, ... } },
    { slot = 8, count = 1, metadata = { card_id = 55, ... } },
}
```
Return an empty table when nothing is found. The `metadata.card_id` field is what Nash Banking uses to know which physical card sits in which slot.
{% endhint %}

```lua
Search = function(source, itemName)
    return exports['your-inventory']:Search(source, itemName) or {}
end,
```

### `SetMetadata(source, slot, metadata)`

Overwrite the metadata of a specific slot. Used to update the `frozen` flag on a physical card when the player toggles freeze from the phone or banking UI.

```lua
SetMetadata = function(source, slot, metadata)
    exports['your-inventory']:SetMetadata(source, slot, metadata)
end,
```

## Optional — "Item removed" event

Nash Banking can auto-freeze a physical card in the database when the corresponding item leaves the player's inventory (dropped, stolen, given away…). This is only used if `Config.PhysicalCard.FreezeOnLoss = true`.

For **ox_inventory** this is wired automatically.

For any other inventory, wire your inventory's "item removed" event to fire our normalized event, e.g. in your own resource:

```lua
AddEventHandler('your-inventory:server:removedItem', function(source, item, count, metadata, slot)
    TriggerEvent('nash_banking:itemRemoved', source, item, count, metadata, slot)
end)
```

If your inventory has no such event, skip this step — the rest of the banking still works, only the auto-freeze-on-loss feature becomes inert.

## Testing your bridge

Checklist after wiring a custom inventory:

1. `[Nash Banking] Inventory system: custom` prints on server start.
2. Order a physical card at a bank NPC and pick it up on delivery day — the `nash_card_physical` item appears in your inventory with the correct card metadata.
3. Freeze the card from the phone app → the item's metadata `frozen` flag updates in your inventory.
4. Delete the card from the phone app → the item disappears from your inventory.
5. Buy a business TPE from the business banking app → the `nash_tpe` item appears in your inventory.
6. (Optional, if `FreezeOnLoss` is on) Drop the physical card → the card is auto-frozen in the DB.
