# Guides

## Showing a Notification

```lua
lib.notify({
    title = "Purchase Complete",
    type = "success",
    duration = 3000
})
```

## Using a Progress Bar

```lua
local completed = lib.progressBar({
    duration = 5000,
    label = "Searching...",
    useWhileDead = false,
    canCancel = true
})

if completed then
    -- action completed
end
```

## Opening an Input Dialog

```lua
local input = lib.inputDialog("Enter Details", {
    { type = "input", label = "Your Name", required = true },
    { type = "number", label = "Amount" }
})

if input then
    print(input[1], input[2])
end
```

## Showing an Alert Dialog

```lua
local confirmed = lib.alertDialog({
    header = "Confirm Action",
    content = "Are you sure?",
    centered = true,
    cancel = true
})

if confirmed == "confirm" then
    -- user confirmed
end
```
