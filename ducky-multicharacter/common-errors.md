# Common Errors

### Characters not loading
**Cause:** oxmysql not running or SQL not imported.
**Solution:** Ensure oxmysql is started before ducky_multicharacter and SQL is imported.

### NUI selection screen blank
**Cause:** NUI build missing.
**Solution:** Re-download and replace the web/ folder.

### Framework not detected
**Cause:** Config.Framework set incorrectly.
**Solution:** Set `Config.Framework = "esx"` or `"qb"` explicitly in config.lua.

### Error on character creation
**Cause:** Missing columns in database.
**Solution:** Re-import the provided SQL file to ensure all columns exist.
