# FAQ

### Q: Can I use BagPick with QBCore?
**A:** No, BagPick is designed exclusively for the ESX framework.

### Q: How do I change the language?
**A:** Set `Config.Locale` in `config.lua` to your desired language code (e.g., `'en'`).

### Q: How do I adjust the difficulty?
**A:** Modify `Config.reussitesac` — a lower value means a higher success rate. Adjust animation timing with `Config.Tirsacmintime` and `Config.Tirsacmaxtime`.

### Q: How do I disable police alerts entirely?
**A:** Set `Config.appelpolice` to a very high number (e.g., `99999`).

### Q: Why can't certain jobs steal bags?
**A:** Jobs listed in `Config.JobsInterdits` are blocked. Remove a job from the table to allow it.

### Q: Do I need qs-dispatch?
**A:** No, qs-dispatch is optional. BagPick uses its own built-in police alert blip system.
