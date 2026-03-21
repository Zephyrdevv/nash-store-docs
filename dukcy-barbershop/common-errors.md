# Common Errors

### Barber NUI not opening
**Cause:** NUI build missing or ox_target not running.
**Solution:** Ensure ox_target is started and the web/ folder is present.

### Changes not persisting
**Cause:** Database not configured or SQL not imported.
**Solution:** Import the provided SQL file and verify your database credentials.

### nil value on Config.Framework
**Cause:** Framework not detected.
**Solution:** Explicitly set `Config.Framework = "esx"` or `"qb"` in config.lua.

### Barber location not targetable
**Cause:** ox_target not registering the zone.
**Solution:** Restart dukcy_barbershop after ox_target is fully loaded.
