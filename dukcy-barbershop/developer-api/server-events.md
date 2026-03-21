# Server Events

## dukcy_barbershop:server:saveAppearance
Saves the player appearance to the database.

| Parameter | Type | Description |
|-----------|------|-------------|
| appearance | table | Appearance data table |

## dukcy_barbershop:server:chargePlayer
Deducts the barber cost from the player account.

| Parameter | Type | Description |
|-----------|------|-------------|
| amount | number | Amount to charge |
| category | string | hair, beard, eyebrows, makeup |
