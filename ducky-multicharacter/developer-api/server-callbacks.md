# Server Callbacks

## ducky_multicharacter:getCharacters
Returns all characters for the requesting player.

**Returns:** table array of character data objects.

## ducky_multicharacter:createCharacter
Creates a new character for the player.

| Parameter | Type | Description |
|-----------|------|-------------|
| charData | table | Character name, dob, gender |

**Returns:** boolean success, number charId

## ducky_multicharacter:deleteCharacter
Deletes a character by ID.

| Parameter | Type | Description |
|-----------|------|-------------|
| charId | number | Character ID to delete |

**Returns:** boolean success
