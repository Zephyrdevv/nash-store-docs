# Server Events

## ducky_multicharacter:server:characterSelected
Fired when a player selects a character.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| charId | number | Selected character ID |

## ducky_multicharacter:server:characterCreated
Fired when a new character is created.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| charData | table | New character data |

## ducky_multicharacter:server:characterDeleted
Fired when a character is deleted.

| Parameter | Type | Description |
|-----------|------|-------------|
| source | number | Player server ID |
| charId | number | Deleted character ID |
