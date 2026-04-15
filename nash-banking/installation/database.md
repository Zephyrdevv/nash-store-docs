# Database

## Importing the schema

1. Open your database manager (HeidiSQL, phpMyAdmin, DBeaver…).
2. Import `nash_banking/INSTALL/sql/nash_banking.sql` into your FiveM database.
3. Verify the tables were created (16 tables total).

The SQL script uses `CREATE TABLE IF NOT EXISTS` everywhere — running it twice is safe. Compatible with MySQL 5.7+ and MariaDB 10.2+.

## Tables created

16 tables are created by `nash_banking.sql`.

### Core

| Table | Purpose |
|---|---|
| `nash_players` | Registration data per identifier (firstname, lastname, phone, DOB, avatar) |
| `nash_transactions` | Banking ledger — type ∈ { deposit, withdraw, transfer_in, transfer_out, savings_in, savings_out, subscription, purchase, income } |
| `nash_savings` | Savings balance + current rate per player |
| `nash_subscriptions` | Current tier, start date, next charge date, auto_renew flag |
| `nash_cards` | All cards (virtual + physical), with delivery status and per-card spending limits |
| `nash_messages` | In-bank messaging (text / GIF) |
| `nash_daily_counters` | Persistent daily counters (transfers / withdraws per day) |

### Invest

| Table | Purpose |
|---|---|
| `nash_stock_prices` | Current price + 24h change per symbol |
| `nash_stock_history` | Rolling price history (capped by `Config.Invest.HistoryMaxPoints`) |
| `nash_portfolio` | Per-player stock holdings (shares + avg buy price) |

### Crypto

| Table | Purpose |
|---|---|
| `nash_crypto_prices` | Current crypto prices |
| `nash_crypto_history` | Rolling crypto price history |
| `nash_crypto_portfolio` | Per-player crypto holdings |

### Business

| Table | Purpose |
|---|---|
| `nash_businesses` | Businesses (owner, balance, optional `job_name`) |
| `nash_business_employees` | Employees with roles (`boss`, `manager`, `employee`) — FK to `nash_businesses` on cascade |
| `nash_business_transactions` | Business ledger — FK to `nash_businesses` on cascade |

## Indexes & performance

The schema ships with performance indexes out of the box:

| Index | Table | Columns |
|---|---|---|
| `idx_identifier` | `nash_transactions`, `nash_cards`, `nash_portfolio`, `nash_crypto_portfolio` | `identifier` |
| `idx_date` | `nash_transactions`, `nash_business_transactions` | `date` |
| `idx_card_id` | `nash_transactions` | `card_id` |
| `idx_conversation` | `nash_messages` | `(sender_identifier, receiver_identifier, date)` |
| `idx_receiver` | `nash_messages` | `(receiver_identifier, date)` |
| `idx_symbol_date` | `nash_stock_history`, `nash_crypto_history` | `(symbol, recorded_at)` |
| `uk_player_stock` | `nash_portfolio` | unique `(identifier, symbol)` |
| `uk_player_crypto` | `nash_crypto_portfolio` | unique `(identifier, symbol)` |
| `idx_owner` | `nash_businesses` | `owner_identifier` |
| `idx_business` | `nash_business_employees`, `nash_business_transactions` | `business_id` |
| `uk_biz_emp` | `nash_business_employees` | unique `(business_id, identifier)` |
| `idx_counter_date` | `nash_daily_counters` | `date` |
| `idx_daily_spend` | `nash_transactions` | `(card_id, date)` — daily spending lookups |
| `idx_card_active` | `nash_cards` | `(identifier, frozen)` — active card scans |
| `idx_subscription_expiry` | `nash_subscriptions` | `next_charge_at` — due-charge scans |

## Foreign keys

Business tables cascade deletes:

```sql
CONSTRAINT fk_biz_emp_business FOREIGN KEY (business_id) REFERENCES nash_businesses(id) ON DELETE CASCADE
CONSTRAINT fk_biz_tx_business  FOREIGN KEY (business_id) REFERENCES nash_businesses(id) ON DELETE CASCADE
```

Deleting a business automatically removes its employees and transaction history.

## Upgrading from an older version

Migrations are shipped as additional SQL files in `nash_banking/INSTALL/sql/` named `migration_*.sql`. Run them in order.

{% hint style="warning" %}
Always back up your database before running a migration on a production server.
{% endhint %}
