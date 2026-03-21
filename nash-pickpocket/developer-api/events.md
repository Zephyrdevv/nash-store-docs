# Events

## Client Events

### nash_pickpocket:client:attempt
Triggers a pickpocket attempt on the nearest ped.

### nash_pickpocket:client:success
Fired on successful pickpocket.

| Parameter | Type | Description |
|-----------|------|-------------|
| item | string | Item name received |
| amount | number | Quantity received |

### nash_pickpocket:client:fail
Fired on failed pickpocket attempt.

## Server Events

### nash_pickpocket:server:giveItem
Gives the stolen item to the player's inventory.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| item | string | Item name |
| amount | number | Quantity |

### nash_pickpocket:server:alertPolice
Sends a police dispatch alert on failure.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| coords | vector3 | Location of the attempt |
