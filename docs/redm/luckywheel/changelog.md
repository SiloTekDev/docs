# Changelog

Version currently shipping: **1.0.0** — paired with
`Silo-LuckyWheel-Props` **1.0** and [`silo-libs`](../silo-libs/index.md).

## v1.0.0 — 2026-08-15

First release. Everything below is in the shipped build.

### Gameplay

- Streamed stand and wheel sharing one texture dictionary, `luckywheeltex`.
- ++g++ within `PromptDistance` spins for `SpinDurationMs` (9 s) over
  `MinTurns`-`MaxTurns` full rotations.
- The ratchet pointer **clacks over every wooden peg** while turning
  (`SoundTick`, throttled by `SoundTickMs`); a cash-register bell rings when
  it settles.
- Everyone within `SpectateRange` (40 m) watches the same wheel land on the
  same slice. `CooldownSec` between spins per player.
- Unlimited wheels, each spinning independently, each with its own map blip.

### Money & prizes

- The server takes the money, rolls the segment and pays the prize when the
  wheel stops. Nothing about the outcome lives on the client.
- `Prizes` per value — money, items, or both. `Chances` weight each value and
  default to how many segments physically exist on the painted wheel.
- Default prizes return about **84%** of money taken in — a 16% house edge.
- Wins of `AnnounceMinValue` (30) and up are announced within
  `AnnounceRange`.
- Item prizes are **VORP-only** today; on RSG they are skipped and logged.

### Configuration

- **VORP Core / RSG Core auto-detected**. No database, no NUI.
- **10 languages**: `en ro it de fr es pt ru pl tr`.
- `SegmentLayout` lists the 54 painted values clockwise from the pointer and
  must match the artwork; `SpinDirection` flips the landing side.
- Every sound is a native RDR2 soundset entry, previewable with `/lwsound`.
- `NotifyFunction` hook for your own notification system.

### Test mode

- `Dev = true` — debug prints plus `lwtest <wheelId> <segIndex 1-54>` for
  verifying texture alignment. Set `false` on a live server.

### Known limitations

- Props carry embedded collision, which loads only on a **full server
  start** — after the first install a plain `restart` leaves players walking
  through the stand.
