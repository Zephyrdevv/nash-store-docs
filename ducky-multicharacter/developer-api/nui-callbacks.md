# NUI Callbacks

## multichar:selectCharacter
Fired when the player clicks a character slot.

**Payload:**
```json
{
  "charId": 1
}
```

## multichar:createCharacter
Fired when the player submits the character creation form.

**Payload:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "dob": "1990-01-01",
  "gender": "male"
}
```

## multichar:deleteCharacter
Fired when the player confirms character deletion.

**Payload:**
```json
{
  "charId": 2
}
```
