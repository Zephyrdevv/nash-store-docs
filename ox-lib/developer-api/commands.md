# Commands

## lib.addCommand(name, options, cb)
Registers a chat command with built-in argument parsing and permission checks.

| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | Command name (without /) |
| options | table | Command options and params |
| cb | function | Handler function |

### options fields

| Field | Type | Description |
|-------|------|-------------|
| help | string | Help text shown in F1 menu |
| restricted | string | Ace permission required |
| params | table | Array of parameter definitions |

## Example

```lua
lib.addCommand("givemoney", {
    help = "Give money to a player",
    restricted = "group.admin",
    params = {
        { name = "id", type = "playerId", help = "Target player ID" },
        { name = "amount", type = "number", help = "Amount to give" }
    }
}, function(source, args)
    -- handler
end)
```
