# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.MaxCharacters | 3 | Maximum character slots per player |
| Config.Framework | auto | auto, esx, qb |
| Config.SpawnPoint | table | Default spawn coords for new characters |
| Config.EnableDeletion | true | Allow players to delete characters |
| Config.CameraCoords | table | Camera position for selection screen |

## Default Spawn Point

```lua
Config.SpawnPoint = {
    coords = vec4(215.0, -810.0, 30.7, 0.0),
    label = "City Hall"
}
```

## Character Slots

```lua
Config.MaxCharacters = 5
```
