# RedM Scripts

Premium RedM resources by SiloTek. Every script is **server-authoritative**,
localized in **multiple languages**, and runs on **VORP and RSG** through the
free [silo-libs](silo-libs/index.md) bridge — the framework is auto-detected,
nothing to configure.

## The family

| Resource | What it is | Get it |
|----------|------------|--------|
| **[silo-libs](silo-libs/index.md)** | Free, open-source framework bridge (VORP / RSG) every SiloTek script builds on | [GitHub — free](https://github.com/SiloTekDev/silo-libs) |
| **[Silo-Backgammon](backgammon/index.md)** | Two-player 3D backgammon on physical tables, with betting, spectators and crash recovery | [silotek.dev](https://silotek.dev) |
| **[Silo-Mayorpoly](mayorpoly/index.md)** | 2–6 player 3D frontier property board game: real board, real deeds on the table, trading, breaks and crash recovery | [silotek.dev](https://silotek.dev) |
| **[Silo-DuelDice](dueldice/index.md)** | Street dice duel — one item, two kneeling players, no UI at all | [silotek.dev](https://silotek.dev) |
| **[Silo-PonyExpress](ponyexpress/index.md)** | Player-driven delivery job: courier board, XP & levels, express orders, parcel lockers | [silotek.dev](https://silotek.dev) |
| **[Silo-ForestRanger](forestranger/index.md)** | Forest ranger job: six random incident types, Eagle Eye tracking, vanilla arrests, ranks and shared payouts | [silotek.dev](https://silotek.dev) |
| **[Silo-LuckyWheel](luckywheel/index.md)** | A hand-painted 1899 wheel of fortune standing in the world — server-rolled slices, pay to spin with cash or an item, money or item prizes, big-win announcements | [silotek.dev](https://silotek.dev) |

### Games or jobs?

- **Minigames** — [Silo-Backgammon](backgammon/index.md),
  [Silo-Mayorpoly](mayorpoly/index.md), [Silo-DuelDice](dueldice/index.md),
  [Silo-LuckyWheel](luckywheel/index.md)
- **Jobs** — [Silo-PonyExpress](ponyexpress/index.md),
  [Silo-ForestRanger](forestranger/index.md)

## Shared principles

All SiloTek scripts follow the same rules:

- **Server-authoritative** — money, items and game state only ever change on
  the server. A modified client cannot pay itself, spawn goods or skip checks.
- **Open config, escrowed code** — everything a server owner needs to edit
  (`config.lua`, locales, SQL) ships unencrypted; the core logic is protected
  by CFX Asset Escrow.
- **Automatic database setup** — where a script uses a database, its tables
  create and upgrade themselves on first start. No SQL files to import.
- **One notification hook** — `NotifyFunction` at the bottom of every config
  plugs your notification system in with one line (VORP / RSG auto-detected
  by default).

!!! tip "Start order matters"
    `silo-libs` must start **after** your framework and **before** any
    SiloTek script. Streamed prop resources (e.g. `Silo-Backgammon-Props`)
    start first of all. Every install guide shows the exact `server.cfg`
    block.

## Support

Questions, bugs, feature requests: [Discord](https://discord.gg/T5T5ZsGEsU).
