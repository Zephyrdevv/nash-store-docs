# How to customize locales

Nash Banking uses **two** locale systems — Lua (server & client notifications, transaction descriptions, logs) and React (the UI strings).

## The two locale systems

### 1. Lua locales

Located in `locales/*.lua`. Loaded via `ox_lib`'s locale system.

### 2. React locales

Located in `web/src/hooks/useLocale.ts` (plus the phone app equivalents).

{% hint style="warning" %}
When adding or editing any text, **both** systems must be updated. A string missing from one of them will appear in English by default or as its raw key.
{% endhint %}

## Adding a new language

_Filled in Pass 3._

## Changing existing translations

_Filled in Pass 3._

## Rebuilding the UI after changes

_Filled in Pass 3._
