# Guides

## Adding a New Shop Location

1. Get your in-game coords.
2. Open `config.lua` and add to `Config.Locations`:

```lua
{
    coords = vec3(X, Y, Z),
    heading = 0.0,
    label = "My Shop",
    type = "mid_end"
}
```

3. Restart the resource.

## Saving Outfits

Players can save outfits by clicking the "Save Outfit" button in the NUI and providing a name. Saved outfits can be loaded from the outfit tab.

## Restricting Items by Job

To limit clothing items to specific jobs, add job checks inside the item filter table in config.lua.
