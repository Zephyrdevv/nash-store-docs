# Server Events

## nash_shops:server:purchaseItem
Fired when a player purchases an item from a shop.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| shopName | string | Shop identifier |
| item | string | Item name |
| amount | number | Quantity purchased |
| price | number | Total price paid |

## nash_shops:server:robberyStart
Fired when a player begins robbing a shop.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| shopName | string | Shop being robbed |
