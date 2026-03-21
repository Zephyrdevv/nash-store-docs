# Events

## Client Events

### `Iz_bagpick:goanimped`
Triggers the steal animation on a target ped.

| Parameter | Type | Description |
|-----------|------|-------------|
| `pPed` | `integer` | The ped entity handle to animate on |

### `Iz_bagpick:alertPolice`
Displays police alert blip on the client map.

| Parameter | Type | Description |
|-----------|------|-------------|
| `coords` | `vector3` | World coordinates of the theft |
| `street` | `string` | Street name for the alert |

## Server Events

### `Iz_bagpick:goitem`
Grants reward items to the player after a successful theft.

| Parameter | Type | Description |
|-----------|------|-------------|
| `newItem` | `table` | Table with `item`, `label`, `quantite` |

```lua
TriggerServerEvent('Iz_bagpick:goitem', { item = 'money', label = 'Money', quantite = 7 })
```

## Server Callbacks

### `Iz_bagpick:checkCops`
Checks whether enough police officers are online.

| Returns | Type | Description |
|---------|------|-------------|
| `result` | `boolean` | `true` if enough cops are online |
