# Configuration

Everything lives in `config.lua`. Every piece of text lives in
`locales/locales.lua`.

## Wheels

```lua
Wheels = {
    { coords = vector3(3283.25, -1330.62, 41.74), heading = 90.0,
      label = "Lucky Wheel Valentine", blip = -1081805875 },
},
```

`coords` is the **base of the stand**, not its centre. `blip = false` hides it
from the map.

## The price of a spin

`SpinCost` says what one spin costs — cash **or** an inventory item, never
both:

```lua
SpinCost = { type = "money", amount = 10 },                                   -- $10
SpinCost = { type = "money", amount = 0 },                                    -- free spin
SpinCost = { type = "item", amount = 1, item = "goldbar", label = "Gold Bar" },
```

| Field | Meaning |
|--------|---------|
| `type` | `"money"` takes cash, `"item"` takes an item from the satchel |
| `amount` | dollars, or how many items |
| `item` | the item name **as it exists in your framework's item database** |
| `label` | how the price reads in the prompt and in messages (`false` = the raw item name) |

The old one-line form still works and still means dollars:

```lua
SpinCost = 10,
```

### A different price per wheel

Any entry in `Wheels` can carry its own `cost`, in the same shape, and ignore
the global `SpinCost`:

```lua
Wheels = {
    { coords = vector3(3283.25, -1330.62, 41.74), heading = 90.0,
      label = "Lucky Wheel Valentine", blip = -1081805875 },
    { coords = vector3(2635.75, -1225.30, 53.30), heading = 0.0,
      label = "High Roller Wheel", blip = -1081805875,
      cost = { type = "item", amount = 1, item = "goldbar", label = "Gold Bar" } },
},
```

The world prompt shows the price of the wheel you are standing at — `Spin
($10)` at one, `Spin (1x Gold Bar)` at the other.

!!! warning "Item costs are VORP-only today"
    Charging an item needs a framework inventory that silo-libs supports —
    the same limitation item *prizes* have. On RSG the player is told the
    wheel is out of order and the reason is logged to the server console; the
    spin is **never** given away for free. Cash costs work on both
    frameworks.

!!! tip "A broken price is reported by name"
    `type = "item"` with no `item`, an item `amount` of 0, or a misspelled
    `type` is printed with the wheel's label when the resource starts, and
    that wheel refuses to spin until it is fixed — it never silently becomes
    free. For a genuinely free wheel use `{ type = "money", amount = 0 }`.

## Prizes

| Option | Default | Meaning |
|--------|---------|---------|
| `SpinCost` | `$10` | price of one spin — cash or item (above) |
| `Prizes` | see below | what every wheel value pays |
| `Chances` | matches the paint | how often each value is rolled |

The default `Chances` mirror how many segments of each value physically exist
on the wheel (21 slices of "1", one of "100x", and so on), so the odds are
exactly what players see. With the default `Prizes` the wheel returns about
**84%** of money taken in on a $10 spin — a house edge of 16%. The maths is
yours to tune, per value. Charging an item instead? Then the money prizes are
pure profit for the players — price the item to match, or pay out in items
too.

Items ride along with money:

```lua
["100"] = { money = 135, label = "100x",
            items = { { item = "goldbar", amount = 1 } } },
```

`label` controls how the value is named in notifications ("100x" instead of
"100").

!!! warning "Item prizes are VORP-only today"
    Item prizes need a framework inventory that silo-libs supports. On RSG,
    item entries are skipped and logged to the server console — money still
    pays normally. If an item cannot be delivered (full inventory) the win
    message lists only what actually arrived and the console records the rest.

## The spin

| Option | Default | Meaning |
|--------|---------|---------|
| `SpinDurationMs` | `9000` | how long the wheel spins |
| `MinTurns` / `MaxTurns` | `4` / `6` | full rotations before stopping |
| `CooldownSec` | `5` | wait between spins, per player |
| `SpectateRange` | `40.0` | who sees the spin, in metres |

## Announcements

| Option | Default | Meaning |
|--------|---------|---------|
| `AnnounceMinValue` | `30` | announce wins of this value and up (`false` = never) |
| `AnnounceRange` | `50` | metres that hear it (`0` = whole server) |

## Sounds

Native RDR2 sounds, every entry `{ soundset, sound }`, any entry `false` to
mute. Out of the box the pointer **clacks over every wooden peg** while the
wheel spins (`SoundTick`, throttled by `SoundTickMs`) and a cash-register bell
rings when it settles (`SoundStop`).

```
/lwsound <soundset> <sound>
```

The soundset browser lives at
[github.com/femga/rdr3_discoveries](https://github.com/femga/rdr3_discoveries)
under `audio`.

## The wheel face (advanced)

`SegmentLayout` lists the **54 values painted on the shipped texture**,
clockwise from the pointer. The winning segment is chosen from this list, so it
must always match what players actually see.

Leave it alone unless you repaint the wheel texture. If you do, `Dev = true`
gives you a server-console command to verify the alignment slice by slice:

```
lwtest <wheelId> <segIndex 1-54>
```

If the wheel visibly lands one slice to the *wrong side* of the pointer, flip
`SpinDirection`.

## Language

`Locale = "en"` — 10 languages included (`en ro it de fr es pt ru pl tr`).
Add your own by copying the `en` block in `locales/locales.lua`.

## Notifications

```lua
function NotifyFunction(source, text)
    -- auto-detects VORP / RSG; replace with your own system if needed
end
```

## Troubleshooting

| Symptom | Cause / fix |
|---|---|
| Props are **black** | `luckywheeltex.ytd` missing from the props resource, renamed, or too large to stream. Keep the shipped file as-is. |
| You **walk through** the stand | Collision loads only on a **full server start** — `restart` is not enough after the first install. |
| Pressing ++g++ does nothing | `silo-libs` not started before this resource, or a script error in the server console. |
| Wheel lands on the **wrong slice** | Flip `SpinDirection`, then verify with `lwtest` (`Dev = true`). |
| No prompt at the wheel | You are farther than `PromptDistance`, or the wheel's coords are wrong (check F8 for spawn errors). |
| "This wheel is out of order" | The spin cost is misconfigured — the server console names the wheel and the reason — or an item cost is running on a framework without an inventory adapter (**VORP only** today). |
| An item cost is never taken | The `item` name must match your framework's item database exactly. A wrong name gives the same "out of order" message and logs it. |
