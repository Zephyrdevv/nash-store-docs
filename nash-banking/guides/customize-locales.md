# How to customize locales

Nash Banking uses **two** parallel locale systems:

1. **Lua locales** — `nash_banking/locales/*.lua` — server-side notifications, transaction descriptions, chat messages, Discord logs.
2. **React locales** — `nash_banking/web/src/hooks/useLocale.ts` (+ the two phone apps) — every string rendered in the NUI.

{% hint style="warning" %}
When adding or editing any user-facing text, **both** systems must be updated. A string missing from one side shows as its raw key (`tx_card_payment`) or falls back to English.
{% endhint %}

## The two systems

### Lua locales (`locales/`)

One file per language:

```
nash_banking/locales/
├── en.lua
└── fr.lua
```

Each file returns a `Locales[lang]` table:

```lua
-- locales/en.lua
Locales['en'] = {
    tx_card_payment = 'Card payment',
    tpe_timeout = 'Payment timed out',
    insufficient_funds = 'Insufficient funds',
    -- ...
}
```

The active language is picked from `Config.Locale` (`'fr'` or `'en'`). The helper `L('key')` in every server / client script looks up the key; if it's missing, it falls back to `Locales['fr']`, then to the raw key.

### React locales (`web/src/hooks/useLocale.ts`)

Inside `useLocale.ts` you'll find:

```ts
const LOCALES = {
  en: {
    home_title: 'Home',
    cards_title: 'Cards',
    // ...
  },
  fr: {
    home_title: 'Accueil',
    cards_title: 'Cartes',
    // ...
  },
}
```

Components call `const t = useLocale()` and then `t('home_title')` — same fallback logic as the Lua side.

The two phone apps have their own `useLocale.ts` under `nash_banking_phone/web/src/` and `nash_businessbanking_phone/web/src/`.

## Adding a new language

Say you want to add Spanish (`es`):

1. **Lua** — duplicate `locales/fr.lua` → `locales/es.lua`. Translate every value. Top line becomes `Locales['es'] = { … }`.
2. **React (main UI)** — add an `es` block in `web/src/hooks/useLocale.ts` with every key translated.
3. **React (phone apps)** — same in the two phone app `useLocale.ts`.
4. Set `Config.Locale = 'es'` in `shared/config.lua` and in each phone app's `config.lua`.
5. Rebuild the UIs (see below).

## Changing existing translations

- **Lua** — edit `locales/fr.lua` (or `en.lua`) in place. `restart nash_banking` and the new string takes effect immediately.
- **React** — edit `useLocale.ts` and rebuild. Without a rebuild, the old string stays in the compiled `build/`.

## Rebuilding the UI after changes

All three resources ship with a Vite-based React front-end:

```bash
# Main banking NUI
cd nash_banking/web
npm install     # first time only
npm run build

# Phone apps (only if you edited their locales)
cd ../../nash_banking_phone/web && npm run build
cd ../../nash_businessbanking_phone/web && npm run build
```

Each `npm run build` writes the new assets into the resource's `build/` (or `ui/`) folder. Then `restart nash_banking` (or the matching phone resource).

{% hint style="info" %}
Only string changes can be hot-reloaded via `restart` — if you change the React component tree, the NUI needs a `refresh` client-side too (close + reopen the banking UI).
{% endhint %}

## Locale helper notes

- Server-side `L('key')` is defined in `server/main.lua` and reused everywhere.
- Client-side `L('key')` is defined in `client/main.lua`.
- When adding a locale key, do it in **both** `en.lua` and `fr.lua` — a missing key on one side logs a warning in the console on first use.
