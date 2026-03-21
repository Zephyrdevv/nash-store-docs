# Installation

## Requirements

- ox_lib (Nash Edition)
- ox_target (Nash Edition)
- ESX or QBCore
- oxmysql

## Steps

1. Place `nash_clothesshop` in your resources directory.
2. Import `nash_clothesshop.sql` into your database.
3. Add to `server.cfg`:

```
ensure oxmysql
ensure ox_lib
ensure ox_target
ensure nash_clothesshop
```

4. Configure shop locations in `config.lua`.
5. Restart your server.
