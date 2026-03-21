# NUI Callbacks

## shops:purchase
Fired when the player confirms a purchase.

**Payload:**
```json
{
  "shop": "247_main",
  "item": "water",
  "amount": 2
}
```

## shops:close
Fired when the player closes the shop menu.

## shops:requestItems
Fired when the shop NUI requests the item list for a shop.

**Payload:**
```json
{
  "shop": "247_main"
}
```
