# Server Events

This section lists all net events registered on the server. These events can be triggered from the client with `TriggerServerEvent` to interact with the banking system. Each entry includes parameters, guards (rate limits, validation) and example usage.

<details>

<summary>nash_banking:tpeConfirm</summary>

```lua
-- Fired by the client when the buyer confirms a TPE payment with a selected card
TriggerServerEvent('nash_banking:tpeConfirm', selectedCardId)
```

_Parameters, rate limit, validation and full example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:tpeCancel</summary>

```lua
-- Fired by the client when the buyer cancels an ongoing TPE payment
TriggerServerEvent('nash_banking:tpeCancel')
```

_Parameters and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:p2pInitiate</summary>

```lua
-- Fired by the merchant's client to initiate a player-to-player TPE payment
TriggerServerEvent('nash_banking:p2pInitiate', targetServerId, amount, description, businessId)
```

_Parameters, rate limit, validation and example — filled in Pass 2._

</details>

<details>

<summary>Other events</summary>

_Additional server events will be documented in Pass 2._

</details>
