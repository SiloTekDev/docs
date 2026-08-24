# Changelog

Version currently shipping: **1.1.1** — paired with
[`silo-libs`](../silo-libs/index.md) **1.1.0 or newer**. No streamed props.

## v1.1.1 — 2026-08-24

NUI hardening: `html/app.js` now ships obfuscated (same filename) and its
callback URLs are hardcoded to the resource name. No gameplay changes.

## v1.1.0 — 2026-08-02

**Added**

- **Management sidebar** at the station for rangers holding the job at
  `BossGrade` or higher: hire any player by server ID, promote (up to
  `BossGrade`) and fire (moved to `FiredJob`) the rangers currently online.
  Works with any `RequireJob` setting. Reads the grade through
  `SiloLibs.GetJobGrade` (**silo-libs 1.1.0+**), falling back to VORP / RSG
  directly on older silo-libs.
- `/rangerjob` diagnostic (`Dev = true`) — prints exactly what the job gate
  reads for you: `job=… grade=… canWork=… boss=…`. The first thing to run
  when duty or the management panel misbehaves.

## v1.0.0 — 2026-07-30

First release. Everything below is in the shipped build.

### The job

- Shifts start and end at a ranger station. No fixed routes — the server
  generates incidents around a ranger who is **inside** a patrol zone, every
  `MinEventMinutes`-`MaxEventMinutes`.
- Six incident types, each with its own weight, pay, XP, time limit and
  tuning: `forest_fire`, `sick_animal`, `poachers`, `lost_tourist`,
  `fallen_tree`, `wolves`.
- Incidents are announced within `AlertRadius` and appear as a map blip. An
  on-screen **compass** shows direction, distance, remaining time and
  progress. The GPS route is **off by default** — `/rangergps` or the station
  panel enables it.
- Every field action is one key held down (++e++ by default).

### Progression & pay

- Every ranger on duty within `ParticipationRange` when an incident is solved
  is rewarded — full amount each or split (`SharedPayoutMode`).
- Paid on the spot or at shift end (`PayoutMode`). Unpaid money survives
  disconnects.
- XP, levels and ranks: `XpPerLevel`, `LevelBonus`, `MaxLevel`, `Ranks`.
  **Top Rangers** leaderboard at the station.

### Multiplayer scene handling

- Fires, fallen trees and camp props are **local** on every client but placed
  at server-generated coordinates — zero networked objects.
- Peds are **networked** and spawned by a single elected host client. If the
  host leaves, the server reassigns and the scene is rebuilt.

### Security

- The server decides **which** incident spawns, **where** and **when**; the
  client's proposed ground is re-validated.
- Every field action is validated server-side — on duty, close enough,
  correct incident and state, required tool, rate-limited.
- Money and XP are computed on the server. Poacher surrender rolls too.

### Configuration

- **VORP Core / RSG Core auto-detected**; oxmysql for storage, tables
  auto-create at boot.
- `RequireJob` / `JobNames` / `AllowSelfEmploy`, `Uniform`, `RequiredItems`,
  `Webhooks` for Discord logging.
- **10 languages**: `en ro it de fr es pt ru pl tr`.
- **Seven config hooks** and **six exports** (four server, two client).
- Headless test suite: `lua test/test_ranger.lua`.

### Known limitations

- Wildfires use RDR2 script fires — visual and dangerous up close, but they
  do not permanently scar the map.
- `RequiredItems` works fully on **VORP only**; the RSG inventory adapter in
  silo-libs is still a stub.
- Events spawn only around rangers **inside** a patrol zone.
