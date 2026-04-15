# ox\_inventory Items

Nash Banking ships with two items that must be added to your `ox_inventory/data/items.lua` file. Both rely on client-side exports, so the `client.export` field is mandatory.

## Items to add

The exact definitions are provided in `nash_banking/INSTALL/ox_inventory/data/items.lua`.

### `nash_card_physical`

_Physical bank card — used when the Physical Card feature is enabled._

### `nash_tpe`

_Payment terminal — used by merchants for P2P payments._

{% hint style="danger" %}
If you forget the `client.export = 'nash_banking.nash_tpe'` line, the item will do **nothing** when used. This is the #1 reason "my TPE doesn't open".
{% endhint %}

## After editing

Restart `ox_inventory` (or the server) for the new items to be registered.
