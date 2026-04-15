# QBCore

## Supported versions

Tested on current `qb-core` (community fork). The bridge only uses documented `QBCore.Functions.*` APIs so it is resilient to minor version bumps.

## Auto-detection

QBCore is picked when `GetResourceState('qb-core') == 'started'` (after ESX / QBOX are ruled out).

The bridge then calls `exports['qb-core']:GetCoreObject()` and stores the result in `FrameworkObj`.

## Money accounts

| Bridge function | QBCore implementation |
|---|---|
| `Bridge.GetBankBalance` | `xPlayer.Functions.GetMoney('bank')` (fallback: `PlayerData.money.bank`) |
| `Bridge.GetCashBalance` | `xPlayer.Functions.GetMoney('cash')` (fallback: `PlayerData.money.cash`) |
| `Bridge.AddBank`        | `xPlayer.Functions.AddMoney('bank', amount, reason)` |
| `Bridge.RemoveBank`     | `xPlayer.Functions.RemoveMoney('bank', amount, reason)` |
| `Bridge.AddCash`        | `xPlayer.Functions.AddMoney('cash', amount, reason)` |
| `Bridge.RemoveCash`     | `xPlayer.Functions.RemoveMoney('cash', amount, reason)` |

## Player identifier

`Bridge.GetIdentifier` uses `xPlayer.PlayerData.citizenid` (e.g. `JSA12345`).

## Player names

QBCore stores first/last name inside `PlayerData.charinfo`. The bridge decodes it (handles both `table` and `string`/JSON shapes):

```lua
Bridge.GetName(xPlayer)      -- "John Doe"
Bridge.GetFirstName(xPlayer) -- "John"
Bridge.GetLastName(xPlayer)  -- "Doe"
```

Fallback order: `charinfo` table → `charinfo` JSON string → `PlayerData.name` → `'Unknown'`.

## Admin detection

`Bridge.IsAdmin` calls:

```lua
QBCore.Functions.HasPermission(source, 'admin') or
QBCore.Functions.HasPermission(source, 'god')
```

Any player with either permission passes `/bankadmin` and `/resetbank`.

## Callbacks

`Bridge.RegisterCallback` → `QBCore.Functions.CreateCallback`
`Bridge.TriggerCallback` → `QBCore.Functions.TriggerCallback`

## Offline balance lookups

The bridge queries the `players` table:

```sql
SELECT money FROM players WHERE citizenid = ?
```

Then decodes the `money` JSON column and updates the `bank` key on writes.

## Known quirks

- **`charinfo` as JSON string** — some QBCore forks store `charinfo` as a JSON string instead of a table. The bridge detects and decodes both.
- **Async MySQL in callbacks** — Nash Banking uses `MySQL.*.await` (`oxmysql`) for synchronous-style queries in callbacks. Make sure `oxmysql` is started **before** `qb-core` in `server.cfg`.
