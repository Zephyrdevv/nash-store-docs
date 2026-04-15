# config.discord.lua

Discord webhook logging — one webhook per event category.

File: `shared/config.discord.lua`

## Setup

```lua
Config.Discord = {
    Enabled = true,

    BotName = 'Nash Banking',
    BotAvatar = '',  -- URL of the avatar (empty = Discord default)

    Webhooks = {
        transactions  = '',  -- Deposits, withdraws, transfers, registrations
        atm           = '',  -- ATM operations, PIN failures
        cards         = '',  -- Card creation, deletion, freeze, PIN change
        subscriptions = '',  -- Upgrade, cancellation, renewal
        business      = '',  -- Business creation, deposit, withdraw, employees, TPE
        admin         = '',  -- Admin actions (money, business management)
        tpe           = '',  -- TPE payments + P2P
        invest        = '',  -- Stocks / ETF / crypto trades
    },

    Colors = {
        success  = 0x2ecc71,  -- Green
        error    = 0xe74c3c,  -- Red
        warning  = 0xf39c12,  -- Orange
        info     = 0x3498db,  -- Blue
        admin    = 0x9b59b6,  -- Purple
    },
}
```

## Creating a webhook

1. Discord → channel settings → **Integrations** → **Webhooks** → **New webhook**.
2. Copy the URL.
3. Paste it into the matching category in `Config.Discord.Webhooks`.
4. Leave a field empty (`''`) to disable logging for that category.

See [Guides › Set up Discord logs](../guides/setup-discord-logs.md) for a full walkthrough.
