# Installation

## Requirements

- ox_lib (Nash Edition)
- ESX or QBCore
- ox_inventory (optional)

## Steps

1. Place `smoke_lean` in your resources directory.
2. Add items to your inventory database if using item-based consumption.
3. Add to `server.cfg`:

```
ensure ox_lib
ensure smoke_lean
```

4. Configure items and effects in `config.lua`.
5. Restart server.
