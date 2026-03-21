# Common Errors

### No such export getVersion
**Cause:** Another version of ox_lib is still running.
**Solution:** Ensure only one ox_lib resource exists.

### UI components look unstyled
**Cause:** NUI build files missing or corrupted.
**Solution:** Re-download and replace the web/ folder.

### Callbacks not responding
**Cause:** ox_lib started after the resource registering callbacks.
**Solution:** Ensure ox_lib is ensured before all dependent resources.
