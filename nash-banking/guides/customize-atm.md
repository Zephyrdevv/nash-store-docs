# How to add ATM / TPE locations

Bank branches and standalone ATM positions live in [`shared/config.locations.lua`](../config/config-locations.md). The ATM detection also auto-scans for vanilla GTA ATM props in a 50m radius — you don't need to list every Fleeca.

## Adding an ATM location

Append to `Config.ATMLocations`:

```lua
Config.ATMLocations = {
    { coords = vector3(149.12, -1038.94, 29.37), label = 'DAB Legion Square' },
    -- ...
    -- New ATM
    { coords = vector3(5545.20, -152.08, 35.10), label = 'DAB Mount Chiliad' },
}
```

A purple blip + ox_target interaction appears at the coords after restart.

## Adding a bank agency (desktop UI)

Append to `Config.BankLocations`:

```lua
Config.BankLocations = {
    { coords = vector4(149.4754, -1042.0674, 29.3680, 337.8459), label = 'Banque Legion Square' },
    -- ...
    -- New branch
    { coords = vector4(1089.03, -3192.14, -38.99, 180.0), label = 'Banque souterraine LSIA' },
}
```

`vector4` (with heading) instead of `vector3` — the heading is the NPC's facing direction.

## Changing interaction range

Three settings matter:

```lua
-- shared/config.lua
Config.InteractionDistance = 2.5           -- default E-key mode
Config.OxTarget.Distance   = 2.5           -- ox_target zone radius
Config.ATM.InteractionDistance = 1.5       -- ATM-specific (tighter by default)
Config.ATM.ScanRadius          = 50.0      -- how far to scan for vanilla ATM props
```

Bump `ScanRadius` if you have large interiors and want vanilla ATMs to register from further away. Keep it reasonable (< 100m) — every tick scans nearby props.

## Disabling vanilla Fleeca ATMs

Empty the `Models` array so no prop is detected:

```lua
Config.ATM.Models = {}
```

Only entries in `Config.ATMLocations` will be usable. Useful for custom map installs where stock ATMs conflict with your map.

## Removing all default locations

Start from scratch:

```lua
Config.BankLocations = {}
Config.ATMLocations = {}
Config.DeliveryPoints = {}
```

Then add only the ones you want. The banking system still works with zero locations — `/bank` command and the `nash_banking:openBank` export don't require a physical branch.

## Adding a delivery point

Delivery points are pickup locations for physical cards, **separate** from bank branches (branches are always pickup points).

```lua
Config.DeliveryPoints = {
    { coords = vector4(707.68, -964.16, 30.40, 90.0), label = 'Point relais Pillbox' },
}
```

A blue ped + `ox_target` interaction "Récupérer un colis" spawns at each point.

## Restart

`restart nash_banking` after any edit. Blips and NPCs re-spawn on the next client tick.
