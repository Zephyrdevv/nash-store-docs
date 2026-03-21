# Common Errors

### Pickpocket option not showing on peds
**Cause:** ox_target not running or options not registered.
**Solution:** Ensure ox_target is started and nash_pickpocket is ensured after ox_target.

### Items not added to inventory after success
**Cause:** ox_inventory not running or item not in items table.
**Solution:** Ensure ox_inventory is started and all loot items exist in the items table.

### Cooldown not working
**Cause:** Config.Cooldown set to 0 or negative.
**Solution:** Set `Config.Cooldown` to a positive integer in config.lua.

### Framework not detected
**Cause:** auto-detection failed.
**Solution:** Set `Config.Framework = "esx"` or `"qb"` explicitly.
