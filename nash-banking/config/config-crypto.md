# config.crypto.lua

Simulated crypto market — 8 pairs shipped, faster update cadence than stocks.

File: `shared/config.crypto.lua`

## Engine

```lua
Config.Crypto = {
    UpdateInterval = 30,           -- Seconds between updates (faster than invest)
    HistoryMaxPoints = 500,
    MeanReversionFactor = 0.008,
    PriceFloorPct = 0.5,
    PriceCeilPct = 1.5,

    MarketImpact = {
        Enabled = false,
        BuyImpactPct = 0.003,
        SellImpactPct = 0.003,
        MaxImpactPct = 0.08,
    },

    Cryptos = { ... }
}
```

## Pairs (default)

| Symbol | Name | Base price | Volatility | Color | Logo | Badge |
|---|---|---|---|---|---|---|
| BTC | Bitcoin | 97 854 | 0.035 | `#F7931A` | `₿` | TOP 1 |
| ETH | Ethereum | 3 456 | 0.04 | `#627EEA` | `Ξ` | TOP 2 |
| SOL | Solana | 175 | 0.06 | `#9945FF` | `◎` | TOP 5 |
| XRP | Ripple | 2.34 | 0.05 | `#23292F` | `✕` | — |
| DOGE | Dogecoin | 0.42 | 0.08 | `#C2A633` | `Ð` | — |
| ADA | Cardano | 1.12 | 0.05 | `#0033AD` | `₳` | — |
| DOT | Polkadot | 8.92 | 0.05 | `#E6007A` | `●` | — |
| AVAX | Avalanche | 42.15 | 0.055 | `#E84142` | `A` | — |

Each entry:

```lua
{ symbol = 'BTC', name = 'Bitcoin', basePrice = 97854, volatility = 0.035,
  color = '#F7931A', logo = '₿', badge = 'TOP 1' }
```

Access is gated by `features.cryptoAccess` (Premium-only by default).
