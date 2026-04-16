# How to enable physical cards

Physical cards are **enabled by default**. This guide walks through the full flow so you know what to tweak.

## Enabling the feature

Physical cards require three pieces:

1. **The `nash_card_physical` item declared in `ox_inventory/data/items.lua`** (see [Installation › ox_inventory](../installation/ox-inventory.md)):

    ```lua
    ['nash_card_physical'] = {
        label = 'Bank Card',
        weight = 10,
        stack = false,
        close = true,
        client = { export = 'nash_banking.nash_card_physical' },
    },
    ```

2. **`Config.Delivery.Item = 'nash_card_physical'`** in `shared/config.lua` (default).

3. **At least one delivery point** — all bank branches automatically act as pickup points; additional points can be added via `Config.DeliveryPoints`.

## Delivery flow

From the player's perspective:

1. Open `/bank` → Cards tab → **New card** → choose `physical` → pick a pickup location.
2. `Config.Delivery.Cost` is debited (default 0 = free).
3. The card is created in `nash_cards` with `delivery_status = 'pending'` and `delivery_ready_at = NOW() + Config.Delivery.Delay`.
4. After `Config.Delivery.Delay` seconds (default 0 = instant), the status flips to `'ready'`.
5. Player walks to the pickup point → interacts with the NPC → the card is handed over as a `nash_card_physical` item with the card's metadata. `delivery_status` becomes `'delivered'`.

```lua
Config.Delivery = {
    Cost = 0,        -- Delivery price
    Delay = 0,       -- Seconds before 'pending' → 'ready' (set to 3600 for "1h by mail")
    Item = 'nash_card_physical',
    -- Blip + OxTarget configs...
}
```

## Freezing on loss

```lua
Config.PhysicalCard = {
    FreezeOnLoss = true,      -- default
    AllowReorder = true,
    ReorderCost = 500,
    KeepSameNumber = false,
}
```

With `FreezeOnLoss = true`, the `ox_inventory:removedItem` event handler in `server/hooks.lua` watches for removals of `nash_card_physical` and, when `metadata.card_id` is set, calls `Database.FreezeCardById` — the card becomes unusable until the player reorders.

Set to `false` if you want card items to be transferable without side-effects (less realistic but allows giving cards to others).

## Requiring the physical card for TPE payments

By default, players can pay with a virtual card even if they also own the physical version. To **require the physical item in inventory** for any card payment involving a physical-format card:

```lua
Config.TPE = {
    RequirePhysicalCard = true,
    ...
}
```

Card payments then verify the player has the matching `nash_card_physical` item with `metadata.card_id` equal to the selected card's ID. Missing → payment rejected.

## Inspecting a card from the inventory

When a player uses the item, the `nash_card_physical` client export opens the inspect UI with the card's metadata — number, CVV, expiry, PIN, cardholder, frozen state.

Integration in `ox_inventory/data/items.lua` (just the `client.export` line) is enough — nothing else to wire.

## Reordering a lost card

Players can reorder from the Cards tab. `Config.PhysicalCard.ReorderCost` is debited and a new delivery starts:

- `KeepSameNumber = true` — same `card_number`, new CVV + PIN + expiry.
- `KeepSameNumber = false` (default) — all card details regenerated.

Frozen cards that were never lost (e.g. manually frozen by the player) are unfrozen on reorder.
