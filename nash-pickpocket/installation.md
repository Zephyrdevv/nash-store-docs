# Installation

## Prerequisites

- es_extended (ESX Framework)
- ox_lib — Utility library
- ox_inventory — Inventory system
- ox_target — Targeting system

## Optional

- p_mdt — Police MDT dispatch alerts

## Steps

1. Download the resource and place it in `resources/`.

2. Register custom items in `ox_inventory/data/items.lua`:

```lua
["phone"]  = { label = "Phone",  weight = 150, stack = false },
["wallet"] = { label = "Wallet", weight = 100, stack = false },
["watch"]  = { label = "Watch",  weight = 200, stack = false },
["ring"]   = { label = "Ring",   weight = 50,  stack = true  },
```

3. Ensure dependencies in `server.cfg`:

```cfg
ensure es_extended
ensure ox_lib
ensure ox_inventory
ensure ox_target
ensure nash_pickpocket
```

4. Restart your server and test by approaching an NPC at night.

> No SQL import is required.
