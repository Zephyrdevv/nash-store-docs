# Configuration

All configuration is done in `config.lua`.

## General Settings

| Parameter | Default | Description | Possible Values |
|-----------|---------|-------------|-----------------|
| `Config.Locale` | `'fr'` | Language used for UI messages | `'fr'`, `'en'`, or any locale key |
| `Config.PoliceRequired` | `0` | Minimum police officers online to allow bag snatching | Any integer `0+` |
| `Config.PoliceJobs` | `{'police', 'gendarmerie'}` | Job names counted as police | Table of job name strings |

## Timing & Difficulty

| Parameter | Default | Description | Possible Values |
|-----------|---------|-------------|-----------------|
| `Config.Tirsacmintime` | `3000` | Minimum steal animation duration (ms) | Any integer (ms) |
| `Config.Tirsacmaxtime` | `10000` | Maximum steal animation duration (ms) | Any integer (ms) |
| `Config.appelpolice` | `10` | Police call chance — 1 in X chance | Any integer `1+` |
| `Config.reussitesac` | `3` | Success rate — 1 in X chance of succeeding | Any integer `1+` |

## Blip Settings

| Parameter | Default | Description |
|-----------|---------|-------------|
| `Config.Blip.Color` | `1` | Blip color on minimap |
| `Config.Blip.Scale` | `0.8` | Blip scale |
| `Config.Blip.Sprite` | `408` | Blip icon sprite ID |
| `Config.Blip.Time` | `30` | Duration blip remains visible (seconds) |

## Loot Table

```lua
Config.item = {
    { item = 'money', label = 'Money', quantite = 7 },
    { item = 'water', label = 'Water', quantite = 1 },
}
```
