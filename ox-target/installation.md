# Installation

## Requirements

- ox_lib (Nash Edition)
- A supported framework (ESX / QBCore / QBx)

## Steps

1. Place the `ox_target` folder in your resources directory.
2. Add `ensure ox_lib` before `ensure ox_target` in your `server.cfg`.
3. Add `ensure ox_target` in your `server.cfg`.
4. Restart the server.

## server.cfg Example

```
ensure ox_lib
ensure ox_target
```
