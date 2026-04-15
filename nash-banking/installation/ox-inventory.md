# ox\_inventory Items

Nash Banking ships with two items that must be added to your `ox_inventory/data/items.lua` file. Both rely on client-side exports, so the `client.export` field is **mandatory**.

{% hint style="danger" %}
If you forget the `client.export` line, the item will do **nothing** when used. This is the #1 reason "my TPE doesn't open" or "my physical card can't be inspected".
{% endhint %}

## Items to add

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

## What each item does

### `nash_card_physical`

The physical bank card item. Given to the player when they order a physical card (if the Physical Card feature is enabled). Using it opens the card inspection UI showing the card number, expiry, CVV and PIN.

### `nash_tpe`

The payment terminal. Given to a player or business when they buy a TPE from the business banking. Using it spawns the terminal prop in the player's hand and opens the seller POS UI so they can charge a nearby player.

## After editing

Restart `ox_inventory` (or the server) for the new items to be registered.

```
restart ox_inventory
```
