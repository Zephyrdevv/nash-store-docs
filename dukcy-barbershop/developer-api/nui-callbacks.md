# NUI Callbacks

## barbershop:confirm
Triggered when the player confirms their selection in the NUI.

**Payload:**
```json
{
  "category": "hair",
  "value": 3
}
```

## barbershop:cancel
Triggered when the player cancels without purchasing. Restores the previous appearance.

## barbershop:preview
Triggered on each slider change for live preview without committing.

**Payload:**
```json
{
  "component": "beard",
  "index": 2,
  "texture": 0
}
```
