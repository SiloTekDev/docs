# Installation

The package contains **three resources** — all three go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `Silo-LuckyWheel-Props` | streamed props (stand, wheel, textures) — **must start first** |
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-LuckyWheel` | the game itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only — needed for item prizes and item spin costs

ensure Silo-LuckyWheel-Props   # props FIRST — the game needs the models
ensure silo-libs          # then the framework bridge
ensure Silo-LuckyWheel    # then the game
```

**No database.** Nothing to import, nothing to configure for storage.

## 2. First start

!!! warning "Full server start, not `restart`"
    The props carry embedded collision, and collision only loads on a **full
    server start**. If you can walk through the stand after the very first
    install, stop the server and start it again — a plain `restart` does not
    reload collision.

## 3. Place your wheels

Open `config.lua` and add locations under `Wheels`:

```lua
Wheels = {
    { coords = vector3(3283.25, -1330.62, 41.74), heading = 90.0,
      label = "Lucky Wheel Valentine", blip = -1081805875 },
},
```

- `coords` is where the **base of the stand** touches the ground.
- The model is **3.8 m tall** — give it a wall or a saloon corner.
- `blip = false` hides the wheel from the map.
- Add as many as you like; each spins independently.

## 4. Set the price

`SpinCost` says what one spin costs — cash **or** an inventory item:

```lua
SpinCost = { type = "money", amount = 10 },                                   -- $10
SpinCost = { type = "item", amount = 1, item = "goldbar", label = "Gold Bar" },
```

Any wheel can override it with its own `cost = { ... }`, so a cash wheel and
a gold-bar wheel can stand in the same town. Full reference on the
[Configuration](configuration.md#the-price-of-a-spin) page.

!!! warning "Item costs are VORP-only today"
    Charging an item needs a framework inventory adapter — the same
    limitation item *prizes* have. On RSG the player is told the wheel is out
    of order and the reason is logged; the spin is never given away free.
    Cash costs work on both frameworks.

If the server console names a wheel and a broken cost on start, fix it before
going live — that wheel refuses to spin until you do.

## 5. Test it

Set `Dev = true` for debug prints and the alignment command:

```
lwtest <wheelId> <segIndex 1-54>
```

Preview sounds in game with `/lwsound <soundset> <sound>`.

!!! warning
    Set `Dev = false` again before going live.
