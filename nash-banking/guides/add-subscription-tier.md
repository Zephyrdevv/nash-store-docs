# How to add a subscription tier

All subscription data lives in [`shared/config.subscriptions.lua`](../config/config-subscriptions.md). Adding a tier is a matter of appending one entry to `Config.Subscriptions.Tiers`.

## Editing `config.subscriptions.lua`

Example — a new `black` tier between Plus and Premium:

```lua
Config.Subscriptions = {
    ChargeInterval = 30,
    AllowNegativeBalance = true,
    DowngradeOnFail = false,

    Tiers = {
        -- ... standard, plus, premium ...

        -- New Black tier
        {
            id = 'black',
            name = 'Black',
            price = 1000,
            color = '#000000',
            gradient = 'linear-gradient(135deg, #0a0a0a 0%, #1a1a1a 50%, #2d2d2d 100%)',
            description = 'Black card — limits relevés sans crypto',
            highlights = {
                { icon = 'transfer', text = 'Virements illimités' },
                { icon = 'atm',      text = 'Retraits illimités' },
                { icon = 'star',     text = '3 NashPoints à chaque 10€' },
                { icon = 'invest',   text = 'Accès complet aux investissements' },
                { icon = 'support',  text = 'Support client étendu' },
            },
            features = {
                maxCards = 3,
                maxTransferPerDay = -1,
                maxTransferAmount = -1,
                maxWithdrawPerDay = -1,
                maxWithdrawAmount = -1,
                freeAtmWithdrawals = 10,
                nashPointsMultiplier = 3,
                investAccess = true,
                cryptoAccess = false,
                savingsRate = 3.00,
                prioritySupport = true,
            }
        },
    }
}
```

## `features` — all available keys

| Key | Type | Meaning |
|---|---|---|
| `maxCards` | number | Hard cap on cards (overrides `Config.Cards.MaxCardsPerPlayer`) |
| `maxTransferPerDay` | number | Daily transfer count (`-1` = unlimited) |
| `maxTransferAmount` | number | Max per transfer |
| `maxWithdrawPerDay` | number | Daily withdraw count |
| `maxWithdrawAmount` | number | Max per withdraw |
| `freeAtmWithdrawals` | number | Free ATM withdraws per month |
| `nashPointsMultiplier` | number | Loyalty points per 10€ spent |
| `investAccess` | boolean | Unlocks Invest page |
| `cryptoAccess` | boolean | Unlocks Crypto page |
| `savingsRate` | number | Annual % applied to savings |
| `prioritySupport` | boolean | Cosmetic flag for RP / UI |

## `highlights` — icons & text

Drives the feature list shown in the subscription modal. Each entry is `{ icon = '...', text = '...' }`.

### Available icons

| Key | Component |
|---|---|
| `transfer` | ArrowLeftRight |
| `atm` | Banknote |
| `star` | Star |
| `card` | CreditCard |
| `invest` | TrendingUp |
| `crypto` | Bitcoin |
| `support` | Headphones |
| `shield` | Shield |
| `zap` | Zap |
| `gift` | Gift |
| `percent` | Percent |
| `piggy` | PiggyBank |

Adding a new icon requires editing `web/src/components/subscription/SubscriptionCard.tsx` to map a new key to a Lucide icon, then rebuilding the UI.

## Gradient & color

- `color` — a solid hex used for badges, accents and the tier dot in the header.
- `gradient` — the full CSS background of the tier card. Any valid CSS `background` string works; 3-stop linear gradients match the shipped style best.

## Pricing and billing cycle

- `price` — charged every `Config.Subscriptions.ChargeInterval` days.
- `AllowNegativeBalance = true` — debits the tier even if the bank goes into the red.
- `DowngradeOnFail = true` (with `AllowNegativeBalance = false`) — silently drops the player back to `standard` when they can't pay.

## Restart

After editing the file:

```
restart nash_banking
restart nash_banking_phone
restart nash_businessbanking_phone
```

The new tier appears in the Subscription modal on next `/bank` open.
