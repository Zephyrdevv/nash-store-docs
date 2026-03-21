# FAQ

**Q: Why are my options not showing?**
A: Ensure ox_lib is started before ox_target and before the resource registering options.

**Q: Can I use ox_target with qb-target exports?**
A: No. ox_target has its own export API. Migrate your qb-target calls to ox_target exports.

**Q: How do I add an option to a vehicle?**
A: Use `exports.ox_target:addGlobalVehicle()` or entity-specific exports.

**Q: Can I add targets to NPC peds?**
A: Yes, use `exports.ox_target:addGlobalPed()` or `addLocalEntity()`.
