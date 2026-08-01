# Phone Apps (LB Phone & Quasar Phone Pro) — optional

Nash Banking ships two phone apps:

- **`nash_banking_phone`** — personal banking app
- **`nash_businessbanking_phone`** — business banking app

Both are independent resources that depend on `nash_banking` being started first (they import `@nash_banking/bridge/framework/framework.lua` and `@nash_banking/bridge/framework/client.lua`).

## Requirements

- One phone resource started: **`lb-phone`** *or* **`qs-smartphone-pro`**
- `nash_banking` started before the phone apps

The phone type is **auto-detected at startup** — no config needed. LB Phone has priority if both are started.

## Installing an app

1. Drop `nash_banking_phone/` (and/or `nash_businessbanking_phone/`) inside your `resources/` folder.
2. Add to your `server.cfg` **after** `nash_banking`:

    ```cfg
    ensure lb-phone                     # or qs-smartphone-pro
    ensure nash_banking
    ensure nash_banking_phone
    ensure nash_businessbanking_phone
    ```

3. Restart the server. In-game, open the phone — the app should be visible.

## How the apps register

Each app auto-detects which phone system is running and uses the appropriate API. Console print at startup:

```
[Nash Banking Phone] Phone system: lb
```
…or:
```
[Nash Banking Phone] Phone system: quasar
```

- **LB Phone** — pre-installed by default via `exports['lb-phone']:AddCustomApp({...})`.
- **Quasar Phone Pro** — registered in the phone's app store via `exports['qs-smartphone-pro']:addCustomApp({...})`.

The app re-registers automatically if the phone resource restarts (listener on `onResourceStart`).

## Configuration

Each phone app has its own `config.lua`. **The two apps use slightly different shapes** — the personal app wraps everything in a `Config.Phone` table, the business app uses flat `Config.*` keys. The fields themselves are equivalent.

### Personal — `nash_banking_phone/config.lua`

```lua
Config.Phone = {
    Locale       = 'en',          -- 'fr' or 'en'
    AppName      = 'Nash Banking',
    DefaultApp   = true,          -- LB Phone only: pre-installed (false = downloadable from the app store)
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

The icon shown on the phone home screen is loaded from `ui/assets/<IconFile>` inside each phone resource. To change it, drop a new file (`.svg`, `.png`, `.jpg`, `.webp`, or `.gif`) in `ui/assets/` and point `IconFile` at it:

```lua
-- Personal phone
Config.Phone.IconFile = 'my_logo.png'

-- Business phone
Config.IconFile = 'my_logo.png'
```

Then restart the phone resource. Recommended size: 120×120 px or larger, square, transparent background if possible — the phone scales the icon down on the home screen.

## How the phone talks to the server

Both phones use standard FiveM NUI primitives (`RegisterNUICallback` on the server, `fetch()` from the iframe), so the UI code is identical across LB Phone and Quasar Phone Pro. The React app reuses the exact same server callbacks as the desktop UI via `Bridge.TriggerCallback`:

```lua
-- Example: home screen data
Bridge.TriggerCallback('nash_banking:getTransactions', function(tx, registered, info, savings, rate)
    ...
end)
```

Any feature added to `server/main.lua` is immediately usable in the desktop NUI **and** both phone systems.

## Disabling the apps

- Don't `ensure` the app resource in `server.cfg`, **or**
- Set `Config.LBPhone.Enabled = false` in `nash_banking/shared/config.lua` to hide the "open in phone" prompts in the desktop UI.

## Disabling phone notifications

In-bank events (transfers received, TPE paid, requests) can mirror to phone toasts. Toggle with `Config.PhoneNotifications = false` in [`shared/config.lua`](../config/config-main.md).
