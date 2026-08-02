# RedM Scripts

Premium RedM resources by SiloTek. Every script is **server-authoritative**,
localized in **multiple languages**, and runs on **VORP and RSG** through the
free [silo-libs](silo-libs/index.md) bridge — the framework is auto-detected,
nothing to configure.

## The family

| Resource | What it is | Get it |
|----------|------------|--------|
| **[silo-libs](silo-libs/index.md)** | Free, open-source framework bridge (VORP / RSG) every SiloTek script builds on | [GitHub — free](https://github.com/SiloTekDev/silo-libs) |
| **[Silo-Backgammon](../redm/backgammon/index.md)** | Two-player 3D backgammon on physical tables, with betting, spectators and crash recovery | [silotek.dev](https://silotek.dev) |
| **[Silo-PonyExpress](../redm/ponyexpress/index.md)** | Player-driven delivery job: courier board, XP & levels, express orders, parcel lockers | [silotek.dev](https://silotek.dev) |

## Shared principles

All SiloTek scripts follow the same rules:

- **Server-authoritative** — money, items and game state only ever change on
  the server. A modified client cannot pay itself, spawn goods or skip checks.
- **Open config, escrowed code** — everything a server owner needs to edit
  (`config.lua`, locales, SQL) ships unencrypted; the core logic is protected
  by CFX Asset Escrow.
- **Automatic database setup** — tables create and upgrade themselves on
  first start. No SQL files to import.
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
