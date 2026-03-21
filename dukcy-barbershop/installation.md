# Installation

## Requirements

- ox_lib (Nash Edition)
- ox_target (Nash Edition)
- ESX or QBCore framework

## Steps

1. Place `dukcy_barbershop` in your resources directory.
2. Import `dukcy_barbershop.sql` into your database (if provided).
3. Add to `server.cfg`:

```
ensure ox_lib
ensure ox_target
ensure dukcy_barbershop
```

4. Configure locations and prices in `config.lua`.
5. Restart your server.
