# Common Errors

### Shop target not appearing
**Cause:** ox_target not running or coords incorrect.
**Solution:** Verify ox_target is started and shop coords match the actual in-game position.

### Item not found in inventory
**Cause:** Item name mismatch between config and ox_inventory items table.
**Solution:** Ensure item names in Config.Shops match exactly with your ox_inventory items.

### Purchase fails silently
**Cause:** Player has insufficient funds or ox_inventory is not running.
**Solution:** Check server console for errors and ensure ox_inventory is ensured before nash_shops.

### Robbery cooldown not working
**Cause:** Config.RobberyTimeout not set.
**Solution:** Set a positive integer value for `Config.RobberyTimeout` in config.lua.
