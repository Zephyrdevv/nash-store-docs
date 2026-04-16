# How to setup Discord logs

Nash Banking logs sensitive events to Discord via webhooks. One webhook per category — you can send everything to a single channel or split it across 8 different channels.

## Creating a webhook

1. Open your Discord server → pick (or create) a channel for logs (e.g. `#nash-banking-logs`).
2. **Channel settings → Integrations → Webhooks → New webhook**.
3. Name it (`Nash Banking`), upload an avatar if you like, **Copy webhook URL**.
4. Keep the URL safe — anyone with it can post in that channel.

## Configuring `config.discord.lua`

Paste the webhook URL into the matching field:

```lua
-- shared/config.discord.lua
Config.Discord = {
    Enabled = true,
    BotName = 'Nash Banking',
    BotAvatar = '',

    Webhooks = {
        transactions  = 'https://discord.com/api/webhooks/…/…',
        atm           = '',
        cards         = 'https://discord.com/api/webhooks/…/…',
        subscriptions = 'https://discord.com/api/webhooks/…/…',
        business      = '',
        admin         = 'https://discord.com/api/webhooks/…/…',  -- always worth logging
        tpe           = '',
        invest        = '',
    },
    ...
}
```

Any category with an empty string (`''`) is silently disabled. `Enabled = false` at the top kills all logging regardless of per-category URLs.

## Choosing which events to log

| Category | Fires on |
|---|---|
| `transactions` | Registrations, deposits, withdraws, transfers, money requests |
| `atm` | ATM deposits, withdraws, PIN failures, card freezes |
| `cards` | Card creation, deletion, freeze toggle, PIN change |
| `subscriptions` | Upgrades, cancellations, automatic renewals |
| `business` | Business creation / deletion, employee add / remove, business deposit / withdraw, business TPE purchase |
| `admin` | Every `/bankadmin` action (add money, remove money, create / delete business, grant subscription) |
| `tpe` | Every confirmed TPE payment (shop + P2P) |
| `invest` | Stock / ETF / crypto trades |

**Recommended minimum for a production server**: `admin`, `transactions`, `tpe`.

## Customizing colors & avatars

```lua
BotName = 'Nash Banking',
BotAvatar = 'https://i.imgur.com/YOUR_ICON.png',  -- 256×256 PNG is ideal

Colors = {
    success  = 0x2ecc71,  -- Green
    error    = 0xe74c3c,  -- Red
    warning  = 0xf39c12,  -- Orange
    info     = 0x3498db,  -- Blue
    admin    = 0x9b59b6,  -- Purple
},
```

Colors are hex integers. Use a color picker and prefix with `0x` (Lua) or convert from `#RRGGBB`.

## Rate limits

Discord allows ~30 webhook messages per minute per channel. On a busy server with every category enabled, split them across at least 3 channels (`transactions`, `admin`, everything else) to avoid Discord 429 rate-limit errors.

## Troubleshooting

**Nothing appears in Discord.**

- Check the webhook URL is correct (paste it into a browser — Discord returns a JSON with the webhook info).
- Check `Config.Discord.Enabled = true`.
- Watch the server console for `[Nash Banking] Discord webhook failed: ...`.

**Messages appear but with no embed content.**

- The bot account might be restricted from embedding links in your channel. Channel settings → Permissions → make sure `Embed Links` is allowed for the webhook's default role.
