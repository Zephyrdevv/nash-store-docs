# Callback Functions

## lib.callback.register(name, cb)
Registers a server-side callback.

| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | Callback identifier |
| cb | function | Handler function (source, ...) |

## lib.callback(name, delay, cb, ...)
Triggers a server callback from the client.

| Parameter | Type | Description |
|-----------|------|-------------|
| name | string | Callback identifier |
| delay | number or false | Throttle delay (ms) or false |
| cb | function | Response handler |
| ... | any | Arguments to pass to server |

## lib.callback.await(name, delay, ...)
Async version that returns the result directly (requires Lua 5.4 coroutines).
