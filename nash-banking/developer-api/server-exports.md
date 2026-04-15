# Server Exports

This section provides all available server exports for Nash Banking. These functions allow you to interact directly with the banking system from the server side, enabling custom features and integrations with other resources. Each export includes a short description and usage example for easy implementation.

<details>

<summary>CardPayment</summary>

```lua
-- Trigger an animated TPE on the player's screen and charge their bank account
local success, message = exports.nash_banking:CardPayment(source, amount, description)

if success then
    print('Payment confirmed')
else
    print('Payment failed: ' .. (message or 'unknown'))
end
```

_Signature, parameters, return values and full example — filled in Pass 2._

</details>

<details>

<summary>HasActiveCard</summary>

```lua
-- Check whether a player has at least one non-frozen card
local hasCard = exports.nash_banking:HasActiveCard(source)
```

_Signature, parameters, return values and full example — filled in Pass 2._

</details>

<details>

<summary>InspectCard</summary>

```lua
-- Open the card inspection UI on the client from metadata (used by the physical card item)
exports.nash_banking:InspectCard(source, metadata)
```

_Signature, parameters, return values and full example — filled in Pass 2._

</details>

<details>

<summary>Other exports</summary>

_Additional server exports will be documented in Pass 2._

</details>
