# Changelog

Full changelog with downloads:
[GitHub Releases](https://github.com/SiloTekDev/silo-libs/releases)

## v1.2.0 — 2026-08-02

**Added**

- `SiloLibs.FindCharacterByName(firstname, lastname, cb)` — character lookup
  by roleplay name straight from the framework database (works for offline
  characters). VORP reads `characters`; RSG reads `players.charinfo`.
  Callback-based by design.

**Fixed**

- Embedded-nil truncation in vorp_inventory calls: a literal `nil` in the
  middle of a cross-resource export call silently dropped every argument
  after it (`[getItemCount] Item [nil] does not exist in DB`). Fixed in
  `GetItemCount`, `GetItem`, `RemoveItemById`, `GetWeapon`,
  `CanCarryWeapons`, `CreateWeapon`, `GetInventoryItemCount`.

**Changed**

- Startup fingerprint print: `[silo-libs] vX.Y.Z loaded (framework: ...)` —
  stale or duplicate copies are spottable straight from the console.

## v1.1.0 — 2026-07-29

**Added**

- `SiloLibs.GetJobGrade(source)` — returns the character's job grade as a
  number on both VORP and RSG. Returns `0` when unavailable.

## v1.0.0 — 2026-07-21

- Initial public release: unified `SiloLibs` API with VORP adapters
  (core + inventory + weapons + custom inventories) and RSG core adapter;
  framework auto-detection at startup.
