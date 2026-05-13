# Changelog

All notable changes to Nash Banking are documented here.

The format follows [Keep a Changelog](https://keepachangelog.com/) and the project adheres to [Semantic Versioning](https://semver.org/).

---

## [1.0.0] — Initial release

The first public release of Nash Banking.

### Banking UI

- Personal banking dashboard with 5 navigable pages: **Home**, **Savings**, **Transfers**, **Crypto**, **Invest**
- **Profile page** with personal info, custom avatar (URL) and subscription management
- **Accounts modal** — consolidated view of every balance (main bank, cash, savings, per-card subtotals)
- **Analytics modal** — deposits / withdrawals stats with charts
- **Bank Info modal** — banking information for the player
- **ATM map** — interactive Leaflet map showing every bank branch and ATM, with searchable sidebar and GPS-waypoint integration

### Card system

- Three card tiers — **Standard**, **Plus**, **Premium** — each with its own gradient and limits
- **Virtual** and **physical** card formats, with optional delivery point pickup
- Per-card freeze / unfreeze
- PIN management (with 3-strikes auto-freeze on the ATM)
- Player-defined monthly spending limit per card (toggle in config: `BySubscription = false`)
- **Per-card transaction history / metadata** — each card tracks its own activity
- Physical card item (`nash_card_physical`) with inspect overlay when used from inventory
- Automatic freeze on item loss (configurable)

### Realistic ATM

- Detects vanilla GTA Fleeca ATMs + supports custom standalone ATM locations
- Card insertion via wallet UI (pick which card to use)
- **3D DUI screen** rendered on the ATM prop with a working PIN keypad
- Withdraw / Deposit flow with per-card daily limits
- Per-month free withdrawal count from the subscription tier

### Player-to-Player TPE

- TPE item (`nash_tpe`) — seller takes the terminal, enters the amount, hands it to the buyer
- Buyer picks a card in their wallet, drag & drop on the TPE to confirm
- Notifications + animations on both sides
- Configurable 2% transaction fee for the seller
- Business-linked TPEs route income to the business balance (with optional society sync for job-linked businesses)

### Pluggable shop integration

- `exports.nash_banking:CardPayment(source, amount, description)` — any shop / job / payment script on the server can route card payments through Nash Banking
- Daily and single-transaction spending limits enforced server-side
- Transactions appear in the player's banking history with the calling script's description
- Includes a working integration with `nash_shops` as a reference

### Business banking

- Per-business balance and transaction ledger
- Three roles: Boss, Manager, Employee (configurable `AccessLevel`)
- Personal businesses (player-created) and job-linked businesses (admin-created, tied to a framework job)
- Boss can purchase a business-routed TPE
- Add / remove employees by player server ID (auto-resolves to identifier)

### Subscriptions

- Three tiers — Standard (free), Plus (500€ / 30 days), Premium (2000€ / 30 days)
- Each tier unlocks: daily transfer / withdraw limits, max amounts, max number of cards, NashPoints multiplier, savings rate, Invest access, Crypto access, priority support
- Auto-renewal with configurable `ChargeInterval` and downgrade-on-fail behavior
- Live subscription comparison modal in the Profile page

### Savings

- Configurable interest rate **per subscription tier**
- Periodic interest (default: hourly cycle)
- Offline catch-up on reconnection (cap configurable, default 7 days)
- Dedicated Savings page in the UI

### Invest (stocks / ETFs)

- 12 default tickers (NVDA, AAPL, TSLA, NFLX, AMZN, MSFT, SPY, QQQ, VTI, ARKK, IVV, VOO)
- Mean-reversion price engine with configurable volatility
- Live charts per asset
- Buy / sell / portfolio
- Subscription-gated (Plus+ by default)

### Crypto

- 8 default pairs (BTC, ETH, SOL, XRP, DOGE, ADA, DOT, AVAX)
- Faster update cadence than stocks
- Same buy / sell / portfolio flow
- Subscription-gated (Premium-only by default)

### Messaging

- In-bank chat between players (text + GIF)
- Giphy integration with searchable picker
- Per-conversation history with swipeable actions
- Server-side rate-limited

### Admin panel

- Command: `/bankadmin` (configurable)
- Global stats — money in circulation, total businesses, registered players
- Top players and top businesses leaderboards
- Player search (by server ID, identifier or name)
- Add / Remove money for any player or business
- Create and delete businesses
- **Demo Mode** flag for promotional video / screenshots (returns mock data without touching the DB)

### LB Phone companion apps

- **Personal app** — home, cards, transfers (with messaging) in a phone-optimized UI
- **Business app** — balance, employees, transactions
- Both apps fully localized (FR / EN)
- Configurable app icon, name, and default-installed flag

### Multi-framework support

- Built-in bridge — auto-detects ESX, QBCore, or QBOX
- Single API used internally by every server / client script (`Bridge.*`)
- Documentation includes a guide for wiring a custom framework

### Localization

- French and English shipped by default
- Two parallel locale systems (Lua + React) for full UI coverage
- All notifications, transaction descriptions, target labels, and UI strings are translatable

### Discord webhook logging

- 8 separate categories (transactions, ATM, cards, subscriptions, business, admin, TPE, invest)
- One webhook per category — split the load across multiple channels
- Per-event-type embed colors

### Technical

- Performance indexes on every hot table (`nash_transactions.idx_daily_spend`, `nash_cards.idx_card_active`, etc.)
- Configurable transaction auto-purge (default: 90 days)
- Streamed bank card and TPE props (no extra resource required)
- DUI-based 3D ATM screen
- Native idle-camera blocker while menus are open (configurable)
- Rate-limited server events on every sensitive endpoint (2 second cooldown per player)
