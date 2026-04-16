# How to integrate the TPE in a shop

Call `exports.nash_banking:CardPayment()` from your server-side checkout logic instead of `xPlayer.removeAccountMoney`. The buyer sees an animated TPE on their screen, picks a card and confirms. You get back `(success, message)`.

## When to use it

- **You want the RP flow** — animated TPE, card selection, daily limit enforcement, card locking on loss.
- **You want the transaction in `nash_transactions`** — the export writes a `card_payment` row automatically.
- **You want to respect subscription limits** — daily spending caps from `Config.Cards.Limits` are applied server-side.

Skip it if you just need a silent `bank -= amount` — call `Bridge.RemoveBank` directly.

## Minimum example

```lua
-- server-side
RegisterNetEvent('my_shop:checkout', function(total)
    local src = source
    local success, err = exports.nash_banking:CardPayment(src, total, 'Shop purchase')
    if not success then
        TriggerClientEvent('ox_lib:notify', src, { type = 'error', description = err })
        return
    end
    -- payment confirmed — deliver the goods
end)
```

## Full example — a supermarket checkout

```lua
-- server.lua
local CART = {}

RegisterNetEvent('supermarket:addToCart', function(itemId)
    local src = source
    CART[src] = CART[src] or { items = {}, total = 0 }
    local item = Items[itemId] -- your catalog
    table.insert(CART[src].items, item)
    CART[src].total = CART[src].total + item.price
end)

RegisterNetEvent('supermarket:checkout', function()
    local src = source
    local cart = CART[src]
    if not cart or cart.total <= 0 then return end

    -- Pre-check: has the player a card at all?
    if not exports.nash_banking:HasActiveCard(src) then
        TriggerClientEvent('ox_lib:notify', src, {
            type = 'error',
            description = 'You need a bank card to pay',
        })
        return
    end

    -- Charge via TPE
    local desc = ('Supermarket — %d items'):format(#cart.items)
    local success, err = exports.nash_banking:CardPayment(src, cart.total, desc)

    if not success then
        -- err ∈ {'tpe_timeout', 'insufficient_funds', 'tpe_no_card', 'tpe_limit_exceeded', 'cancelled'}
        TriggerClientEvent('ox_lib:notify', src, { type = 'error', description = err })
        return
    end

    -- Deliver the goods
    for _, item in ipairs(cart.items) do
        exports.ox_inventory:AddItem(src, item.name, 1)
    end

    CART[src] = nil
    TriggerClientEvent('ox_lib:notify', src, {
        type = 'success',
        description = ('Paid %d€ by card'):format(cart.total),
    })
end)
```

## Handling refusals & timeouts

The `message` string when `success == false` is a locale key. You can display it as-is (for untranslated RP messaging) or map it to a friendlier label:

| Key | Meaning | Suggested action |
|---|---|---|
| `tpe_timeout` | Buyer didn't confirm in 60s | Ask them to try again |
| `insufficient_funds` | Balance too low | "Not enough money on your card" |
| `tpe_no_card` | No active card at all | "Get a bank card first" |
| `tpe_limit_exceeded` | Daily / single limit hit | "Card limit reached, try another" |
| `cancelled` | Buyer clicked cancel | Silent — no error toast needed |

## Routing to a business

If your shop is owned by a registered business, don't call `CardPayment` — use the P2P TPE item instead. The shopkeeper presents the `nash_tpe` item with `metadata.business_id` set:

```lua
-- when the shopkeeper acquires their TPE
exports.ox_inventory:AddItem(shopkeeperSrc, 'nash_tpe', 1, { business_id = 42 })
```

Any payment accepted on that terminal credits business ID 42 instead of the shopkeeper's personal account, minus `Config.TPE.PlayerToPlayer.Fee` (2% by default).

## Limits & security

- Amounts ≤ 0 are rejected server-side.
- Amounts above the card's `singleLimit` / remaining `dailyLimit` are rejected with `tpe_limit_exceeded`.
- The card must belong to the confirming player and must not be frozen (verified with a direct SQL query — no trust in the NUI).
- Rate limit: 2s between `tpeConfirm` events per player.
- Timeout: 60s to confirm before the payment auto-cancels.

The export is safe to call on every checkout without extra debouncing on your side.
