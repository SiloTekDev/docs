# Installation

The package contains **two resources** — both go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-Dominoes` | the game itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only
ensure oxmysql

ensure silo-libs          # the framework bridge first
ensure Silo-Dominoes      # then the game
```

## 2. First start

- The database table creates itself automatically (`silo_dominoes_games`) —
  no SQL to import.
- The framework (VORP or RSG-Core) is detected automatically — nothing to
  set.
- **No streamed props** — tiles, table, chairs, animations and sounds are
  native RDR2 assets.

## 3. Add your tables

Open `config.lua` and add your locations under `Tables` (coords, heading,
label, map blip). That is the only required configuration.

!!! tip "The z coordinate does not need to be exact"
    The script detects the real floor height from the spawned table itself,
    so coordinates copied while standing at the spot (which sit about a
    meter above the floor) work fine — including on docks and piers.

## 4. Test it alone

Set `SinglePlayerBot = true` in the config, restart the resource, sit at a
table — an AI opponent with a visible NPC joins instantly, no second player
and no money needed. **Set it back to `false` when you go live.**
