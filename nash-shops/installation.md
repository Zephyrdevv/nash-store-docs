# Installation

## Requirements

- ox_lib (Nash Edition)
- ox_target (Nash Edition)
- ox_inventory
- ESX or QBCore

## Steps

1. Place `nash_shops` in your resources directory.
2. Add to `server.cfg`:

```
ensure ox_lib
ensure ox_target
ensure ox_inventory
ensure nash_shops
```

3. Configure shop types and items in `config.lua`.
4. Restart your server.
