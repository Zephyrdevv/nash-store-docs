# Guides

## Adding a New Drug Substance

1. Open `config.lua`.
2. Add a new entry to `Config.Substances`:

```lua
{ item = "pills", effect = "RampjumpFog", duration = 15000 }
```

3. Add the item to your inventory database.
4. Restart the resource.

## Using Without Items (Command-Based)

Set `Config.UseItem = false` and use the registered command to trigger effects directly.

## Customizing Screen Effects

Refer to the GTA V native reference for a full list of screen effect names. Common ones:

- `DrugsMichaelAliensFight`
- `DrugsTrevorClownsFight`
- `RampjumpFog`
