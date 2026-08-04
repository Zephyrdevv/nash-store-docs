# Customize locales / add a new language

Since **v1.2.0**, Nash Banking loads its locale strings at **runtime** from Lua files — you can add or edit translations without touching the compiled React NUI and without rebuilding anything.

The mechanism is the same for the desktop banking UI, the personal phone app, and the business phone app. The Lua locale table you edit is sent to each React NUI on open and merged over the compiled `fr` / `en` fallbacks.

## Where the locale files live

| Resource | File(s) | What it drives |
|---|---|---|
| `nash_banking` | `nash_banking/locales/en.lua`, `fr.lua`, `<yours>.lua` | Server messages, transaction descriptions, notifications, **and every string in the desktop banking UI (menus, buttons, modals)** |
| `nash_banking_phone` | `nash_banking_phone/locales.lua` (single file with all `Locales[lang]` blocks inside) | Every string in the personal banking phone app |
| `nash_businessbanking_phone` | `nash_businessbanking_phone/locales.lua` | Every string in the business banking phone app |

All these files are in `escrow_ignore` — you can edit them directly on your server.

## Change an existing translation

Open the corresponding file, edit the key's value, then `restart` the resource. Both the Lua side and the React NUI pick up the change on next open — **no rebuild needed**.

## Add a new language (example: Spanish `es`)

### 1. Main banking

Duplicate `nash_banking/locales/en.lua` → `nash_banking/locales/es.lua`, translate every value, and rename the top-level table:

```lua
-- nash_banking/locales/es.lua
Locales['es'] = {
    tx_card_payment = 'Pago con tarjeta',
    tpe_timeout = 'Tiempo de espera agotado',
    insufficient_funds = 'Fondos insuficientes',
    home_title = 'Inicio',
    cards_title = 'Tarjetas',
    -- … every key from en.lua, translated
}
```

The fxmanifest already loads `locales/*.lua` via a glob, so no manifest edit needed.

### 2. Personal phone

Open `nash_banking_phone/locales.lua` and add a new block alongside the existing `Locales['fr']` / `Locales['en']`:

```lua
Locales['es'] = {
    -- … same keys as the existing Locales['fr'] block, translated
}
```

### 3. Business phone

Same treatment in `nash_businessbanking_phone/locales.lua`.

### 4. Set the language

- **Main banking + server messages**: `Config.Locale = 'es'` in `nash_banking/shared/config.lua`.
- **Personal phone**: `Config.Phone.Locale = 'es'` in `nash_banking_phone/config.lua`.
- **Business phone**: `Config.Locale = 'es'` in `nash_businessbanking_phone/config.lua`.

### 5. Restart

```
restart nash_banking
restart nash_banking_phone
restart nash_businessbanking_phone
```

Open the bank in-game — everything is in Spanish. No rebuild, no source access needed.

## Fallback behavior

For any key missing in your `es.lua`, the app silently falls back to the compiled default (`fr` for the phones, `fr` then `en` for the desktop UI). No red errors, no broken UI — you can start with a partial translation and fill in gaps over time.

## Translating in bulk

The `en.lua` file has ~500 keys for the main banking, plus ~150 in each phone `locales.lua`. Easiest workflow:

1. Copy the whole `Locales['en']` block into your favorite LLM.
2. Ask: *"Translate every right-hand string to Spanish while keeping the left-hand keys untouched."*
3. Paste the result back as your `Locales['es']` block.
4. Skim once to catch any culturally-specific string (currency names, greetings, in-jokes).

## Locale helper notes

- Server-side / client-side Lua: `L('key')` reads from `Locales[Config.Locale]`, fallback to `Locales['fr']`, fallback to the raw key.
- Desktop React: `t('key')` merges `data.localeOverrides` (sent from the Lua file at `openBank` time) over the compiled `fr` / `en` defaults.
- Phone apps React: `t('key')` merges the Lua-provided override table over each app's own `DEFAULT_LOCALE` (French).
