# Streamed Props

Nash Banking ships with two streamed archetypes:

- A **bank card** prop, attached to the player's hand during ATM sessions and card inspections.
- A **payment terminal** (TPE) prop, held by the merchant in the P2P TPE flow.

No extra resource to install — the props are streamed directly by `nash_banking`.

## What's already set up

The `fxmanifest.lua` of `nash_banking` already declares:

```lua
files {
    'stream/nash_banking.ytyp',
    'stream/bzzz_prop_payment_terminal.ytyp',
}

data_file 'DLC_ITYP_REQUEST' 'stream/bzzz_prop_payment_terminal.ytyp'
data_file 'DLC_ITYP_REQUEST' 'stream/nash_banking.ytyp'
```

## Files in `stream/`

| File | Purpose |
|---|---|
| `nash_card.ydr` | 3D model of the physical bank card |
| `nash_banking.ytyp` | Archetype definition for `nash_card` |
| `bzzz_prop_payment_terminal.ydr` | 3D model of the TPE terminal |
| `bzzz_prop_payment_terminal.ytyp` | Archetype definition for the TPE prop |
| `READ ME.txt` | Attribution (card pack credits) |

## Attaching coordinates

Default attach bone and offsets used by Nash Banking:

- **Bone** — right hand (`57005`)
- **Card attach** — `xPos: 0.14, yPos: 0.04, zPos: -0.02, xRot: 318.0, yRot: 22.0, zRot: 2.0`

These are tuned for `mp_common / givetake1_a` and `PROP_HUMAN_ATM / ATM`. If you change the animation, you may need to tweak the offsets in `client/main.lua`.

## Troubleshooting

**The prop doesn't spawn in my hand.**

1. Make sure `nash_banking` started without errors (`restart nash_banking` and check the console).
2. Confirm the `data_file` lines above are present in `fxmanifest.lua` and point at existing files.
3. Another resource might declare the same archetype names — grep for `bzzz_prop_payment_terminal` in your resources folder.
4. On some setups you need to reconnect after the first start for the `DLC_ITYP_REQUEST` to register.

**The prop appears at the player's feet.**

This is the fallback position when the attach fails. Usually means the hand bone index isn't valid for the current ped — check you're using a human ped (not an animal or unique outfit).

## Credits

The card prop is based on **Alcaline's Wallet Pack**. Original pack and credits in `stream/READ ME.txt`.
