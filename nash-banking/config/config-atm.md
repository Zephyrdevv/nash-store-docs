# config.atm.lua

ATM detection, PIN security and the ATM map UI (styling of the "Find ATMs" view in the banking NUI).

File: `shared/config.atm.lua`

## Core ATM behavior

```lua
Config.ATM = {
    Enabled = true,
    Models = {
        'prop_atm_01',
        'prop_atm_02',
        'prop_atm_03',
        'prop_fleeca_atm',
    },
    MaxPinAttempts = 3,           -- PIN tries before the card freezes
    FreezeOnMaxAttempts = true,
    ScanRadius = 50.0,            -- ATM prop scan radius (meters)
    InteractionDistance = 1.5,
}
```

## Daily limits per subscription

Daily ATM limits are defined per tier in [`config.subscriptions.lua`](config-subscriptions.md) under `features.maxWithdrawPerDay` and `features.maxWithdrawAmount`. `-1` means unlimited.

Free monthly withdrawals come from `features.freeAtmWithdrawals`.

## ATM map UI

The banking NUI ships with a "Find ATMs" tab rendering Banks + ATMs on an interactive map. All styling is configurable.

```lua
Config.ATMMap = {
    Enabled = true,

    Title = 'DAB & Banques',
    SearchPlaceholder = 'Rechercher...',
    LoadingText = 'Chargement de la carte...',

    BankPopupLabel = 'Agence bancaire',
    ATMPopupLabel = 'Distributeur automatique',
    GPSButtonLabel = 'Poser le GPS',

    -- Marker colors (CSS gradients)
    BankMarkerColor = 'linear-gradient(135deg, #7c3aed, #a855f7)',
    BankMarkerShadow = 'rgba(124, 58, 237, 0.5)',
    ATMMarkerColor = 'linear-gradient(135deg, #2563eb, #3b82f6)',
    ATMMarkerShadow = 'rgba(37, 99, 235, 0.5)',
    PlayerMarkerColor = '#22c55e',

    -- Legend
    BankLegendColor = '#a855f7',
    ATMLegendColor = '#3b82f6',
    PlayerLegendColor = '#22c55e',
    BankLegendLabel = 'Banque',
    ATMLegendLabel = 'DAB',
    PlayerLegendLabel = 'Vous',

    GPSButtonGradient = 'linear-gradient(135deg, #7c3aed, #a855f7)',

    BankMarkerSize = 32,
    ATMMarkerSize = 28,

    DefaultZoom = 1,
    MinZoom = -2,
    MaxZoom = 5,
    FlyToZoom = 3,
}
```

See [Guides › Customize the ATM map](../guides/customize-atm.md) for concrete theming examples.
