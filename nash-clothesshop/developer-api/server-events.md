# Server Events

## nash_clothesshop:server:purchaseItem
Fired when a player purchases a clothing item.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| component | number | Component ID |
| drawable | number | Drawable index |
| texture | number | Texture index |
| price | number | Item price |

## nash_clothesshop:server:saveOutfit
Saves a player outfit to the database.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| outfitData | table | Full component/prop outfit table |
| outfitName | string | Outfit label |
