# Events

## Client Events

### smoke_lean:client:useSubstance
Triggers the substance use animation and effect.

| Parameter | Type | Description |
|-----------|------|-------------|
| substance | string | Item/substance name |

### smoke_lean:client:stopEffect
Stops the current screen effect early.

## Server Events

### smoke_lean:server:logUse
Logs the substance use to the server console or database.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| substance | string | Substance used |
