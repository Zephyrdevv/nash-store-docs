# Common Errors

### `SCRIPT ERROR: @bagpick/...` — es_extended not found
**Cause:** ESX is not started before BagPick.
**Solution:** Ensure `ensure es_extended` appears before `ensure bagpick` in `server.cfg`.

### ox_target interactions not showing on NPCs
**Cause:** `ox_target` is not installed or not started properly.
**Solution:** Verify `ox_target` is ensured before `bagpick`.

### Notifications not appearing
**Cause:** `vms_notify` is missing or not started.
**Solution:** Install `vms_notify` and ensure it before `bagpick`.

### Items not received after successful theft
**Cause:** Items in `Config.item` do not exist in your inventory system.
**Solution:** Register the items in your inventory resource.
