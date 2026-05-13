# config.locations.lua

Bank branches, standalone ATMs and delivery points.

File: `shared/config.locations.lua`

## Bank branches

NPC bankers spawn at each entry. The interaction type is set via `Config.InteractionType` (see [config.lua](config-main.md)).

```lua
Config.BankLocations = {
    { coords = vector4(149.4754, -1042.0674, 29.3680, 337.8459),  label = 'Legion Square Bank' },
    { coords = vector4(313.7769, -280.4378,  54.1586, 339.4863),  label = 'Alta Bank' },
    { coords = vector4(-350.96,  -49.77,     49.04,   338.43),    label = 'Burton Bank' },
    { coords = vector4(-1212.63, -330.76,    37.78,   23.67),     label = 'Rockford Hills Bank' },
    { coords = vector4(-2962.53, 482.97,     15.7,    86.07),     label = 'Great Ocean Highway Bank' },
    { coords = vector4(1175.07,  2706.41,    38.09,   178.81),    label = 'Route 68 Bank' },
}
```

## Standalone ATMs

28 ATM locations shipped by default, covering Los Santos, Vinewood, Del Perro, Paleto Bay, Sandy Shores, Mirror Park, Little Seoul, LSIA, etc.

```lua
Config.ATMLocations = {
    { coords = vector3(149.12, -1038.94, 29.37), label = 'Legion Square ATM' },
    { coords = vector3(-846.29, -340.34, 38.68), label = 'Hawick ATM' },
    -- ... 26 more
}
```

{% hint style="info" %}
Nash Banking also detects vanilla GTA ATM props (`prop_atm_01`, `prop_atm_02`, `prop_atm_03`, `prop_fleeca_atm`) — you can interact with any stock ATM in the map without adding it here. See [config.atm.lua](config-atm.md) → `Config.ATM.Models`.
{% endhint %}

## Delivery points

Additional pickup points for physical cards (on top of the bank branches, which are always pickup points).

```lua
Config.DeliveryPoints = {
    -- { coords = vector4(x, y, z, heading), label = 'Sandy Shores Pickup Point' },
}
```
