# config.business.lua

Business banking (company accounts + TPE terminal item).

File: `shared/config.business.lua`

## Business

```lua
Config.Business = {
    Enabled = true,
    CreationCost = 50000,           -- Cost to create a business
    MaxBusinessesPerPlayer = 1,
    TPECost = 5000,                 -- Cost of a business-linked TPE
    WithdrawFee = 0,                -- 0 = free, 0.05 = 5%
    AccessLevel = 'boss',           -- 'boss' | 'manager' | 'employee' (min role to open the business dashboard)
    NpcModel = 'a_m_y_business_03',
    NpcLocations = {
        { coords = vector4(148.0315, -1041.7035, 29.3679, 340.7635), label = 'Business Bank' },
    },
    Blips = {
        Enabled = true,
        Sprite = 431,
        Color = 46,
        Scale = 0.7,
        Display = 4,
        ShortRange = true,
        Label = 'Business Bank',
    },
    OxTarget = {
        Icon = 'fas fa-briefcase',
        Label = 'Banque d\'entreprise',
        Distance = 2.5,
    },
}
```

### Role hierarchy

When a player opens the business dashboard the bridge compares their role to `AccessLevel`:

| Role | Level |
|---|---|
| `boss` | 3 |
| `manager` | 2 |
| `employee` | 1 |

With `AccessLevel = 'manager'`, both bosses and managers can open the dashboard; employees cannot.

## TPE

```lua
Config.TPE = {
    RequirePhysicalCard = false,     -- Force physical card item in inventory for physical-format cards

    PlayerToPlayer = {
        Enabled = true,
        Item = 'nash_tpe',
        MaxAmount = 50000,
        Fee = 0.02,                  -- 2% fee taken from the seller's receipt
        Distance = 3.0,              -- Max distance to the buyer (meters)
    },
}
```

See [Guides › Create a business](../guides/create-business.md) and [Guides › Integrate TPE](../guides/integrate-tpe.md).
