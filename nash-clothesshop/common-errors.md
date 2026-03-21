# Common Errors

### Clothing NUI not opening
**Cause:** ox_target not running or NUI build missing.
**Solution:** Ensure ox_target is started and the web/ folder is present.

### Outfits not persisting
**Cause:** Database not set up or oxmysql not running.
**Solution:** Import the SQL file and ensure oxmysql is ensured before nash_clothesshop.

### Items showing incorrect prices
**Cause:** Price table not configured for the shop type.
**Solution:** Check the price configuration in config.lua for the relevant shop type.

### Player model reverts after purchase
**Cause:** Framework clothing save not triggered.
**Solution:** Ensure your framework's character save function is called on purchase.
