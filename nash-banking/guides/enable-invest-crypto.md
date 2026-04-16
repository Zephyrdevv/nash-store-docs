# How to enable Invest & Crypto

Invest (stocks / ETFs) and Crypto are full-featured simulated markets shipped with Nash Banking. Both are **enabled by default** but gated behind subscription tiers.

## Enabling the pages

Two levers:

### 1. Page visibility (hard off)

```lua
-- shared/config.features.lua
Config.EnabledPages = {
    invest = true,   -- false hides the tab entirely for everyone
    crypto = true,
    -- ...
}
```

Setting to `false` removes the tab from the nav — no player can access it, regardless of their subscription.

### 2. Subscription gating (soft lock)

```lua
Config.FeatureAccess = {
    Pages = {
        invest = 'plus',       -- requires Plus or higher
        crypto = 'premium',    -- requires Premium
        -- ...
    },
    Actions = {
        buyStock  = 'plus',
        sellStock = 'plus',
        buyCrypto = 'premium',
        sellCrypto = 'premium',
    },
}
```

Players below the required tier see a lock icon. Clicking it opens the upgrade popup.

The per-tier `features.investAccess` / `features.cryptoAccess` booleans (in `config.subscriptions.lua`) are the final server-side check — even if `FeatureAccess` is wide open, a player without `investAccess = true` gets rejected on `nash_banking:buyStock`.

## Adding stock tickers

Append to `Config.Invest.Stocks`:

```lua
Config.Invest.Stocks = {
    -- existing 12 entries ...
    -- New: GME meme-stock
    { symbol = 'GME', name = 'GameStop Corp', basePrice = 22, volatility = 0.09,
      logo = 'G', color = '#DB0019', category = 'action' },
}
```

Fields:

- `symbol` — must be unique, max 10 chars, used as the DB key in `nash_stock_prices`.
- `basePrice` — mean-reversion target.
- `volatility` — σ per tick (0.09 = 9%, very volatile).
- `logo` — single char / emoji shown on the card.
- `color` — hex, used in the chart line and badge.
- `category` — `'action'` or `'etf'` for UI filters.

The new ticker seeds at `basePrice` on the next price tick. Existing portfolios are unaffected.

## Adding crypto currencies

Same pattern for `Config.Crypto.Cryptos`:

```lua
{ symbol = 'MATIC', name = 'Polygon', basePrice = 0.74, volatility = 0.06,
  color = '#8247E5', logo = '◆', badge = 'L2' },
```

Crypto has one extra optional field:

- `badge` — short label shown as a chip ("TOP 1", "L2", "MEME", …).

## Tuning volatility

- **`Config.Invest.MeanReversionFactor`** (default `0.01`) — how fast prices drift back to `basePrice` after a shock. Higher = less drift from baseline.
- **`Config.Invest.PriceFloorPct` / `PriceCeilPct`** (default `0.5` / `1.5`) — hard price rails at ±50% of `basePrice`. Prevents runaway markets over long sessions.
- **`Config.Invest.MarketImpact.Enabled`** (default `false`) — set `true` to let player trades nudge the price. Useful on small servers for a more emergent feel; on big servers turn off to avoid whales manipulating price.

Crypto has the same knobs under `Config.Crypto.*` with slightly more aggressive defaults (faster ticks, lower mean reversion).

## Market hours & update intervals

There are no market hours — the markets are always open. Update cadence:

```lua
Config.Invest.UpdateInterval = 60   -- seconds (default 1 min for stocks)
Config.Crypto.UpdateInterval = 30   -- seconds (default 30s for crypto)
```

Lowering these makes charts feel more alive but writes more rows to `nash_stock_history` / `nash_crypto_history`. The history is capped at `HistoryMaxPoints` (default 500) per symbol — older rows are pruned automatically.

## Disabling one market

To run Invest but no Crypto:

```lua
-- Option A: hide the page
Config.EnabledPages.crypto = false

-- Option B: revoke at the subscription level
Config.Subscriptions.Tiers[3].features.cryptoAccess = false  -- Premium
```

Option A is cleaner — no tab, no temptation.

## Restart

After editing `config.invest.lua` / `config.crypto.lua`:

```
restart nash_banking
```

The server reseeds the stock / crypto price tables on boot if new symbols are detected.
