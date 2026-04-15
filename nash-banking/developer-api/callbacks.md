# Callbacks

Framework-agnostic callbacks registered via `Bridge.RegisterCallback` on the server. All of them can be triggered from the client via `Bridge.TriggerCallback` (ESX, QBCore and QBOX are handled transparently by the bridge).

## Transactions & balance

<details>

<summary>nash_banking:getTransactions</summary>

```lua
Bridge.TriggerCallback('nash_banking:getTransactions', function(transactions, isRegistered, playerInfo, savingsBalance, savingsRate)
    -- ...
end)
```

_Signature, return values and example — filled in Pass 2._

</details>

<details>

<summary>nash_banking:deposit</summary>

```lua
Bridge.TriggerCallback('nash_banking:deposit', function(success, newBank, newCash)
    -- ...
end, amount)
```

_Signature, return values — filled in Pass 2._

</details>

<details>

<summary>nash_banking:withdraw</summary>

```lua
Bridge.TriggerCallback('nash_banking:withdraw', function(success, newBank, newCash)
    -- ...
end, amount)
```

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:transfer</summary>

```lua
Bridge.TriggerCallback('nash_banking:transfer', function(success, message)
    -- ...
end, targetIdentifier, amount, description)
```

_Filled in Pass 2._

</details>

## Savings

<details>

<summary>nash_banking:moveSavings</summary>

```lua
Bridge.TriggerCallback('nash_banking:moveSavings', function(success, bank, savingsBalance)
    -- Move money from bank to savings
end, amount)
```

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:moveToBank</summary>

```lua
Bridge.TriggerCallback('nash_banking:moveToBank', function(success, bank, savingsBalance)
    -- Move money from savings back to bank
end, amount)
```

_Filled in Pass 2._

</details>

## Cards

<details>

<summary>nash_banking:getCards</summary>

```lua
Bridge.TriggerCallback('nash_banking:getCards', function(cards)
    -- ...
end)
```

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:createCard</summary>

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:freezeCard</summary>

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:deleteCard</summary>

_Filled in Pass 2._

</details>

<details>

<summary>Other card callbacks</summary>

_Filled in Pass 2._

</details>

## Subscriptions

<details>

<summary>nash_banking:upgradeSubscription</summary>

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:cancelSubscription</summary>

_Filled in Pass 2._

</details>

## Businesses

<details>

<summary>nash_banking:getBusinessData</summary>

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:buyBusinessTPE</summary>

_Filled in Pass 2._

</details>

<details>

<summary>Other business callbacks</summary>

_Filled in Pass 2._

</details>

## Invest & Crypto

<details>

<summary>nash_banking:getInvestData</summary>

_Filled in Pass 2._

</details>

<details>

<summary>nash_banking:getCryptoData</summary>

_Filled in Pass 2._

</details>

<details>

<summary>Other invest/crypto callbacks</summary>

_Filled in Pass 2._

</details>

## Admin

<details>

<summary>nash_banking:adminGetData</summary>

_Filled in Pass 2._

</details>

<details>

<summary>Other admin callbacks</summary>

_Filled in Pass 2._

</details>
