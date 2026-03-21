# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.UseItem | true | Require item to use |
| Config.ItemName | "lean" | Inventory item name |
| Config.Duration | 30000 | Effect duration in ms |
| Config.AnimDict | "amb@world_human_drinking@coffee@male@idle_a" | Animation dictionary |
| Config.AnimName | "idle_a" | Animation clip name |
| Config.ScreenEffect | "DrugsMichaelAliensFight" | Screen effect name |

## Adding Multiple Substances

```lua
Config.Substances = {
    { item = "lean", effect = "DrugsMichaelAliensFight", duration = 30000 },
    { item = "weed", effect = "DrugsMichaelAliensFightIn", duration = 20000 }
}
```
