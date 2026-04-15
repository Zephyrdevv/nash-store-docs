# ESX

## Supported versions

Nash Banking is tested on:

- **ESX Legacy** (1.10+)
- **ESX 1.x** (the current maintained fork on GitHub)

Both use the `es_extended` resource and `exports['es_extended']:getSharedObject()`.

## Auto-detection

ESX is picked when `GetResourceState('es_extended') == 'started'` (or not `'missing'` as a fallback).

The bridge then calls `exports['es_extended']:getSharedObject()` via `pcall` and stores the result in `FrameworkObj`.

## Money accounts

| Bridge function | ESX implementation |
|---|---|
| `Bridge.GetBankBalance` | `xPlayer.getAccount('bank').money` |
| `Bridge.GetCashBalance` | `xPlayer.getMoney()` |
| `Bridge.AddBank`        | `xPlayer.addAccountMoney('bank', amount, reason)` |
| `Bridge.RemoveBank`     | `xPlayer.removeAccountMoney('bank', amount, reason)` |
| `Bridge.AddCash`        | `xPlayer.addMoney(amount, reason)` |
| `Bridge.RemoveCash`     | `xPlayer.removeMoney(amount, reason)` |

## Player identifier

The bridge uses `xPlayer.identifier` (e.g. `license:abc123…`) as the canonical identifier stored in every Nash Banking table.

## Names

- `Bridge.GetName(xPlayer)` → `xPlayer.getName()`
- `Bridge.GetFirstName(xPlayer)` → `xPlayer.get('firstName')`
- `Bridge.GetLastName(xPlayer)` → `xPlayer.get('lastName')`

## Admin detection

`Bridge.IsAdmin` calls `xPlayer.getGroup()` and matches it against `Config.Admin.Permissions` (default `{ 'admin', 'superadmin' }`).

## Offline balance lookups

For subscription renewals and admin actions on offline players, the bridge queries `users.accounts` (JSON column):

```sql
SELECT accounts FROM users WHERE identifier = ?
```

## Callbacks

`Bridge.RegisterCallback` → `ESX.RegisterServerCallback`
`Bridge.TriggerCallback` → `ESX.TriggerServerCallback`

## Known quirks

- **Legacy vs 1.x** — both are supported; the bridge uses the API available on both (`getAccount('bank').money` and `getMoney()`).
- **`xPlayer.get(...)` returns `nil`** on very old ESX forks — the bridge falls back to `'Unknown' / 'Player'` strings so the UI never breaks.
