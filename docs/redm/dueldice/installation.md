# Installation

The package contains **two resources** — both go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs) |
| `Silo-DuelDice` | the game itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only

ensure silo-libs          # framework bridge first
ensure Silo-DuelDice      # then the game
```

## 2. First start

- The framework (VORP or RSG-Core) is detected automatically — nothing to set.
- **No database, no SQL file, no streamed props** — the dice are a model the
  game already ships.

## 3. Add the item

Add the item from `Config.DiceItem` (default `zaruri`) to your framework's item
database and make it **usable**. That is the only required setup.

`ConsumeItem` decides whether the item is used up each time a duel starts —
`false` by default, so one set of dice lasts forever.
