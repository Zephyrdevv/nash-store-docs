# Configuration

## config.lua

| Option | Default | Description |
|--------|---------|-------------|
| Config.Framework | auto | auto, esx, qb |
| Config.Currency | money | money or bank |
| Config.CameraEnabled | true | Show camera preview |
| Config.Locations | table | List of barber shop locations |

## Adding a Location

```lua
Config.Locations = {
    {
        coords = vec3(1212.0, -472.0, 66.0),
        heading = 45.0,
        label = "Fade Masters"
    }
}
```

## Prices

```lua
Config.Prices = {
    hair = 50,
    beard = 30,
    eyebrows = 20,
    makeup = 40
}
```
