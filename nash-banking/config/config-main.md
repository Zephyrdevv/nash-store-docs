# config.lua

General settings — bank name, framework, locale, currency, NPC interaction, blips, delivery, admin, LB Phone integration.

File: `shared/config.lua`

## General

| Key | Default | Description |
|---|---|---|
| `Config.Framework` | `'auto'` | `'auto'` (detect), `'esx'`, `'qbcore'`, `'qbox'` |
| `Config.Locale` | `'fr'` | Default language (`'fr'` or `'en'`) |
| `Config.BankName` | `'Nash Banking'` | Name shown in UI, cards, ATM |
| `Config.Command` | `'bank'` | Chat command to open the bank |
| `Config.Currency` | `'€'` | Currency symbol in the UI |
| `Config.CurrencyCode` | `'EUR'` | ISO code (shown on card back) |
| `Config.MaxTransactions` | `50` | Max transactions in the history |
| `Config.PhoneNotifications` | `true` | Mirror banking notifications to LB Phone |
| `Config.NpcModel` | `'a_m_y_business_01'` | Ped model for banker NPCs |
| `Config.TransactionPurgeDays` | `90` | Auto-delete transactions older than N days (0 = disabled) |

## Interaction

`Config.InteractionType` — how players start talking to the banker NPC:

- `'default'` → DrawText3D + `E` key (no dependency)
- `'ox_target'` → `ox_target` zone on the ped (recommended, near-zero resmon)
- `'ox_lib'` → `ox_lib` textUI + `E` key

```lua
Config.InteractionType = 'ox_target'
Config.InteractionDistance = 2.5

Config.OxTarget = {
    Icon = 'fas fa-university',
    Label = 'Accéder à la banque',
    Distance = 2.5,
}

Config.OxLib = {
    Text = '[E] Accéder à la banque',
    Icon = 'university',
    Position = 'right-center',
    Style = {},
}
```

## Blips

```lua
Config.Blips = {
    Enabled = true,
    Sprite = 108,   -- 108 = bank icon
    Color = 2,      -- 2 = green
    Scale = 0.8,
    Display = 4,    -- 4 = map + minimap
    ShortRange = true,
}
```

## Card delivery

```lua
Config.Delivery = {
    Cost = 0,                         -- 0 = free
    Delay = 0,                        -- seconds before card is ready (0 = instant)
    Item = 'nash_card_physical',
    NpcModel = 'a_m_y_business_01',
    Blip = { Enabled = true, Sprite = 478, Color = 5, Scale = 0.7, Display = 4, ShortRange = true },
    OxTarget = { Icon = 'fas fa-box', Label = 'Récupérer un colis', Distance = 2.5 },
    OxLib    = { Text = '[E] Récupérer un colis', Icon = 'box', Position = 'right-center', Style = {} },
}
```

## Admin

```lua
Config.Admin = {
    Command = 'bankadmin',
    Permissions = { 'admin', 'superadmin' },  -- Ace permissions / QBCore groups
}
```

## LB Phone

Requires the `nash_banking_phone` resource + LB Phone.

```lua
Config.LBPhone = {
    Enabled = true,
    AppName = 'Nash Banking',
    DefaultApp = true, -- Pre-installed; false = downloadable from the app store
}
```
