# Installation

## Requirements

- ox_lib (Nash Edition)
- ESX or QBCore framework
- oxmysql

## Steps

1. Place `ducky_multicharacter` in your resources directory.
2. Import `ducky_multicharacter.sql` into your database.
3. Add to `server.cfg`:

```
ensure oxmysql
ensure ox_lib
ensure ducky_multicharacter
```

4. Configure character slots and spawn points in `config.lua`.
5. Restart your server.
