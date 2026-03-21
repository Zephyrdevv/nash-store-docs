# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.Framework | auto | auto, esx, qb |
| Config.EnableRobbery | false | Enable shop robbery mechanic |
| Config.RobberyTimeout | 300 | Cooldown between robberies (seconds) |
| Config.Shops | table | Array of shop definitions |

## Defining a Shop

```lua
Config.Shops = {
    {
        name = "247_main",
        label = "24/7 Supermarket",
        coords = vec3(25.0, -1347.0, 29.4),
        heading = 0.0,
        items = {
            { name = "water", price = 2 },
            { name = "sandwich", price = 5 }
        }
    }
}
```
