# Client Events

Net events broadcast to the client by the server. Listen to them with `RegisterNetEvent` / `AddEventHandler` to hook into the banking flow.

## TPE / Payment flow

<details>

<summary>nash_banking:openTPE</summary>

Opens the TPE overlay on the buyer's screen with the amount, description and their list of active cards.

```lua
RegisterNetEvent('nash_banking:openTPE', function(data)
    -- data.amount        (number)
    -- data.description   (string)
    -- data.cards         (array of cards — id, card_number, card_type, tier, tierColor, tierGradient, playerName)
end)
```

</details>

<details>

<summary>nash_banking:tpeResult</summary>

Result of a TPE payment (success, timeout, insufficient funds, card unavailable, limit exceeded, cancelled).

```lua
RegisterNetEvent('nash_banking:tpeResult', function(result)
    -- result.success  (boolean)
    -- result.message  (string?) — locale key when success == false
end)
```

**Possible `message` values**

| Key | Meaning |
|---|---|
| `tpe_timeout` | Buyer didn't confirm in 60s |
| `insufficient_funds` | Bank balance too low |
| `card_unavailable` | Selected card got frozen / deleted |
| `tpe_limit_exceeded` | Card daily / single limit hit |
| `cancelled` | Buyer cancelled manually |

</details>

<details>

<summary>nash_banking:buyerReceiveTPE</summary>

P2P flow — buyer receives the TPE prop and overlay in their hand (same payload as `openTPE`).

```lua
RegisterNetEvent('nash_banking:buyerReceiveTPE', function(data)
    -- data.amount, data.description, data.cards
end)
```

</details>

<details>

<summary>nash_banking:sellerTransferOut</summary>

Tells the seller's client to play the "give" animation and remove the TPE prop from their hand (the prop appears in the buyer's hand).

```lua
RegisterNetEvent('nash_banking:sellerTransferOut', function()
    -- No payload
end)
```

</details>

<details>

<summary>nash_banking:sellerCleanup</summary>

Tells the seller's client to remove the TPE prop after an error (buyer not found, invalid amount, etc.).

```lua
RegisterNetEvent('nash_banking:sellerCleanup', function()
    -- No payload
end)
```

</details>

<details>

<summary>nash_banking:p2pResult</summary>

Result of a P2P payment sent to the seller.

```lua
RegisterNetEvent('nash_banking:p2pResult', function(result)
    -- result.success  (boolean)
    -- result.amount   (number?) — net amount received (after fee)
    -- result.role     ('seller')
end)
```

</details>

## Balance & notifications

<details>

<summary>nash_banking:updateBalance</summary>

Refreshes the player's bank / cash balance in all open UIs (banking NUI, phone app, business app).

```lua
RegisterNetEvent('nash_banking:updateBalance', function(data)
    -- data.bank  (number)
    -- data.cash  (number)
end)
```

</details>

<details>

<summary>nash_banking:notification</summary>

Displays a Nash-Banking-styled toast in the UI. Respects `Config.PhoneNotifications` for mirroring to LB Phone.

```lua
RegisterNetEvent('nash_banking:notification', function(data)
    -- data.type    ('success' | 'error' | 'warning' | 'info')
    -- data.title   (string)
    -- data.message (string)
    -- data.icon    (string?)  — FontAwesome icon
end)
```

</details>

## Requests (transfer requests)

<details>

<summary>nash_banking:incomingRequest</summary>

A player has requested money from you.

```lua
RegisterNetEvent('nash_banking:incomingRequest', function(reqId, senderName, amount)
    -- reqId, senderName, amount
end)
```

</details>

<details>

<summary>nash_banking:requestAccepted</summary>

The target accepted the money request you sent.

```lua
RegisterNetEvent('nash_banking:requestAccepted', function(amount, targetName)
    -- amount, targetName
end)
```

</details>

<details>

<summary>nash_banking:requestDeclined</summary>

The target declined or the request expired.

```lua
RegisterNetEvent('nash_banking:requestDeclined', function(amount)
    -- amount
end)
```

</details>

## Messaging (nash_banking chat app)

<details>

<summary>nash_banking:newMessage</summary>

New in-bank message received (text / GIF).

```lua
RegisterNetEvent('nash_banking:newMessage', function(message, senderName)
    -- message.id, message.sender_identifier, message.content, message.type
end)
```

</details>

## Cards

<details>

<summary>nash_banking:inspectCard</summary>

Opens the card inspection UI (used by the physical card item and the `InspectCard` server export).

```lua
RegisterNetEvent('nash_banking:inspectCard', function(metadata)
    -- card_number, cvv, expiry, pin, card_type, cardholder, frozen, description
end)
```

</details>

<details>

<summary>nash_banking:refreshDeliveries</summary>

Tells the client the delivery status of a card changed (ready for pickup, delivered, etc.) so the UI refreshes.

```lua
RegisterNetEvent('nash_banking:refreshDeliveries', function()
    -- No payload
end)
```

</details>
