# Guides

## Setting Up the Loot Table

1. Open `config.lua`.
2. Edit `Config.LootTable` to define what can be stolen:

```lua
Config.LootTable = {
    { name = "cash",   min = 50, max = 300, chance = 60 },
    { name = "watch",  min = 1,  max = 1,   chance = 15 },
    { name = "keycard", min = 1, max = 1,   chance = 5  }
}
```

3. Ensure all item names exist in your ox_inventory items table.

## Configuring Police Alerts

Set `Config.CopAlert = true` and configure the dispatch event or police notification in `server/alerts.lua`.

## Adjusting Cooldowns

Change `Config.Cooldown` (in seconds) to control how often players can attempt pickpocketing.
