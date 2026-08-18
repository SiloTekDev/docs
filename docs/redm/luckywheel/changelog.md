# Changelog

Version currently shipping: **1.1.0** — paired with
`Silo-LuckyWheel-Props` **1.0** and [`silo-libs`](../silo-libs/index.md).

## v1.1.0 — 2026-08-18

### Added

- **A spin can cost an inventory item instead of cash.** `SpinCost` names what
  is taken and how much:

    ```lua
    SpinCost = { type = "money", amount = 10 },                                   -- $10
    SpinCost = { type = "money", amount = 0 },                                    -- free spin
    SpinCost = { type = "item", amount = 1, item = "goldbar", label = "Gold Bar" },
    ```

- **A price per wheel.** Any `Wheels` entry can carry its own `cost = { ... }`
  and ignore the global `SpinCost` — a cash wheel in Valentine and a gold-bar
  wheel in Saint Denis, side by side. The prompt shows the price of the wheel
  you are standing at.
- A misconfigured price is printed with the wheel's label on server start, and
  that wheel refuses to spin instead of quietly becoming free.

### Changed

- `SpinCost = 10` — the old plain number — still works and still means
  dollars. Existing configs need no edit.
- A spin interrupted by a resource stop refunds **whatever was staked**: the
  item returns to the satchel, cash returns to the wallet.
- Seven new locale strings in all ten languages.

### Notes

- Item costs need a framework inventory silo-libs supports — **VORP today**.
  On RSG the spin is refused with a message to the player and a console line;
  it never becomes free. Money costs work on both frameworks.

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
