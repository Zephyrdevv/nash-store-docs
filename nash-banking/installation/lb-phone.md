# LB Phone Apps (optional)

Nash Banking ships two LB Phone apps:

- **`nash_banking_phone`** — personal banking app
- **`nash_businessbanking_phone`** — business banking app

Both are independent resources that depend on `nash_banking` being started first (they import `@nash_banking/shared/bridge/framework.lua` and `@nash_banking/client/bridge.lua`).

## Requirements

- `lb-phone` resource started
- `nash_banking` started before the phone apps

## Installing an app

1. Drop `nash_banking_phone/` (and/or `nash_businessbanking_phone/`) inside your `resources/` folder.
2. Add to your `server.cfg` **after** `nash_banking`:

    ```cfg
    ensure lb-phone
    ensure nash_banking
    ensure nash_banking_phone
    ensure nash_businessbanking_phone
    ```

3. Restart the server. In-game, open the phone — the app should be visible.

## How the apps register in LB Phone

Each app waits for `lb-phone` to be `'started'`, then calls `exports['lb-phone']:AddCustomApp` with the UI URL.

```lua
-- nash_banking_phone/client.lua
while GetResourceState('lb-phone') ~= 'started' do
    Wait(500)
end

exports['lb-phone']:AddCustomApp({
    identifier  = 'nash-banking',
    name        = Config.Phone.AppName or 'Nash Banking',
    description = L('app_description'),
    developer   = 'Nash',
    defaultApp  = Config.Phone.DefaultApp ~= false,  -- pre-installed
    size        = 4096,
    ui          = GetCurrentResourceName() .. '/ui/index.html',
    icon        = 'https://cfx-nui-' .. GetCurrentResourceName() .. '/ui/assets/icon.svg',
    fixBlur     = true,
})
```

The app re-registers automatically if `lb-phone` restarts (listener on `onResourceStart`).

## Configuration

Each phone app has its own `config.lua`. **The two apps use slightly different shapes** — the personal app wraps everything in a `Config.Phone` table, the business app uses flat `Config.*` keys. The fields themselves are equivalent.

### Personal — `nash_banking_phone/config.lua`

```lua
Config.Phone = {
    Locale       = 'en',          -- 'fr' or 'en'
    AppName      = 'Nash Banking',
    DefaultApp   = true,          -- pre-installed (false = downloadable from the app store)
    Currency     = '€',
    CurrencyCode = 'EUR',
    BankName     = 'Nash Banking',
    IconFile     = 'icon.svg',    -- file in ui/assets/ (see "App icon" below)
}
```

### Business — `nash_businessbanking_phone/config.lua`

```lua
Config.Locale       = 'en'
Config.AppName      = 'Nash Business'
Config.DefaultApp   = true
Config.Currency     = '€'
Config.CurrencyCode = 'EUR'
Config.BankName     = 'Nash Business Banking'
Config.IconFile     = 'icon.svg'   -- file in ui/assets/ (see "App icon" below)
```

## App icon

The icon shown on the LB Phone home screen is loaded from `ui/assets/<IconFile>` inside each phone resource. To change it, drop a new file (`.svg`, `.png`, `.jpg`, `.webp`, or `.gif`) in `ui/assets/` and point `IconFile` at it:

```lua
-- Personal phone
Config.Phone.IconFile = 'my_logo.png'

-- Business phone
Config.IconFile = 'my_logo.png'
```

Then restart the phone resource. Recommended size: 120×120 px or larger, square, transparent background if possible — the phone scales the icon down to ~60×60 on the home screen.

## How the phone talks to the server

The phone apps reuse the exact same callbacks as the desktop UI via the shared `Bridge.TriggerCallback`:

```lua
-- Example: home screen data
Bridge.TriggerCallback('nash_banking:getTransactions', function(tx, registered, info, savings, rate)
    ...
end)
```

This means any feature added to `server/main.lua` is immediately usable in both the desktop NUI and the phone app.

## Disabling the apps

- Don't `ensure` the app resource in `server.cfg`, **or**
- Set `Config.LBPhone.Enabled = false` in `nash_banking/shared/config.lua` to hide the "open in phone" prompts in the desktop UI.

## Disabling phone notifications

In-bank events (transfers received, TPE paid, requests) can mirror to LB Phone toasts. Toggle with `Config.PhoneNotifications = false` in [`shared/config.lua`](../config/config-main.md).
