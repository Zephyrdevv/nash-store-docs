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

Each phone app has its own `config.lua` with a `Config.Phone` block — app name, locale, currency, "default app" flag. See [`config.lua` in Configuration](../config/config-main.md) for the equivalent desktop-side options.

```lua
-- nash_banking_phone/config.lua (excerpt)
Config.Phone = {
    AppName     = 'Nash Banking',
    DefaultApp  = true,
    Locale      = 'fr',
    Currency    = '€',
    CurrencyCode = 'EUR',
    BankName    = 'Nash Banking',
}
```

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
