# config.subscriptions.lua

Subscription tiers and their unlocked features. Limits are enforced server-side. `-1` means unlimited.

File: `shared/config.subscriptions.lua`

## Billing

```lua
Config.Subscriptions = {
    ChargeInterval = 30,             -- Days between automatic charges
    AllowNegativeBalance = true,     -- Debit even if the account goes negative
    DowngradeOnFail = false,         -- When AllowNegativeBalance = false: downgrade on insufficient funds
    Tiers = { ... },
}
```

## Tiers

Three tiers are shipped by default. The `features` block is what the server reads to gate actions and apply limits.

### Standard (free)

```lua
{
    id = 'standard', name = 'Standard', price = 0,
    color = '#3b82f6',
    gradient = 'linear-gradient(135deg, #1e3a5f 0%, #2563eb 50%, #3b82f6 100%)',
    features = {
        maxCards = 1,
        maxTransferPerDay = 5,
        maxTransferAmount = 5000,
        maxWithdrawPerDay = 3,
        maxWithdrawAmount = 2000,
        freeAtmWithdrawals = 2,
        nashPointsMultiplier = 1,
        investAccess = false,
        cryptoAccess = false,
        savingsRate = 1.00,
        prioritySupport = false,
    }
}
```

### Plus (500€ / 30 days)

```lua
{
    id = 'plus', name = 'Plus', price = 500,
    color = '#a855f7',
    gradient = 'linear-gradient(135deg, #581c87 0%, #7c3aed 50%, #a855f7 100%)',
    features = {
        maxCards = 2,
        maxTransferPerDay = 15,
        maxTransferAmount = 25000,
        maxWithdrawPerDay = 10,
        maxWithdrawAmount = 10000,
        freeAtmWithdrawals = 5,
        nashPointsMultiplier = 2,
        investAccess = true,
        cryptoAccess = false,
        savingsRate = 2.00,
        prioritySupport = false,
    }
}
```

### Premium (2 000€ / 30 days)

```lua
{
    id = 'premium', name = 'Premium', price = 2000,
    color = '#D4A017',
    gradient = 'linear-gradient(135deg, #5C4000 0%, #B8860B 50%, #FFD700 100%)',
    features = {
        maxCards = 3,
        maxTransferPerDay = -1,
        maxTransferAmount = -1,
        maxWithdrawPerDay = -1,
        maxWithdrawAmount = -1,
        freeAtmWithdrawals = -1,
        nashPointsMultiplier = 5,
        investAccess = true,
        cryptoAccess = true,
        savingsRate = 4.00,
        prioritySupport = true,
    }
}
```

## Feature glossary

| Key | Type | Meaning |
|---|---|---|
| `maxCards` | number | Maximum number of cards a player can own on this tier |
| `maxTransferPerDay` | number | Daily transfer count cap (`-1` = unlimited) |
| `maxTransferAmount` | number | Max amount per transfer |
| `maxWithdrawPerDay` | number | Daily withdraw count cap |
| `maxWithdrawAmount` | number | Max amount per withdraw |
| `freeAtmWithdrawals` | number | Free ATM withdraws per month, past which `Config.ATM.Fee` applies |
| `nashPointsMultiplier` | number | Loyalty points earned per 10€ spent |
| `investAccess` | boolean | Unlocks the Invest page (stocks / ETFs) |
| `cryptoAccess` | boolean | Unlocks the Crypto page |
| `savingsRate` | number | Annual interest rate on the savings account (%) |
| `prioritySupport` | boolean | Cosmetic flag used by UI / RP integration |

## Highlights

Each tier has a `highlights` array — text lines displayed on the subscription page. Icons available: `transfer`, `atm`, `star`, `card`, `invest`, `crypto`, `support`, `shield`, `zap`, `gift`, `percent`, `piggy`.

See [Guides › Add a subscription tier](../guides/add-subscription-tier.md) to add a fourth tier.
