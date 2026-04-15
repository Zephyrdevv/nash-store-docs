# Client Events

This section lists all net events broadcasted to the client by the server. You can listen to them with `RegisterNetEvent` / `AddEventHandler` to hook into the banking flow. Each entry includes payload and example usage.

<details>

<summary>nash_banking:openTPE</summary>

```lua
RegisterNetEvent('nash_banking:openTPE', function(data)
    -- data.amount, data.description, data.cards
    -- Opens the TPE overlay on the buyer's screen
end)
```

_Payload, fields and full example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:tpeResult</summary>

```lua
RegisterNetEvent('nash_banking:tpeResult', function(result)
    -- result.success (boolean), result.message (string?)
end)
```

_Payload and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:buyerReceiveTPE</summary>

```lua
RegisterNetEvent('nash_banking:buyerReceiveTPE', function(data)
    -- P2P flow — buyer receives the TPE in their hand
    -- data.amount, data.description, data.cards
end)
```

_Payload and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:sellerTransferOut</summary>

```lua
RegisterNetEvent('nash_banking:sellerTransferOut', function()
    -- Tells the seller to play the give animation and remove the prop
end)
```

_Example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:sellerCleanup</summary>

```lua
RegisterNetEvent('nash_banking:sellerCleanup', function()
    -- Tells the seller to remove the prop after an error
end)
```

_Example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:updateBalance</summary>

```lua
RegisterNetEvent('nash_banking:updateBalance', function(data)
    -- data.bank, data.cash
    -- Refreshes the bank / cash balance in the UI
end)
```

_Payload and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:notification</summary>

```lua
RegisterNetEvent('nash_banking:notification', function(data)
    -- data.type ('success' | 'error' | 'warning' | 'info')
    -- data.title, data.message, data.icon
end)
```

_Payload and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:inspectCard</summary>

```lua
RegisterNetEvent('nash_banking:inspectCard', function(metadata)
    -- Opens the card inspection UI with the given metadata
end)
```

_Payload and example — filled in Pass 2._

</details>

<details>

<summary>Other events</summary>

_Additional client events will be documented in Pass 2._

</details>
