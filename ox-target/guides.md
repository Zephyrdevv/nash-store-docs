# Guides

## Adding an Option to a Specific Entity

```lua
local entity = GetClosestObjectOfType(coords, 2.0, hash, false)
exports.ox_target:addLocalEntity(entity, {
    {
        name = "open_safe",
        label = "Open Safe",
        icon = "fas fa-lock-open",
        onSelect = function()
            -- your logic here
        end
    }
})
```

## Adding a Global Vehicle Option

```lua
exports.ox_target:addGlobalVehicle({
    {
        name = "check_engine",
        label = "Check Engine",
        icon = "fas fa-wrench",
        onSelect = function(data)
            print(data.entity)
        end
    }
})
```

## Box Zone Target

```lua
exports.ox_target:addBoxZone({
    coords = vec3(100.0, 200.0, 30.0),
    size = vec3(2, 2, 2),
    rotation = 0,
    options = {
        {
            name = "use_terminal",
            label = "Use Terminal",
            onSelect = function() end
        }
    }
})
```
