# Inventory Items

Nash Banking ships with two items that must be added to your inventory's items config:

- **`nash_card_physical`** — the physical bank card given to the player on card delivery.
- **`nash_tpe`** — the payment terminal obtained through business banking.

Both rely on client-side exports pointing to `nash_banking`, so the item's `client.export` (or your inventory's equivalent) is **mandatory**.

{% hint style="danger" %}
If you forget the `client.export` line, the item will do **nothing** when used. This is the #1 reason "my TPE doesn't open" or "my physical card can't be inspected".
{% endhint %}

## ox_inventory

Open `ox_inventory/data/items.lua` and paste the following entries into the returned table:

```lua
['nash_card_physical'] = {
    label = 'Bank Card',
    weight = 0,
    stack = false,
    close = true,
    description = 'Nash Banking physical card',
    client = {
        export = 'nash_banking.nash_card_physical',
    },
},

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

You can also find these definitions ready to copy-paste in `nash_banking/INSTALL/ox_inventory/data/items.lua`.

Restart `ox_inventory` (or the server) for the new items to be registered:

```
restart ox_inventory
```

## qs-inventory / qb-inventory / other inventories

Port the same two items into your inventory's items config. The item names **must** stay `nash_card_physical` and `nash_tpe` — the banking script hard-codes them.

Ensure both items call the correct nash exports when used:

- `nash_card_physical` → `exports.nash_banking:nash_card_physical(data, slot)`
- `nash_tpe` → `exports.nash_banking:nash_tpe(data, slot)`

Consult your inventory's docs for the exact usable-item syntax.

## Custom inventories

If your inventory isn't auto-detected, see the [Custom Inventory](../compatibility/custom-inventory.md) guide — you only need to wire 4 functions in `bridge/inventory/config.lua` and the banking script will use your inventory transparently.

## What each item does

### `nash_card_physical`

The physical bank card item. Given to the player when they order a physical card (if the Physical Card feature is enabled). Using it opens the card inspection UI showing the card number, expiry, CVV and PIN.

### `nash_tpe`

The payment terminal. Given to a player or business when they buy a TPE from the business banking. Using it spawns the terminal prop in the player's hand and opens the seller POS UI so they can charge a nearby player.
