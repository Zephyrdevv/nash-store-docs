# Common Errors

### Target menu not appearing
**Cause:** ox_target started before ox_lib or NUI build missing.
**Solution:** Ensure `ensure ox_lib` comes before `ensure ox_target` in server.cfg.

### Options registered but not visible
**Cause:** Entity not in range or raycast disabled.
**Solution:** Check DrawDistance config and ensure RaycastEntities is true.

### Export not found: ox_target
**Cause:** ox_target resource not started.
**Solution:** Add `ensure ox_target` to server.cfg after ox_lib.

### Zone options not showing
**Cause:** Player not inside the zone bounds.
**Solution:** Enable `BoxZoneDebug` temporarily to verify zone placement.
