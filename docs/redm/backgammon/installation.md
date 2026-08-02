# Installation

The package contains **three resources** — all three go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `Silo-Backgammon-Props` | streamed props (board, checkers, textures) — **must start first** |
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-Backgammon` | the game itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only
ensure oxmysql

ensure Silo-Backgammon-Props   # props FIRST — the game needs the models
ensure silo-libs          # then the framework bridge
ensure Silo-Backgammon    # then the game
```

## 2. First start

- The database table creates itself automatically
  (`silo_backgammon_games`) — **no SQL to import**.
- The framework (VORP or RSG-Core) is detected automatically — nothing to
  set.

## 3. Add your tables

Open `config.lua` and add your locations under `Tables` (coords, heading,
label, map blip). That is the only required configuration.

```lua
Tables = {
    { coords = vector3(-173.92, 626.60, 113.03), heading = 145.25,
      label = "Backgammon Valentine", blip = -1081805875 },
}
```

## 4. Test it alone

Set `SinglePlayerBot = true` in the config, restart the resource, sit at a
table — an AI opponent (with a visible NPC) joins instantly, no second player
and no money needed.

!!! warning
    Set `SinglePlayerBot = false` again before going live.
