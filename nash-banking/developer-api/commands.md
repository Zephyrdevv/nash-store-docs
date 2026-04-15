# Commands

Chat commands shipped with Nash Banking.

## Player commands

<details>

<summary>/bank</summary>

Opens the banking NUI. Command name is configurable via `Config.Command`.

```
/bank
```

**Permissions** — none (available to all players).

**Equivalent** — `exports.nash_banking:openBank()` or interacting with a bank NPC.

</details>

## Admin commands

<details>

<summary>/bankadmin</summary>

Opens the admin dashboard (global stats, leaderboards, player search, business management).

```
/bankadmin
```

**Permissions** — matches `Config.Admin.Permissions` (default: `{ 'admin', 'superadmin' }`).

**Command name** — configurable via `Config.Admin.Command`.

</details>

<details>

<summary>/resetbank</summary>

Resets the executing admin's own banking registration (used to retest the onboarding flow in development). **Does not** affect other players.

```
/resetbank
```

**Permissions** — `Bridge.IsAdmin(xPlayer, source)` must return `true` (same check as `/bankadmin`).

</details>

## Debug / utility commands

<details>

<summary>/testTPE</summary>

Triggers `exports.nash_banking:CardPayment` on the caller with a 150€ test payment. Useful to quickly verify the TPE flow during integration.

```
/testTPE
```

**Permissions** — none, but can't be run from the server console (`source == 0` is rejected).

**Implementation** — [`server/main.lua:1722`](../../) calls `exports['nash_banking']:CardPayment(source, 150, 'Test TPE - Superette')`.

</details>

## Adding your own command

Nash Banking uses vanilla `RegisterCommand`. To add your own admin command on top of the banking API:

```lua
RegisterCommand('givecard', function(source, args)
    local xPlayer = Bridge.GetPlayer(source)
    if not xPlayer or not Bridge.IsAdmin(xPlayer, source) then return end

    local targetId = tonumber(args[1])
    local cardType = args[2] or 'standard'

    -- Your logic here (e.g. Database.CreateCard for the target)
end, false)
```

See [Developer API](README.md) for the full list of exports and callbacks.
