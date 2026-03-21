# Installation

## Prerequisites

- [es\_extended](https://github.com/esx-framework/esx-legacy) (ESX Framework)
- [mysql-async](https://github.com/brouznouf/fivem-mysql-async) or compatible MySQL resource
- [ox\_target](https://github.com/overextended/ox_target) — Targeting system
- [vms\_notify](https://vms-store.com) — Notification system

## Steps

1. **Download** the `bagpick` resource and place it into your server's `resources/` directory.

2. **Ensure dependencies** in your `server.cfg`:
   ```cfg
   ensure es_extended
   ensure mysql-async
   ensure ox_target
   ensure vms_notify
   ```

3. **Add the resource** to your `server.cfg`:
   ```cfg
   ensure bagpick
   ```

4. **Verify items** (`money`, `water`) exist in your inventory system.

5. **Restart your server** and verify no errors in the console.

> **Note:** No SQL import is required.
