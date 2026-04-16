# FAQ

Below you'll find answers to the most frequently asked questions related to setup, usage, and integration. We recommend checking this section before opening a support ticket.

## General

<details>

<summary>Is Nash Banking standalone?</summary>

No. Nash Banking requires a framework (ESX, QBCore or QBOX) for player identity and money accounts. It also depends on `oxmysql`, `ox_lib`, `ox_inventory` and `ox_target` (or `ox_lib` textUI / the default DrawText3D mode). See [Requirements](installation/requirements.md).

</details>

<details>

<summary>Which frameworks are supported?</summary>

ESX, QBCore, and QBOX. See [Compatibility](compatibility/README.md).

</details>

<details>

<summary>Can I use it on a QB-based framework that's not in the list?</summary>

Yes — see [Custom Framework](compatibility/custom-framework.md).

</details>

## Subscriptions

<details>

<summary>How do I grant a subscription to a player for free?</summary>

Three options — admin panel, direct SQL, or `Database.SetSubscription`. See [How to give a subscription](guides/give-subscription.md) for each.

</details>

<details>

<summary>Can I add more subscription tiers?</summary>

Yes — see [How to add a subscription tier](guides/add-subscription-tier.md).

</details>

<details>

<summary>What exactly does each tier unlock?</summary>

| Feature | Standard | Plus | Premium |
|---|---|---|---|
| Price (30 days) | Free | 500€ | 2 000€ |
| Max cards | 1 | 2 | 3 |
| Transfers / day | 5 | 15 | ∞ |
| Max transfer amount | 5 000€ | 25 000€ | ∞ |
| Withdraws / day | 3 | 10 | ∞ |
| Max withdraw amount | 2 000€ | 10 000€ | ∞ |
| Free ATM withdraws / month | 2 | 5 | ∞ |
| NashPoints per 10€ spent | 1 | 2 | 5 |
| Invest (stocks / ETF) | ❌ | ✅ | ✅ |
| Crypto trading | ❌ | ❌ | ✅ |
| Savings APY | 1.00 % | 2.00 % | 4.00 % |
| Priority support | ❌ | ❌ | ✅ |

Full definitions in [`config.subscriptions.lua`](config/config-subscriptions.md).

</details>

## Savings

<details>

<summary>How is the savings interest rate configured?</summary>

Per subscription tier, in `config.subscriptions.lua` → `features.savingsRate`. The rate is expressed as **annual %** (e.g. `2.00` = 2% / year).

</details>

<details>

<summary>When is interest credited?</summary>

Every `Config.Savings.InterestInterval` seconds (default `3600` = 1h). A pro-rated share of the annual rate is applied to the savings balance each tick.

Offline catch-up: when a player reconnects, up to `Config.Savings.MaxOfflinePeriods` missed intervals are applied at once (default 168 = 7 days cap).

</details>

## TPE (Payment Terminal)

<details>

<summary>How do I integrate the TPE into my own shop?</summary>

Call `exports.nash_banking:CardPayment(source, amount, description)` from your server-side checkout logic. Full walk-through in [How to integrate the TPE in a shop](guides/integrate-tpe.md).

</details>

<details>

<summary>What's the difference between `CardPayment` export and the `nash_tpe` item?</summary>

- `CardPayment` server export — the **shop flow**. You pass `(source, amount, description)` and the TPE opens on the buyer. No item required.
- `nash_tpe` inventory item — the **player-to-player flow**. A merchant uses the item in-game, types an amount, the TPE transfers to the nearest player's hand. Uses `nash_banking:p2pInitiate` internally.

</details>

<details>

<summary>Can the TPE route income to a business account?</summary>

Yes — when a merchant uses a `nash_tpe` item with `metadata.business_id` set, the funds are credited to the business balance instead of the merchant's personal account. The bridge verifies the merchant is a registered employee of that business before routing.

Buying a TPE via the business dashboard (`buyBusinessTPE` callback) gives an item with the `business_id` metadata already set.

</details>

## Cards

<details>

<summary>Are PIN / CVV safely stored?</summary>

They are stored as plain strings in `nash_cards` — the resource is designed for roleplay, not real banking. Access to the DB from the game is gated by framework-level checks (owner identifier or admin permission). PINs are never sent to the NUI until the correct player requests them through the authenticated callback.

Do not expose the DB directly to untrusted scripts — treat `nash_cards` with the same caution as any identity table.

</details>

<details>

<summary>How do I enable physical cards?</summary>

See [How to enable physical cards](guides/enable-physical-cards.md).

</details>

<details>

<summary>What happens if a player loses their physical card?</summary>

With `Config.PhysicalCard.FreezeOnLoss = true` (the default), the card is **automatically frozen** in the DB the moment the item is removed from inventory (listening to `ox_inventory:removedItem`). The player can reorder it via the cards page for `Config.PhysicalCard.ReorderCost` (default 500€). `KeepSameNumber = false` regenerates a fresh card number on reorder.

</details>

## Businesses

<details>

<summary>What's the difference between a personal business and a job-linked business?</summary>

- **Personal business** — created by a player via `/bank` → Business tab (when `Config.Business.AllowPlayerCreation = true`). Any player can be made an employee. Transactions are independent from the framework's job system.
- **Job-linked business** — created by an admin via `/bankadmin` with a `job_name`. Employees of the framework job automatically have access at their matching role (boss/manager/employee). When the business receives TPE income with an associated `job_name`, it is credited to the job society account in ESX / `management` in QBCore.

</details>

<details>

<summary>How do employees get paid?</summary>

Nash Banking doesn't manage paychecks — that's the framework's job system. The business dashboard lets the boss move money **between the business balance and their personal bank account** (deposit / withdraw) and see the full TPE income ledger. For recurring wages, trigger a transfer from your paycheck script using `Database.AddBusinessBalance` or by calling the business dashboard endpoints.

</details>

## Phone apps

<details>

<summary>Are the phone apps mandatory?</summary>

No — they are optional resources. The desktop UI is fully featured without them.

</details>

<details>

<summary>How do I disable phone notifications?</summary>

Set `Config.PhoneNotifications = false` in `shared/config.lua`. Toast notifications will then only show in the banking NUI, not on LB Phone.

</details>

## Localization

<details>

<summary>Which languages are shipped?</summary>

French (`fr`) and English (`en`).

</details>

<details>

<summary>Can I add another language?</summary>

See [How to customize locales](guides/customize-locales.md).

</details>

## Performance & security

<details>

<summary>Are there rate limits?</summary>

Yes — all sensitive server events (`nash_banking:tpeConfirm`, `nash_banking:p2pInitiate`, transfers, registrations) are rate-limited at **2 seconds per player** via in-memory `rateLimits` tables. Messaging is capped at `Config.Messaging.RateLimitMs` (default 1000ms). Bank balance checks are re-verified server-side on every money-moving callback — never trust client-side math.

</details>

<details>

<summary>Will the script handle thousands of transactions?</summary>

Yes. Daily counters, active card lookups and subscription expiry checks all have dedicated indexes (see [Database › Indexes](installation/database.md#indexes--performance)). `Config.MaxTransactions` (default 50) caps what is sent to the UI, and `Config.TransactionPurgeDays` (default 90) auto-deletes old rows so the table stays small.

</details>
