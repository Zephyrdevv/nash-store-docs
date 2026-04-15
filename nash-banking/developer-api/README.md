# Developer API

Nash Banking exposes a public API so other resources can integrate with it.

## Sections

- [Server Exports](server-exports.md) — server-side exports (`CardPayment`, `HasActiveCard`, `InspectCard`, …)
- [Client Exports](client-exports.md) — client-side exports, including the `ox_inventory` item handlers
- [Server Events](server-events.md) — `RegisterNetEvent` events handled by the server
- [Client Events](client-events.md) — events broadcasted to the client
- [Callbacks](callbacks.md) — framework-agnostic callbacks registered by the resource
- [Commands](commands.md) — chat commands shipped with the resource

## Conventions

- All event names are prefixed with `nash_banking:`.
- All public exports live in the `nash_banking` resource namespace.
- Server exports that move money use the `Bridge.*` helpers internally — framework-agnostic.

{% hint style="warning" %}
Events prefixed with an underscore (e.g. `nash_banking:_internal`) are **internal** — do not call them from other resources; they may change without notice.
{% endhint %}
