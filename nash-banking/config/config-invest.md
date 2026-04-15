# config.invest.lua

Simulated stock market — 12 stocks / ETFs shipped, mean-reversion price engine.

File: `shared/config.invest.lua`

## Engine

```lua
Config.Invest = {
    UpdateInterval = 60,           -- Seconds between price updates
    HistoryMaxPoints = 500,        -- Max history points kept per symbol
    MeanReversionFactor = 0.01,    -- Pulls price back toward basePrice
    PriceFloorPct = 0.5,           -- Hard floor: 50% of basePrice
    PriceCeilPct = 1.5,            -- Hard ceil: 150% of basePrice

    MarketImpact = {
        Enabled = false,
        BuyImpactPct = 0.002,      -- % price move per $1000 bought
        SellImpactPct = 0.002,
        MaxImpactPct = 0.05,       -- Max 5% impact per trade
    },

    Stocks = { ... }
}
```

## Stocks & ETFs (default)

| Symbol | Name | Base price | Volatility | Category |
|---|---|---|---|---|
| NVDA | NVIDIA Corp | 875 | 0.03 | action |
| AAPL | Apple Inc | 227 | 0.02 | action |
| TSLA | Tesla Inc | 248 | 0.04 | action |
| NFLX | Netflix Inc | 897 | 0.02 | action |
| AMZN | Amazon.com | 205 | 0.025 | action |
| MSFT | Microsoft Corp | 415 | 0.02 | action |
| SPY | S&P 500 ETF | 587 | 0.015 | etf |
| QQQ | Nasdaq 100 ETF | 503 | 0.02 | etf |
| VTI | Vanguard Total | 275 | 0.015 | etf |
| ARKK | ARK Innovation | 52 | 0.05 | etf |
| IVV | iShares Core S&P | 589 | 0.015 | etf |
| VOO | Vanguard S&P 500 | 540 | 0.015 | etf |

Each entry:

```lua
{ symbol = 'NVDA', name = 'NVIDIA Corp', basePrice = 875, volatility = 0.03,
  logo = '⬢', color = '#76B900', category = 'action' }
```

- `volatility` — standard deviation per tick (0.03 = 3% σ).
- `logo` — single glyph or short string shown in the UI.
- `color` — hex CSS color used in charts and badges.
- `category` — `'action'` or `'etf'` for UI filters.

Access is gated by `features.investAccess` (Plus+ by default) — see [config.subscriptions.lua](config-subscriptions.md).
