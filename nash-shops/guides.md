# Guides

## Adding a New Shop

1. Open `config.lua` and add an entry to `Config.Shops`:

```lua
{
    name = "hardware_shop",
    label = "Hardware Store",
    coords = vec3(X, Y, Z),
    heading = 0.0,
    items = {
        { name = "lockpick", price = 50 },
        { name = "screwdriver", price = 15 }
    }
}
```

2. Restart the resource.

## Enabling Robbery

Set `Config.EnableRobbery = true` and configure `Config.RobberyTimeout` and `Config.RobberyReward` in config.lua.

## Adding Items to an Existing Shop

Add new item entries to the `items` table of the target shop in `config.lua`.
