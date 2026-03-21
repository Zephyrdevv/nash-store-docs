# Installation

## Steps

1. Place the `ox_lib` folder in your resources directory.
2. Add `ensure ox_lib` to your `server.cfg` **before** all Nash Store scripts.
3. Restart the server.

## server.cfg Example

```
ensure ox_lib
ensure ox_target
ensure nash_shops
ensure nash_clothesshop
```

ox_lib must always be the first Nash resource ensured.
