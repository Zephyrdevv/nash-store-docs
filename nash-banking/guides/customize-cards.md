# How to customize cards

All card customization lives in [`shared/config.cards.lua`](../config/config-cards.md).

## Changing gradients and colors

Each card tier has a CSS-like `color` (gradient) and a single-character `icon`:

```lua
Config.Cards.Types = {
    {
        id = 'standard',
        name = 'Standard',
        description = 'Basic virtual card',
        color = 'linear-gradient(135deg, #1a1a2e 0%, #2d2d44 100%)',
        icon = 'S',
    },
    {
        id = 'plus',
        name = 'Plus',
        color = 'linear-gradient(135deg, #581c87 0%, #7c3aed 50%, #a855f7 100%)',
        icon = 'P',
    },
    {
        id = 'premium',
        name = 'Premium',
        color = 'linear-gradient(135deg, #B8860B 0%, #FFD700 50%, #B8860B 100%)',
        icon = 'G',
    },
}
```

The UI renders the gradient on both the desktop card view and the phone card view. Any valid CSS `background` string works: `linear-gradient`, `radial-gradient`, hex, `rgba`.

## Changing card limits (single / daily)

Per-tier caps on TPE payments:

```lua
Config.Cards.Limits = {
    standard = { dailyLimit = 5000,  singleLimit = 2000 },
    plus     = { dailyLimit = 15000, singleLimit = 10000 },
    premium  = { dailyLimit = -1,    singleLimit = -1 },  -- unlimited
}
```

- `dailyLimit` — rolling 24h spend cap.
- `singleLimit` — max per individual payment.
- `-1` = unlimited.

Limits are checked server-side at `nash_banking:tpeConfirm` — a client that patches the UI cannot bypass them.

## Player-defined monthly limits

If you want each player to set their own limit per card instead of tier-based caps:

```lua
Config.Cards.SpendingLimit = {
    BySubscription = false,  -- switch to player mode
    DefaultLimit = 5000,     -- default when the player enables it
}
```

With `BySubscription = false`:

- The UI shows a "Spending limit" toggle + slider on each card.
- The player-set limit is stored in `nash_cards.spending_limit` and checked on every payment.
- Tier-based `Config.Cards.Limits` is ignored.

## Enabling / disabling virtual or physical cards

Players can order both types out of the box. To **force virtual-only**:

```lua
Config.PhysicalCard = {
    FreezeOnLoss = true,
    AllowReorder = false,   -- hides the reorder button
    ReorderCost = 500,
    KeepSameNumber = false,
}
```

Then either hide the "Physical" button in the card creation modal (edit `web/src/components/cards/CreateCardModal.tsx` — remove the physical option) or reject the request server-side in `server/main.lua`:

```lua
-- at the top of nash_banking:createCard
if format == 'physical' then cb(false, 'physical_disabled') return end
```

For **physical-only** (no virtual), swap the condition.

## Adding a new card tier

1. Append an entry to `Config.Cards.Types`:

    ```lua
    {
        id = 'black',
        name = 'Black',
        description = 'Invitation only',
        color = 'linear-gradient(135deg, #000 0%, #111 100%)',
        icon = 'B',
    }
    ```

2. Add matching limits in `Config.Cards.Limits`:

    ```lua
    Config.Cards.Limits.black = { dailyLimit = -1, singleLimit = -1 }
    ```

3. If the tier should be gated by a subscription, also add it to `Config.Subscriptions.Tiers` (see [How to add a subscription tier](add-subscription-tier.md)).

4. Restart the three resources.
