# Changelog

Version currently shipping: **1.0.1** — paired with
`Silo-Backgammon-Props` **1.2** and
[`silo-libs`](../silo-libs/index.md) **1.0.0 or newer**.

## v1.0.1 — 2026-08-09

Package revision. The game code is unchanged from 1.0.0; only the manifest
version was raised for the repackaged Keymaster upload.

## v1.0.0 — 2026-07-29

First release. Everything below is in the shipped build.

### Gameplay

- Two-player backgammon on a **physical 3D table** — streamed board and
  checkers, native `p_dice01x` dice.
- **Direct board interaction**: movable checkers lift on your turn, clicking
  one shows ghost checkers on every legal destination, clicking the board
  rolls, right-click cancels.
- Complete rules engine: bar and re-entry, bearing off, doubles, gammon
  detection, opening roll with re-roll on a tie.
- **Three cameras** on ++v++ — top-down, first person, cinematic side shot.
- **Spectators** within `SpectateRange` (12 m) see the live board, checkers
  and dice.
- Automatic sitting animation; optional script-spawned table and chairs with
  full seat fine-tuning.

### Betting

- Either seated player proposes a stake; the other accepts or declines.
- On accept both stakes are **escrowed immediately**, bounded by `MinBet` (1)
  and `MaxBet` (500).
- The pot pays out to the winner automatically. `GammonMultiplier` announces
  a "Gammon!" win — announcement only, the pot is unchanged.

### Crash-safety

- Every move is written to `silo_backgammon_games`, created automatically at
  boot. No SQL to import.
- A disconnect marks the match **interrupted** and reserves the table; when
  both players sit back down it resumes from the exact position, remaining
  dice included.
- Server restarts restore all running games.
- `AbandonClaimMinutes` (30) — after that the present player claims the pot.
- Standing up or walking past `ForfeitDistance` without returning within
  `ForfeitGraceSeconds` forfeits the match.

### Security

- Dice rolled **on the server only**.
- Every move validated server-side against the full rules engine.
- All money movement server-side, through `silo-libs`.
- Per-player rate limiting on every player-triggered net event.

### Configuration

- **VORP Core / RSG Core auto-detected**; oxmysql for storage.
- Unlimited tables — four ship by default (Valentine, Wallace, Rhodes,
  Saint Denis).
- **10 languages**: `en de fr it es pt ro ru pl tr`.
- Every sound is a native RDR2 soundset entry, individually mutable, with
  in-game preview via `/bgsound` and `/bgsoundf`.
- `NotifyFunction` hook for your own notification system.

### Admin

- `/bgcancel <tableId>` — console or ace `silo-bg.admin`. Force-cancels a
  match and refunds online players.

### Test mode

- `SinglePlayerBot` with a visible NPC opponent — no betting, no database
  writes. Set `false` on a live server.

### Known limitations

- The strict "must play both dice if possible" rule is simplified: every
  single move is validated, but the engine does not search ahead for the
  sequence that maximises dice usage.
- The doubling cube is not implemented.
