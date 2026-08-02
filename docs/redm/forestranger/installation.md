# Installation

The package contains **two resources** — both go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-ForestRanger` | the job itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only
ensure oxmysql

ensure silo-libs          # framework bridge first
ensure Silo-ForestRanger  # then the job
```

!!! tip "silo-libs 1.1.0+ recommended"
    The management sidebar reads the ranger's job grade through
    `SiloLibs.GetJobGrade`. On an older silo-libs the script falls back to
    reading the grade straight from VORP / RSG, so nothing breaks — but the
    bridge is the cleaner path.

## 2. First start

- The database tables create themselves automatically
  (`silo_forestranger_rangers`, `silo_forestranger_log`) — **no SQL to
  import**.
- The framework (VORP or RSG-Core) is detected automatically — nothing to set.

## 3. Place your stations and zones

Open `config.lua`:

- `Stations` — where shifts start and end, and where lost travellers and
  hogtied poachers are handed over. The defaults sit near Wallace Station,
  Strawberry and Van Horn; move them to your own ranger posts.
- `Zones` — the forest areas incidents can spawn in (centre + radius).
  Incidents only appear around a ranger who is **inside** one of these zones,
  so keep them on actual woodland.

```lua
Stations = {
    { coords = vector3(-1303.61, 388.78, 95.37), label = "Wallace Station",
      blip = "blip_ambient_law" },
},

Zones = {
    { label = "Big Valley", coords = vector3(-1600.0, 480.0, 100.0),
      radius = 700.0 },
},
```

## 4. Test it

Set `Dev = true` and go on duty. These commands **only exist while
`Dev = true`**:

| Command | What it does |
|---------|--------------|
| `/rangerhere <type>` | spawns the incident **25–60 m from you**, anywhere on the map — the fastest way to test |
| `/rangerevent <type>` | spawns it the real way: random spot in the patrol zone you are standing in |
| `/rangerduty` | toggles your shift from anywhere — no station needed |
| `/rangerclear` | removes every running incident so the next test starts clean |
| `/rangertypes` | lists the type names |

Types: `forest_fire`, `sick_animal`, `poachers`, `lost_tourist`,
`fallen_tree`, `wolves`. Without a type, a random one is picked by weight.

!!! warning
    Set `Dev = false` again before going live — those commands let any player
    spawn incidents and toggle duty from anywhere.
