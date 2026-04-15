# Compatibility Overview

Nash Banking supports three frameworks out of the box through a unified **bridge** system.

## Supported frameworks

- [ESX](esx.md) — `es_extended` (legacy + 1.x)
- [QBCore](qbcore.md) — `qb-core`
- [QBOX](qbox.md) — `qbx_core`

## How the bridge works

Three files make up the bridge:

| File | Purpose |
|---|---|
| `shared/bridge/framework.lua` | Auto-detection + fetch of the framework object |
| `server/bridge.lua` | Unified server-side API (`Bridge.GetPlayer`, `Bridge.AddBank`, `Bridge.RegisterCallback`, …) |
| `client/bridge.lua` | Unified client-side API (`Bridge.TriggerCallback`, `Bridge.Notify`, `Bridge.GetPlayerData`, …) |

Every server / client script uses `Bridge.*` instead of calling ESX / QBCore directly — the bridge dispatches to the right framework at runtime.

## Detection order

`shared/bridge/framework.lua` runs on both client and server at resource start:

1. If `Config.Framework` is `'esx'`, `'qbcore'` or `'qbox'`, use it directly (skip detection).
2. Otherwise (`'auto'`, default):
   1. `es_extended` started → `esx`
   2. `qbx_core` started → `qbox`
   3. `qb-core` started → `qbcore`
3. If none is started yet, the same checks are run against `GetResourceState() ~= 'missing'` as a fallback.
4. If no framework is found → prints `[Nash Banking] No supported framework detected`.

```lua
-- shared/config.lua
Config.Framework = 'auto' -- 'auto' | 'esx' | 'qbcore' | 'qbox'
```

## Extending the bridge

If you use a custom framework, see [Custom Framework](custom-framework.md).
