# config.features.lua

Savings, messaging, page visibility and per-feature subscription gating.

File: `shared/config.features.lua`

## Savings

```lua
Config.Savings = {
    -- Interest rate lives in Config.Subscriptions.Tiers[].features.savingsRate
    InterestInterval = 3600,    -- Seconds between interest payouts (3600 = 1h)
    OfflineCatchUp = true,      -- Apply missed interest when the player reconnects
    MaxOfflinePeriods = 168,    -- Cap on offline catch-up (168h ≈ 7 days)
}
```

## Messaging

```lua
Config.Messaging = {
    -- Tenor API key lives in server/config.secret.lua (server-only for security)
    MaxMessageLength = 500,
    RateLimitMs = 1000,
    MaxMessagesPerConversation = 100,
    GifSearchLimit = 20,
}
```

## Page visibility

Set any entry to `false` to completely hide the tab from the navigation bar.

```lua
Config.EnabledPages = {
    home       = true,
    invest     = true,
    transfers  = true,
    crypto     = true,
    savings    = true,
    cards      = true,
    messaging  = true,
    atmMap     = true,
}
```

## Subscription gating

Each page / action can be locked behind a subscription tier. Players below the required tier see a lock icon; clicking opens the upgrade popup.

Hierarchy: `standard < plus < premium`.

### Pages

```lua
Config.FeatureAccess = {
    Pages = {
        home       = 'standard',
        invest     = 'plus',
        transfers  = 'standard',
        crypto     = 'premium',
        savings    = 'standard',
    },
    ...
}
```

### Actions

```lua
Config.FeatureAccess = {
    ...
    Actions = {
        deposit      = 'standard',
        withdraw     = 'standard',
        transfer     = 'standard',
        requestMoney = 'standard',
        buyStock     = 'plus',
        sellStock    = 'plus',
        buyCrypto    = 'premium',
        sellCrypto   = 'premium',
        createCard   = 'standard',
        findATM      = 'standard',
    },
}
```
