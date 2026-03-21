# NUI Callbacks

smoke_lean does not use a full NUI menu. The following callbacks are used for minimal UI interactions.

## lean:close
Closes any open UI element and cancels the use action.

## lean:confirm
Confirms the use action when a confirmation prompt is enabled.

**Payload:**
```json
{
  "substance": "lean"
}
```
