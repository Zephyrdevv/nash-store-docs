# Guides

## Adding a Custom Barber Location

1. Find the coords in-game using a coord script or F8 console.
2. Open `config.lua` and add a new entry to `Config.Locations`:

```lua
{
    coords = vec3(X, Y, Z),
    heading = 0.0,
    label = "My Barber"
}
```

3. Save and restart the resource.

## Adjusting Prices

Edit `Config.Prices` in config.lua to set per-category pricing.

## Disabling Camera Preview

Set `Config.CameraEnabled = false` in config.lua to disable the camera during customization.
