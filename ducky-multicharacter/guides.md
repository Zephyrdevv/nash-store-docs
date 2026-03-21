# Guides

## Adding a Custom Spawn Point

1. Find your in-game coords using a coord tool.
2. Open `config.lua` and add to `Config.SpawnPoint`:

```lua
Config.SpawnPoint = {
    coords = vec4(X, Y, Z, Heading),
    label = "My Location"
}
```

3. Restart the resource.

## Increasing Character Slots

Set `Config.MaxCharacters` to the desired number. Note that increasing slots may require UI adjustments.

## Disabling Character Deletion

To prevent players from deleting characters, set `Config.EnableDeletion = false` in config.lua.
