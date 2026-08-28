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

## The odds

`Chances` are **shares, not percentages**. Each number is divided by the sum of
all of them:

```lua
["100"] = 1   -- total 100 -> exactly 1%   (1 spin in 100)
["100"] = 1   -- total  54 -> 1.85%        (1 spin in 54)
```

!!! warning "The defaults total 54, not 100"
    One share per segment painted on the wheel, so the odds match what players
    see. That makes `["100"] = 1` a **1.85%** jackpot — roughly one every 13
    minutes of continuous play, not one in a hundred spins. This catches people
    out.

**To think in percentages**, make the shares add up to 100 — then every number
is a literal percent. `config.lua` carries a ready-made block for it. Decimals
work as well: `0.2` out of 100 is one spin in 500.

### Seeing the real odds

From the server console:

```
lwodds
```

```
odds — shares total 54  (NOT percentages: each share is divided by 54)
  VALUE  SHARE  CHANCE   1 IN N SPINS        SEGMENTS
  1      21     38.89%   1 in 3 (~1 min)     21
  2      14     25.93%   1 in 4 (~1 min)     14
  100    1      1.85%    1 in 54 (~13 min)   1
  tip: make the shares add up to 100 and each one reads as a percentage
```

It reports what the roll actually does, so a prize that is out of stock shows
`0%` and its odds already appear redistributed across the rest. Minutes assume
one player spinning without a break.

## Prize stock

Every prize takes a `stock` — how many times it can be won before the next
server restart. `0` or missing means unlimited.

```lua
["100"] = { money = 1000, label = "100x", stock = 1 },  -- one jackpot per restart
["30"]  = { money = 300, stock = 5 },                   -- five of these
["1"]   = { money = 10, stock = 0 },                    -- unlimited
```

A prize that runs out **leaves the wheel**: it is never rolled again until
restart, and the remaining values share its odds. The pointer still lands on a
painted segment — it just never lands on that value.

Nothing is written to disk. A restart refills every prize, which is what "per
restart" means.

!!! warning "`stock = 0` means unlimited, not sold out"
    Zero and a missing field both mean "no limit". To close a prize entirely,
    remove it from `Prizes` or set its `Chances` weight to 0.

If every prize is exhausted the wheel refuses to spin and tells the player so
— checked **before** they are charged, so nobody pays for a wheel that has
nothing left to give.

From the server console:

```
lwstock          # print what is left
lwstock reset    # refill without restarting
```

!!! tip "Stock is not a balance fix"
    Limiting the jackpot lowers the long-run payout, but it does not repair a
    pay table that is too generous to begin with — the wheel simply pays the
    smaller prizes more often. Get `Prizes` against `SpinCost` right first,
    then use stock to cap how often the top values can appear.

## Discord logging

Every spin can be logged to Discord — who spun, what they paid, what they won:

```lua
Webhooks = {
    Enabled  = false,          -- master switch
    Username = "Lucky Wheel",
    AvatarUrl = "",
    Win      = "",             -- every spin result
    BigWin   = "",             -- only wins of AnnounceMinValue and up
},
```

| Option | Meaning |
|--------|---------|
| `Enabled` | `false` turns all logging off, whatever the URLs say |
| `Win` | a line for **every** spin — a busy channel |
| `BigWin` | only wins of `AnnounceMinValue` and up — a quiet highlights channel |

Get a URL from **Edit Channel → Integrations → Webhooks → New Webhook**.

!!! tip "Same URL in both is safe"
    Put one URL in both slots and a big win is posted **once**, not twice.
    Leave a slot empty to switch that log off.

Each entry names the wheel, the slice it landed on, the stake (`$10` or
`1x Gold Bar`), the winnings, the character ID and — for cash spins — the net
(`+$90` / `-$40`). Net is left out when the stake was an item, since the two
sides are not comparable.

Two things worth having in the log: item prizes that **could not be
delivered** (a full satchel) are named under `NOT delivered`, and a player who
disconnects between the spin and the payout is logged as a dropped prize
rather than vanishing silently.

The messages are English regardless of `Locale` — they are read by your staff,
not by players.

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
| Prompt shows **another script's title** | Fixed in 1.3.1 — update the resource. Older builds cached the prompt text at start and the game reused that buffer slot. |
| "This wheel is out of order" | The spin cost is misconfigured — the server console names the wheel and the reason — or an item cost is running on a framework without an inventory adapter (**VORP only** today). |
| An item cost is never taken | The `item` name must match your framework's item database exactly. A wrong name gives the same "out of order" message and logs it. |
