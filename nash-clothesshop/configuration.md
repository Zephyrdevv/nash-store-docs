# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.Framework | auto | auto, esx, qb |
| Config.Currency | money | money or bank |
| Config.AllowOutfits | true | Enable outfit saving/loading |
| Config.MaxOutfits | 10 | Max saved outfits per player |
| Config.Locations | table | Array of shop location data |

## Adding a Shop Location

```lua
Config.Locations = {
    {
        coords = vec3(72.0, -1393.0, 29.0),
        heading = 0.0,
        label = "Discount Store",
        type = "low_end"
    }
}
```

## Shop Types

| Type | Description |
|------|-------------|
| low_end | Budget clothing selection |
| mid_end | Standard clothing selection |
| high_end | Premium clothing selection |
