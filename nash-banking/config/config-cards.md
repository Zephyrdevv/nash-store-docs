# config.cards.lua

Card types, payment limits and physical-card behavior.

File: `shared/config.cards.lua`

## Basics

```lua
Config.Cards = {
    AutoCreateOnRegister = true,   -- Give a default card on registration
    DefaultType = 'standard',
    MaxCardsPerPlayer = 3,         -- Hard cap (per-tier cap in Config.Subscriptions)
    DefaultPin = '0000',
    Types = { ... },
}
```

## Types

```lua
Types = {
    { id = 'standard', name = 'Standard', description = 'Carte virtuelle gratuite',
      color = 'linear-gradient(135deg, #1a1a2e 0%, #2d2d44 100%)', icon = 'S' },
    { id = 'plus',     name = 'Plus',     description = 'Carte Plus avec avantages etendus',
      color = 'linear-gradient(135deg, #581c87 0%, #7c3aed 50%, #a855f7 100%)', icon = 'P' },
    { id = 'premium',  name = 'Premium',  description = 'Carte premium avec avantages exclusifs',
      color = 'linear-gradient(135deg, #B8860B 0%, #FFD700 50%, #B8860B 100%)', icon = 'G' },
}
```

## Payment limits (per card tier)

```lua
Config.Cards.Limits = {
    standard = { dailyLimit = 5000,  singleLimit = 2000 },
    plus     = { dailyLimit = 15000, singleLimit = 10000 },
    premium  = { dailyLimit = -1,    singleLimit = -1 },     -- -1 = unlimited
}
```

- `dailyLimit` — total daily spending via TPE / card payments.
- `singleLimit` — max per single transaction.

## Spending-limit mode

```lua
Config.Cards.SpendingLimit = {
    BySubscription = false,
    DefaultLimit = 5000,
}
```

- `BySubscription = true` → limits come from `Config.Cards.Limits`, **not** editable by players.
- `BySubscription = false` → players can enable/disable and set their own monthly limit per card (default: `DefaultLimit`).

## Physical card

```lua
Config.PhysicalCard = {
    FreezeOnLoss = true,     -- When the item is removed from inventory, freeze the card in DB
    AllowReorder = true,
    ReorderCost = 500,
    KeepSameNumber = false,  -- true = reorder keeps the same number, false = regenerate
}
```

See [Guides › Customize cards](../guides/customize-cards.md) for adding new tiers.
