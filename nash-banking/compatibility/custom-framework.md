# Custom Framework

If you use a custom or private framework, you can wire it into Nash Banking by editing the bridge files.

## Files to edit

| File | Purpose |
|---|---|
| `shared/bridge/framework.lua` | Framework detection + fetching your core object |
| `server/bridge.lua` | Server-side API — must implement all `Bridge.*` functions listed below |
| `client/bridge.lua` | Client-side API — player data, callbacks, notifications |

## Minimum required functions (server)

Implement these in `server/bridge.lua`. Switch on `Framework == 'myframework'` and return the expected shape — matching the ESX / QBCore branches is the easiest starting point.

| Function | Returns | Description |
|---|---|---|
| `Bridge.GetPlayer(source)` | table | Player object from server ID |
| `Bridge.GetPlayerByIdentifier(id)` | table | Player object from persistent ID |
| `Bridge.GetIdentifier(xPlayer)` | string | Stable unique ID stored in every DB table |
| `Bridge.GetSource(xPlayer)` | number | Server source ID |
| `Bridge.GetName(xPlayer)` | string | `'Firstname Lastname'` |
| `Bridge.GetFirstName(xPlayer)` | string | Used on cards + UI |
| `Bridge.GetLastName(xPlayer)` | string | Used on cards + UI |
| `Bridge.GetJob(xPlayer)` | string | Job name (for business linking) |
| `Bridge.SetJob(xPlayer, jobName, grade)` | — | Set the player's job |
| `Bridge.GetBankBalance(xPlayer)` | number | — |
| `Bridge.GetCashBalance(xPlayer)` | number | — |
| `Bridge.AddBank(xPlayer, amount, reason)` | — | — |
| `Bridge.RemoveBank(xPlayer, amount, reason)` | — | — |
| `Bridge.AddCash(xPlayer, amount, reason)` | — | — |
| `Bridge.RemoveCash(xPlayer, amount, reason)` | — | — |
| `Bridge.IsAdmin(xPlayer, source)` | boolean | Match against `Config.Admin.Permissions` |
| `Bridge.RegisterCallback(name, cb)` | — | Register a server callback |
| `Bridge.GetOfflineBankBalance(id, cb)` | — | Query offline player's bank balance via SQL |
| `Bridge.SetOfflineBankBalance(id, balance, cb)` | — | Set offline player's bank balance via SQL |
| `Bridge.OfflinePlayerExists(id, cb)` | — | Check if an identifier exists in your players table |

## Minimum required functions (client)

In `client/bridge.lua`:

| Function | Description |
|---|---|
| `Bridge.GetPlayerData()` | Full player data table (money, job, charinfo, …) |
| `Bridge.TriggerCallback(name, cb, ...)` | Trigger a server callback |
| `Bridge.Notify(msg, type)` | Push a toast — `type` ∈ `'success' / 'error' / 'info'` |
| `Bridge.GetFirstName()` / `GetLastName()` / `GetIdentifier()` | Player info helpers |
| `Bridge.GetAccountMoney(accounts, name)` | Read a sub-account (`'bank'`, `'cash'`) |

## Template

Start from an existing branch. For a framework that resembles QBCore, copy the `else` branch of each function:

```lua
function Bridge.GetBankBalance(xPlayer)
    if not xPlayer then return 0 end
    if Framework == 'esx' then
        return xPlayer.getAccount('bank').money
    elseif Framework == 'myframework' then
        return xPlayer.money.bank or 0
    else
        -- existing QBCore/QBOX path
    end
end
```

## Testing your bridge

Minimum checklist after wiring a custom framework:

1. `[Nash Banking] Framework detected: myframework (obj: OK)` prints on server start.
2. `/bank` opens the UI with the correct firstname, lastname and avatar.
3. Deposit → cash goes down, bank goes up. Withdraw does the opposite.
4. `/testTPE` charges 150€ successfully.
5. `Bridge.IsAdmin` returns `true` for an admin player → `/bankadmin` works.
6. A money transfer between two players updates both bank balances and writes two rows in `nash_transactions`.
7. Subscription renewal on an offline player works (`Bridge.GetOfflineBankBalance` / `SetOfflineBankBalance`).
