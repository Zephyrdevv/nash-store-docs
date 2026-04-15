# Compatibility Overview

Nash Banking supports three frameworks out of the box through a unified **bridge** system.

## Supported frameworks

- [ESX](esx.md) — legacy and 1.x
- [QBCore](qbcore.md)
- [QBOX](qbox.md)

## How the bridge works

The bridge auto-detects your framework at runtime and exposes a unified API (`Bridge.GetPlayer`, `Bridge.GetBankBalance`, `Bridge.AddBank`, etc.) used everywhere in the resource.

You can override auto-detection in `shared/config.lua`:

```lua
Config.Framework = 'auto' -- 'auto' | 'esx' | 'qbcore' | 'qbox'
```

## Extending the bridge

If you use a custom framework, see [Custom Framework](custom-framework.md).
