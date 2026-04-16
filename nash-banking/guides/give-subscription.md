# How to give a subscription

Three ways to grant a subscription tier to a player — admin panel, direct SQL, or programmatically from another resource.

## Via admin panel (recommended)

1. As an admin, run `/bankadmin` in chat.
2. Open the **Players** tab and search for the target player (by name, citizen ID or phone).
3. Click their row → **Actions** → **Set subscription** → pick `plus` / `premium`.
4. Optional: set a custom expiry date, otherwise it defaults to `NOW() + Config.Subscriptions.ChargeInterval` days.

No money is debited when you grant a subscription from the admin panel — it's a free grant.

## Via SQL

Directly insert or update the row in `nash_subscriptions`:

```sql
-- Grant Premium for 30 days from now
INSERT INTO nash_subscriptions (identifier, tier, started_at, next_charge_at, auto_renew)
VALUES ('license:abc123…', 'premium', NOW(), DATE_ADD(NOW(), INTERVAL 30 DAY), 1)
ON DUPLICATE KEY UPDATE
    tier = 'premium',
    started_at = NOW(),
    next_charge_at = DATE_ADD(NOW(), INTERVAL 30 DAY),
    auto_renew = 1;
```

Replace `'license:abc123…'` with the player's identifier (QBCore: `citizenid`).

## Via server export (programmatic)

Call `Database.SetSubscription` from another server-side script:

```lua
-- In your custom resource (server-side):
local identifier = 'license:abc123…'
local tier = 'premium'
local nextChargeAt = os.date('%Y-%m-%d %H:%M:%S', os.time() + 30 * 86400)

exports.nash_banking:SetSubscription(identifier, tier, nextChargeAt)
```

{% hint style="info" %}
`SetSubscription` is not exposed as a formal export by default. If you want to call it from other resources, wrap the `Database.SetSubscription` function with an `exports()` in `server/main.lua`.
{% endhint %}

## Lifetime / permanent subscriptions

Set `next_charge_at` to a far-future date or `NULL` and keep `auto_renew = 1`. Next-charge sweeps only consider rows where `next_charge_at <= NOW()`, so a very distant date effectively makes the subscription permanent:

```sql
UPDATE nash_subscriptions
SET tier = 'premium',
    next_charge_at = '2099-01-01 00:00:00',
    auto_renew = 1
WHERE identifier = 'license:abc123…';
```

## Granting tokens / rewards for external purchases

If you sell subscriptions on Tebex and want to auto-grant on purchase:

1. Use Tebex's in-game commands feature with a custom command like `/grantsub {{identifier}} premium`.
2. Register the command in a small custom resource:

    ```lua
    RegisterCommand('grantsub', function(source, args)
        if source ~= 0 then return end -- console only
        local identifier = args[1]
        local tier = args[2] or 'premium'
        local nextChargeAt = os.date('%Y-%m-%d %H:%M:%S', os.time() + 30 * 86400)
        -- Call Nash Banking's Database.SetSubscription here
    end, true)
    ```
