# Common Errors

### Animation not playing
**Cause:** Invalid AnimDict or AnimName.
**Solution:** Verify the animation names using a GTA V animation viewer tool.

### Item not consumed
**Cause:** ox_inventory not running or item not registered.
**Solution:** Ensure ox_inventory is started and the item exists in the items table.

### Screen effect not showing
**Cause:** Invalid effect name.
**Solution:** Use a valid GTA V screen effect identifier in `Config.ScreenEffect`.

### Command not registered
**Cause:** Resource started before ox_lib.
**Solution:** Ensure ox_lib is ensured before smoke_lean in server.cfg.
