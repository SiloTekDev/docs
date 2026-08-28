# Changelog

Version currently shipping: **1.0.1** — paired with
[`silo-libs`](../silo-libs/index.md) and `oxmysql`. No streamed props — every
tile, table and chair is a native RDR2 asset.

## [1.0.1] - 2026-08-28

### Fixed

- **Chairs could land under a raised platform.** `PlaceObjectOnGroundProperly`
  raycasts down from the prop's own position; a chair spawned exactly level
  with a platform surface could start the ray a hair below it and snap to the
  terrain underneath. Furniture now spawns **0.5 m above** the detected floor
  before snapping, and a chair that still ends up more than 0.75 m below the
  table's floor is clamped back to it. Placement is now fully deterministic:
  the table settles on whatever surface is at the coords, the chairs follow
  the table's floor, tiles and players follow the table.

## [1.0.0] - 2026-08-26

First release. All findings of the 2026-08-23 code audit
(`AUDIT-2026-08-23.md`) were fixed before shipping — the audit file records
what was found; the fixes are folded into the sections below.

NUI hardening: callback URLs hardcoded to `https://Silo-Dominoes/` (no
`GetParentResourceName`); the release package ships `html/app.js` obfuscated
in place.

Voice: push-to-talk stays usable during play — game UI focus keeps input
alive and blocks every control except `INPUT_PUSH_TO_TALK`.

### Money is never lost

- **Owed-money ledger**: any stake or pot that cannot be paid on the spot —
  admin cancel with a player offline, winner offline at the payout instant,
  expired abandoned game — is recorded on the game row (`status = 'owed'`)
  and **paid automatically the next time that character is seen online**.
- **Abandoned games expire** after `AbandonExpireHours` (default 24 h, 0 =
  never): both stakes refunded (offline seats via the ledger), table freed.
- The round result is persisted with the game, so a server restart inside
  the round summary can no longer discard a decided match.
- A routine save can no longer overwrite a settled row (status guard +
  `pot = 0` written on finish) — closes a double-payout window.

### Gameplay

- Two-player dominoes on **physical 3D tables** — walk up to a chair, ++g++
  to sit (native `MINIGAME_DOMINOES_PLAYER` scenario), the closed set rests
  on the table until an opponent sits down.
- **You play on the table, not in a menu**: playable tiles lift on your turn,
  click one, translucent previews show where it fits on either open end,
  click the end to play. Click the boneyard to draw. Right-click cancels.
- Tiles **slide with a hop** to their place (`TileMoveMs`, `TileArcZ`); your
  hand lies face up, the opponent sees only backs.
- Three fixed cameras cycled with ++v++: top-down, first person, cinematic
  side shot (`CameraHeight`, `CameraPitch`, `CameraFov`, `CameraBlendMs`).
- Spectators within `SpectateRange` see the live line and both players'
  tile backs.
- Six native domino skins (`TileSet` 1–6); tile geometry is read from the
  model's bounding box so a skin change keeps the layout right.
- Every sound is a native RDR2 soundset entry, configurable per event;
  `/domsound` and `/domsoundf` preview any sound in game.
- Map blips per table; furniture (table + 2 chairs) spawns within
  `FurnitureRange` or can be disabled for owner-placed furniture.
- **Floor height is self-detected** per table (from the ground-snapped table
  prop, with a ground probe as fallback): the `z` in `Config.Tables` is only
  a hint, so coords pasted while standing at the spot — ~1 m above the floor
  — no longer leave players, tiles and cameras floating.

### Rules

- **All Fives** (`Variant = "fives"`): points whenever the open ends sum to a
  multiple of 5 (a double at an end counts twice); round winner scores the
  loser's pips rounded to the nearest 5.
- **Draw** and **Block** variants; `TargetScore` for the match (0 = one round).
- 7 tiles each, the rest is the boneyard; the highest double (else heaviest
  tile) opens; automatic pass when stuck in Block mode; blocked rounds go to
  the lighter hand.

### Money

- Either player proposes a stake (`MinBet`–`MaxBet`), the other accepts or
  declines; on accept **both stakes are withdrawn into escrow at once**.
- The pot is paid to the match winner automatically. Settlement is guarded
  so two callers racing across a DB yield cannot both pay out.

### Crash-safe

- Every move is saved to `silo_dominoes_games`. A disconnect marks the match
  **interrupted**; when both players sit back down at the same table the
  round resumes from the exact position — hands, line, boneyard, scores.
- After `AbandonClaimMinutes` the remaining player can **claim the pot**.
- Server restart restores every unfinished game from the database.
- Leaving on purpose (stand up, or walk past `ForfeitDistance` and not
  return within `ForfeitGraceSeconds`) forfeits the match to the opponent.

### Security

- Deal, placement, draws, scoring and money happen **on the server only**;
  every move is validated against the shared rules engine.
- A client only ever receives **its own hand** — the opponent's tiles are
  never sent.
- Every net event is rate-limited per player.

### Configuration & integration

- **VORP Core / RSG Core auto-detected** through `silo-libs`; `oxmysql`
  required, table auto-created at start.
- **10 languages** — `en ro it de fr es pt ru pl tr` in `locales/locales.lua`,
  English fallback for missing keys.
- `NotifyFunction` at the bottom of `config.lua`.
- `escrow_ignore` keeps `config.lua`, `locales/locales.lua`, `sql/*.sql`,
  `README.md` and `html/*` readable.

### Admin & test mode

- `/domcancel <tableId>` (console or ace `silo-dom.admin`) — cancels a match
  and refunds the stakes (offline players via the owed-money ledger).
- `SinglePlayerBot = true` — an AI opponent with a visible `BotPed` takes the
  second seat; no betting, no DB. `Dev = true` enables debug prints and
  `/domcalib`. **Both must be `false` on a live server.**
