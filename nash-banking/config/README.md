# Configuration Files Overview

Nash Banking is split into **10 configuration files**, each focused on a specific area. All files are located in `nash_banking/shared/`.

| File | Purpose |
| ---- | ------- |
| [`config.lua`](config-main.md) | Global settings — framework, currency, locale |
| [`config.locations.lua`](config-locations.md) | ATM, bank agency, and NPC positions |
| [`config.cards.lua`](config-cards.md) | Card types, limits, delivery |
| [`config.subscriptions.lua`](config-subscriptions.md) | Subscription tiers, features, highlights |
| [`config.invest.lua`](config-invest.md) | Stock market configuration |
| [`config.crypto.lua`](config-crypto.md) | Crypto market configuration |
| [`config.atm.lua`](config-atm.md) | ATM limits and fees |
| [`config.business.lua`](config-business.md) | Business types, TPE price |
| [`config.features.lua`](config-features.md) | Feature flags (phone notifications, global toggles) |
| [`config.discord.lua`](config-discord.md) | Discord webhooks for logs |

{% hint style="info" %}
All config files are loaded as **shared scripts**, meaning they are available on both client and server.
{% endhint %}
