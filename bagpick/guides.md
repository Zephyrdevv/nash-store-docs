# Guides

## Customizing the Loot Table

```lua
Config.item = {
    { item = 'money',     label = 'Cash',       quantite = 10 },
    { item = 'phone',     label = 'Phone',      quantite = 1  },
    { item = 'goldchain', label = 'Gold Chain', quantite = 1  },
}
```

## Adding Custom Police Jobs

```lua
Config.PoliceJobs = {'police', 'gendarmerie', 'sheriff', 'fbi'}
```

## Blacklisting Additional Jobs

```lua
Config.JobsInterdits = {
    ['police']         = true,
    ['ambulance']      = true,
    ['my_custom_job']  = true,
}
```
