# Callbacks

Framework-agnostic callbacks registered via `Bridge.RegisterCallback` on the server. All of them can be triggered from the client via `Bridge.TriggerCallback` (ESX, QBCore and QBOX are handled transparently by the bridge).

> The bridge wraps `ESX.RegisterServerCallback` / `QBCore.Functions.CreateCallback` so callbacks work identically on all three frameworks. See [Compatibility](../compatibility/README.md).

## Transactions & balance

<details>

<summary>nash_banking:getTransactions</summary>

Returns the player's transaction history, registration status, profile, savings balance and subscription savings rate. Also triggers a catch-up interest calculation for offline periods.

```lua
Bridge.TriggerCallback('nash_banking:getTransactions', function(transactions, isRegistered, playerInfo, savingsBalance, savingsRate)
    -- transactions  : array of rows from nash_transactions (capped to Config.MaxTransactions)
    -- isRegistered  : boolean
    -- playerInfo    : { firstname, lastname, phone, dateofbirth, avatar }
    -- savingsBalance: number
    -- savingsRate   : number (% per year, from subscription tier features.savingsRate)
end)
```

</details>

<details>

<summary>nash_banking:deposit</summary>

Moves cash → bank.

```lua
Bridge.TriggerCallback('nash_banking:deposit', function(success, newBank, newCash)
    -- newBank, newCash : updated balances
end, amount)
```

</details>

<details>

<summary>nash_banking:withdraw</summary>

Moves bank → cash. Enforces daily withdraw limits from the subscription tier.

```lua
Bridge.TriggerCallback('nash_banking:withdraw', function(success, newBank, newCash)
end, amount)
```

</details>

<details>

<summary>nash_banking:transfer</summary>

Sends money from the player's bank to another player (by server ID or phone). Enforces daily transfer count and max amount from the subscription tier. Writes transactions on both sides.

```lua
Bridge.TriggerCallback('nash_banking:transfer', function(success, message)
end, targetId, amount, description)
```

</details>

<details>

<summary>nash_banking:requestMoney</summary>

Sends a money request to another player.

```lua
Bridge.TriggerCallback('nash_banking:requestMoney', function(success, message)
end, targetId, amount)
```

</details>

<details>

<summary>nash_banking:respondRequest</summary>

Accepts or declines a received money request (triggers an actual transfer if accepted).

```lua
Bridge.TriggerCallback('nash_banking:respondRequest', function(success, message)
end, requestId, accepted)
```

</details>

## Registration & profile

<details>

<summary>nash_banking:register</summary>

Registers a player in the banking system (triggered by the account creation form). Optionally auto-creates the default card.

```lua
Bridge.TriggerCallback('nash_banking:register', function(success)
end, { firstname = 'John', lastname = 'Doe', phone = '555-1234', dateofbirth = '01/01/1990' })
```

</details>

<details>

<summary>nash_banking:updateAvatar</summary>

Updates the player's profile avatar URL (or clears it when `avatarUrl == ''`).

```lua
Bridge.TriggerCallback('nash_banking:updateAvatar', function(result)
    -- result.success, result.avatar
end, avatarUrl)
```

</details>

## Savings

<details>

<summary>nash_banking:moveSavings</summary>

Moves money from bank to the savings account. Applies catch-up interest first.

```lua
Bridge.TriggerCallback('nash_banking:moveSavings', function(success, bank, savingsBalance)
end, amount)
```

</details>

<details>

<summary>nash_banking:moveToBank</summary>

Moves money from savings back to bank.

```lua
Bridge.TriggerCallback('nash_banking:moveToBank', function(success, bank, savingsBalance)
end, amount)
```

</details>

## Messaging

<details>

<summary>nash_banking:sendMessage</summary>

Sends a message (text or GIF) to another player.

```lua
Bridge.TriggerCallback('nash_banking:sendMessage', function(success)
end, receiverIdentifier, content, msgType) -- msgType: 'text' | 'gif'
```

</details>

<details>

<summary>nash_banking:getMessages</summary>

Returns the conversation with a given contact.

```lua
Bridge.TriggerCallback('nash_banking:getMessages', function(messages)
end, contactIdentifier)
```

</details>

<details>

<summary>nash_banking:searchGifs</summary>

Searches GIFs via Tenor (requires `Config.secret.lua` → `TenorApiKey`).

```lua
Bridge.TriggerCallback('nash_banking:searchGifs', function(gifs)
end, query)
```

</details>

<details>

<summary>nash_banking:trendingGifs</summary>

Returns trending GIFs for the messaging UI.

```lua
Bridge.TriggerCallback('nash_banking:trendingGifs', function(gifs) end)
```

</details>

<details>

<summary>nash_banking:deleteConversation</summary>

Deletes all messages in a conversation.

```lua
Bridge.TriggerCallback('nash_banking:deleteConversation', function(success)
end, contactIdentifier)
```

</details>

## Cards

<details>

<summary>nash_banking:getCards</summary>

Returns the player's cards (with masked PIN for security).

```lua
Bridge.TriggerCallback('nash_banking:getCards', function(cards)
    -- cards[i]: { id, card_number, cvv, expiry, card_type, format, frozen, delivery_status, description }
end)
```

</details>

<details>

<summary>nash_banking:createCard</summary>

Creates a new card (virtual or physical). Enforces `maxCards` from the subscription tier. Charges `Config.Delivery.Cost` for physical cards.

```lua
Bridge.TriggerCallback('nash_banking:createCard', function(success, message)
end, cardType, format, deliveryLocation)
-- cardType: 'standard' | 'plus' | 'premium'
-- format:   'virtual' | 'physical'
-- deliveryLocation: index in Config.Delivery.Locations (physical only)
```

</details>

<details>

<summary>nash_banking:pickupCards</summary>

Picks up all delivered physical cards at a given delivery location (spawns the `nash_card_physical` item with metadata).

```lua
Bridge.TriggerCallback('nash_banking:pickupCards', function(success, count)
end, locationIndex)
```

</details>

<details>

<summary>nash_banking:getDeliveryLocations</summary>

Returns the list of pickup points along with how many cards are ready at each.

```lua
Bridge.TriggerCallback('nash_banking:getDeliveryLocations', function(locations) end)
```

</details>

<details>

<summary>nash_banking:toggleFreezeCard</summary>

Freezes / unfreezes a card. A frozen card can't be used for TPE payments.

```lua
Bridge.TriggerCallback('nash_banking:toggleFreezeCard', function(success, frozen)
end, cardId)
```

</details>

<details>

<summary>nash_banking:updateCardPin</summary>

Changes a card PIN (4 digits).

```lua
Bridge.TriggerCallback('nash_banking:updateCardPin', function(success)
end, cardId, newPin)
```

</details>

<details>

<summary>nash_banking:deleteCard</summary>

Permanently deletes a card. Removes associated physical items from inventory.

```lua
Bridge.TriggerCallback('nash_banking:deleteCard', function(success)
end, cardId)
```

</details>

<details>

<summary>nash_banking:getCardDailyLimit</summary>

Returns the current day's spend on a card along with the daily limit from the card tier.

```lua
Bridge.TriggerCallback('nash_banking:getCardDailyLimit', function(spent, limit)
end, cardId)
```

</details>

<details>

<summary>nash_banking:getCardSpendingLimit</summary>

Returns the player-defined monthly spending limit on a card (only used when `Config.Cards.SpendingLimit.BySubscription = false`).

```lua
Bridge.TriggerCallback('nash_banking:getCardSpendingLimit', function(enabled, limit, spent) end, cardId)
```

</details>

<details>

<summary>nash_banking:updateCardSpendingLimit</summary>

Updates the player-defined monthly spending limit on a card.

```lua
Bridge.TriggerCallback('nash_banking:updateCardSpendingLimit', function(success)
end, cardId, enabled, limit)
```

</details>

<details>

<summary>nash_banking:getCardTransactions</summary>

Returns the transactions made with a given card.

```lua
Bridge.TriggerCallback('nash_banking:getCardTransactions', function(transactions) end, cardId)
```

</details>

## Subscriptions

<details>

<summary>nash_banking:getSubscription</summary>

Returns the current subscription, all tiers and the next charge date.

```lua
Bridge.TriggerCallback('nash_banking:getSubscription', function(data)
    -- data.current   : { tier, started_at, last_charged_at, next_charge_at }
    -- data.tiers     : Config.Subscriptions.Tiers
    -- data.features  : resolved features of current tier
end)
```

</details>

<details>

<summary>nash_banking:upgradeSubscription</summary>

Upgrades to a higher tier. Charges the price immediately if the player has funds (or allows negative if `Config.Subscriptions.AllowNegativeBalance = true`).

```lua
Bridge.TriggerCallback('nash_banking:upgradeSubscription', function(success, message)
end, newTier) -- 'plus' | 'premium'
```

</details>

<details>

<summary>nash_banking:cancelSubscription</summary>

Downgrades back to `standard`.

```lua
Bridge.TriggerCallback('nash_banking:cancelSubscription', function(success) end)
```

</details>

## Businesses

<details>

<summary>nash_banking:getBusiness</summary>

Returns the business the player belongs to (role, balance, employees, TPE ownership).

```lua
Bridge.TriggerCallback('nash_banking:getBusiness', function(business) end)
```

</details>

<details>

<summary>nash_banking:createBusiness</summary>

Creates a new business (when `Config.Business.AllowPlayerCreation = true`).

```lua
Bridge.TriggerCallback('nash_banking:createBusiness', function(success, message) end, name)
```

</details>

<details>

<summary>nash_banking:businessDeposit / nash_banking:businessWithdraw</summary>

Moves money between the player's personal bank account and the business balance (role ≥ `Config.Business.AccessLevel`).

```lua
Bridge.TriggerCallback('nash_banking:businessDeposit', function(success) end, amount)
Bridge.TriggerCallback('nash_banking:businessWithdraw', function(success) end, amount)
```

</details>

<details>

<summary>nash_banking:getBusinessTransactions</summary>

Returns the business's transaction ledger.

```lua
Bridge.TriggerCallback('nash_banking:getBusinessTransactions', function(transactions) end)
```

</details>

<details>

<summary>nash_banking:addBusinessEmployee / nash_banking:removeBusinessEmployee</summary>

Adds / removes an employee (by identifier).

```lua
Bridge.TriggerCallback('nash_banking:addBusinessEmployee', function(success) end, identifier, role)
Bridge.TriggerCallback('nash_banking:removeBusinessEmployee', function(success) end, identifier)
```

</details>

<details>

<summary>nash_banking:buyBusinessTPE</summary>

Buys a business TPE terminal (`Config.Business.TPE.Price`), gives the `nash_tpe` item with `metadata.business_id` set.

```lua
Bridge.TriggerCallback('nash_banking:buyBusinessTPE', function(success, message) end)
```

</details>

## Invest (stocks)

<details>

<summary>nash_banking:getStocks</summary>

Returns all stocks with their current price, 24h change and volatility.

```lua
Bridge.TriggerCallback('nash_banking:getStocks', function(stocks) end)
```

</details>

<details>

<summary>nash_banking:getStockHistory</summary>

Returns the price history for a stock.

```lua
Bridge.TriggerCallback('nash_banking:getStockHistory', function(history) end, symbol)
```

</details>

<details>

<summary>nash_banking:getPortfolio</summary>

Returns the player's stock holdings (shares + avg buy price).

```lua
Bridge.TriggerCallback('nash_banking:getPortfolio', function(positions) end)
```

</details>

<details>

<summary>nash_banking:buyStock / nash_banking:sellStock</summary>

Buys / sells stock shares. Requires `features.investAccess` from the subscription.

```lua
Bridge.TriggerCallback('nash_banking:buyStock', function(success, message) end, symbol, amount)
Bridge.TriggerCallback('nash_banking:sellStock', function(success, message) end, symbol, shares)
```

</details>

## Crypto

<details>

<summary>nash_banking:getCryptos</summary>

Returns all crypto pairs with current price and volatility.

```lua
Bridge.TriggerCallback('nash_banking:getCryptos', function(cryptos) end)
```

</details>

<details>

<summary>nash_banking:getCryptoHistory</summary>

Returns price history for a crypto symbol.

```lua
Bridge.TriggerCallback('nash_banking:getCryptoHistory', function(history) end, symbol)
```

</details>

<details>

<summary>nash_banking:getCryptoPortfolio</summary>

Returns the player's crypto holdings.

```lua
Bridge.TriggerCallback('nash_banking:getCryptoPortfolio', function(positions) end)
```

</details>

<details>

<summary>nash_banking:buyCrypto / nash_banking:sellCrypto</summary>

Buys / sells crypto. Requires `features.cryptoAccess` from the subscription (Premium only by default).

```lua
Bridge.TriggerCallback('nash_banking:buyCrypto', function(success, message) end, symbol, amount)
Bridge.TriggerCallback('nash_banking:sellCrypto', function(success, message) end, symbol, amount)
```

</details>

## ATM

<details>

<summary>nash_banking:atmGetCards</summary>

Returns the player's active cards for the ATM card selector.

```lua
Bridge.TriggerCallback('nash_banking:atmGetCards', function(cards) end)
```

</details>

<details>

<summary>nash_banking:atmVerifyPin</summary>

Validates the PIN on a card (3 attempts — freezes the card on failure).

```lua
Bridge.TriggerCallback('nash_banking:atmVerifyPin', function(success, attemptsLeft) end, cardId, pin)
```

</details>

<details>

<summary>nash_banking:atmDeposit / nash_banking:atmWithdraw</summary>

Deposit / withdraw from the ATM. Withdraw applies `Config.ATM.Fee` after `features.freeAtmWithdrawals`.

```lua
Bridge.TriggerCallback('nash_banking:atmDeposit', function(success, newBank, newCash) end, cardId, amount)
Bridge.TriggerCallback('nash_banking:atmWithdraw', function(success, newBank, newCash, fee) end, cardId, amount)
```

</details>

## Admin

Only callable by players matching `Config.Admin.Permissions`. See [Commands](commands.md) and the dedicated admin UI.

<details>

<summary>nash_banking:adminCheckPermission</summary>

Returns whether the source is allowed to use admin features.

```lua
Bridge.TriggerCallback('nash_banking:adminCheckPermission', function(isAdmin) end)
```

</details>

<details>

<summary>nash_banking:adminGetStats / adminGetTopPlayers / adminGetTopBusinesses</summary>

Global banking stats and leaderboards for the admin dashboard.

```lua
Bridge.TriggerCallback('nash_banking:adminGetStats', function(stats) end)
Bridge.TriggerCallback('nash_banking:adminGetTopPlayers', function(players) end)
Bridge.TriggerCallback('nash_banking:adminGetTopBusinesses', function(businesses) end)
```

</details>

<details>

<summary>nash_banking:adminSearchPlayer</summary>

Fuzzy-searches a player by identifier, name or phone and returns their banking profile.

```lua
Bridge.TriggerCallback('nash_banking:adminSearchPlayer', function(players) end, query)
```

</details>

<details>

<summary>nash_banking:adminAddMoney / nash_banking:adminRemoveMoney</summary>

Credits / debits a player's bank account. Writes an `admin_action` transaction.

```lua
Bridge.TriggerCallback('nash_banking:adminAddMoney', function(success) end, identifier, amount)
Bridge.TriggerCallback('nash_banking:adminRemoveMoney', function(success) end, identifier, amount)
```

</details>

<details>

<summary>nash_banking:adminGetJobs / adminCreateBusiness / adminDeleteBusiness / adminGetBusinessDetails</summary>

Business administration endpoints.

```lua
Bridge.TriggerCallback('nash_banking:adminGetJobs', function(jobs) end)
Bridge.TriggerCallback('nash_banking:adminCreateBusiness', function(success, id) end, ownerIdentifier, name, jobName)
Bridge.TriggerCallback('nash_banking:adminDeleteBusiness', function(success) end, businessId)
Bridge.TriggerCallback('nash_banking:adminGetBusinessDetails', function(details) end, businessId)
```

</details>

<details>

<summary>nash_banking:adminAddBusinessMoney / nash_banking:adminRemoveBusinessMoney</summary>

Credits / debits a business's balance.

```lua
Bridge.TriggerCallback('nash_banking:adminAddBusinessMoney', function(success) end, businessId, amount)
Bridge.TriggerCallback('nash_banking:adminRemoveBusinessMoney', function(success) end, businessId, amount)
```

</details>
