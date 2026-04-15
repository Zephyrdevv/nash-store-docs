# QBOX

QBOX is a fork of QBCore. Nash Banking treats it as a separate detection target because QBOX ships a slightly different core object path.

## Auto-detection

QBOX is picked when `GetResourceState('qbx_core') == 'started'`. QBOX has priority over QBCore in the detection order, so on a hybrid install (`qbx_core` + `qb-core` started together) QBOX wins.

## Core object lookup

The bridge tries `qb-core` first (QBOX ships a compatibility shim), then falls back to `qbx_core`:

```lua
-- Attempt 1: qb-core compat layer
exports['qb-core']:GetCoreObject()

-- Attempt 2: qbx_core direct
exports['qbx_core']:GetCoreObject()
```

If both fail, a warning is printed: `[Nash Banking] Failed to get QBOX object`.

## Differences from QBCore

Once the core object is obtained, the same QBCore code paths are used:

- `xPlayer.Functions.GetMoney('bank' | 'cash')`
- `xPlayer.Functions.AddMoney / RemoveMoney`
- `QBCore.Functions.CreateCallback` / `TriggerCallback`
- `QBCore.Functions.HasPermission(source, 'admin' | 'god')`

See [QBCore compatibility](qbcore.md) for the full mapping.

## Known quirks

- **Pure QBX (no `qb-core`)** — some servers run QBOX without the `qb-core` compat shim. The bridge handles that via the `qbx_core` fallback.
- **`charinfo` shape** — QBOX sometimes stores `charinfo` as a table instead of a string. The bridge normalizes both.
- **Citizen ID format** — uses the same `citizenid` field as QBCore.
