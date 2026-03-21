# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.Framework | auto | auto, esx, qb |
| Config.SuccessChance | 50 | Base pickpocket success % |
| Config.Cooldown | 30 | Cooldown between attempts (seconds) |
| Config.LootTable | table | Items that can be pickpocketed |
| Config.CopAlert | true | Alert nearby police on failure |

## Loot Table Example

```lua
Config.LootTable = {
    { name = "cash",     min = 10,  max = 200, chance = 70 },
    { name = "phone",    min = 1,   max = 1,   chance = 20 },
    { name = "wallet",   min = 1,   max = 1,   chance = 10 }
}
```

## Success Chance Modifiers

Success chance can be modified by player job, skill level, or items in config.lua.
