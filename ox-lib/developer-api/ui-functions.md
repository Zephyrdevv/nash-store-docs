# UI Functions

## lib.notify(data)
Displays a notification to the player.

| Parameter | Type | Description |
|-----------|------|-------------|
| data.title | string | Notification title |
| data.type | string | inform, success, warning, error |
| data.duration | integer | Duration in ms |

## lib.progressBar(data)
Displays a progress bar. Returns true if completed.

## lib.inputDialog(heading, rows)
Opens a form input dialog. Returns array of values or nil.

## lib.alertDialog(data)
Displays a confirmation dialog. Returns confirm or cancel.
