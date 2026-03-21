# NUI Callbacks

## clothesshop:preview
Fired on each component change for live preview.

**Payload:**
```json
{
  "component": 11,
  "drawable": 5,
  "texture": 0
}
```

## clothesshop:purchase
Fired when the player confirms a purchase.

**Payload:**
```json
{
  "component": 11,
  "drawable": 5,
  "texture": 0,
  "price": 120
}
```

## clothesshop:saveOutfit
Fired when the player saves an outfit.

**Payload:**
```json
{
  "name": "Casual Friday",
  "outfit": {}
}
```
