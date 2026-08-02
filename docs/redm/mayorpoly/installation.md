# Installation

The package contains **three resources** — all three go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `Silo-Mayorpoly-Props` | streamed props (board, pieces, deed cards, textures) — **must start first** |
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-Mayorpoly` | the game itself |

## 1. server.cfg — start order matters

```
ensure oxmysql
ensure silo-libs
ensure Silo-Mayorpoly-Props
ensure Silo-Mayorpoly
```

## 2. First start

- The database table is created automatically the first time the resource
  starts — **no SQL file to import**.
- The framework (VORP or RSG-Core) is detected automatically — nothing to set.

`Silo-Mayorpoly-Props` streams sixteen models: the board, the two card piles,
the house and the hotel, and eleven property cards — one per colour group, one
each for the water and electric companies, and one for the railroads. They all
share a single texture dictionary, `monopolytex`.

## 3. Place your tables

Open `config.lua` and add your locations under `Tables` (coords, heading,
label, map blip). That is the only required configuration.

```lua
Tables = {
    { coords = vector3(-346.14, 795.83, 115.14), heading = 0.0,
      label = "Mayorpoly Valentine", blip = -1081805875 },
}
```

## 4. Test it alone

Set `SinglePlayerBot = true` and sit down alone. AI players fill the other
seats and the game starts at once, with no buy-in and nothing written to the
database. They roll, buy, build, trade and go bankrupt on their own.

!!! warning
    Set `SinglePlayerBot = false` again before going live.

!!! tip "Editing the interface"
    After editing anything in `html/`, **restart the resource**. The interface
    files are cached and a plain refresh will keep showing you the old ones.
