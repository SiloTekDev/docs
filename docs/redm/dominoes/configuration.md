# Configuration

Everything a server owner is meant to touch lives in `config.lua`, and every
piece of text lives in `locales/locales.lua`. Nothing else needs editing.

## Rules

| Setting | Default | What it does |
|---|---|---|
| `Variant` | `"fives"` | `fives` = All Fives (score when the open ends add up to a multiple of 5) · `draw` = draw from the boneyard when stuck · `block` = no drawing, pass when stuck |
| `TargetScore` | `100` | Points to win the match; `0` = a single round decides |
| `TileSet` | `1` | Domino skin 1–6 (native RDR2 sets: bone, wood, dark wood…) |

### The three variants

| Variant | How it plays |
|---------|--------------|
| `fives` | **All Fives** — every time the two open ends add up to a multiple of 5 you score that many points on the spot. The round winner also scores the loser's remaining pips, rounded to 5. |
| `draw` | **Draw Dominoes** — stuck players draw from the boneyard until they can play; the round winner scores the loser's remaining pips. |
| `block` | **Block Dominoes** — no drawing; stuck players pass. When both pass, the lighter hand wins the difference. |

## Betting & crash recovery

| Setting | Default | What it does |
|---|---|---|
| `MinBet` / `MaxBet` | `1` / `500` | Stake range in $ |
| `AbandonClaimMinutes` | `30` | How long a disconnected player has to return before the opponent can claim the pot (the player who left waits **twice** as long to claim) |
| `AbandonExpireHours` | `24` | If **nobody** returns, the game expires: both stakes refunded — offline players are paid automatically at their next login. `0` = never |
| `ForfeitDistance` | `5.0` | Walking farther than this (m) triggers a warning |
| `ForfeitGraceSeconds` | `30` | Time to come back after the warning, then the game is lost |

## Tables, furniture & seating

Add as many tables as you want under `Tables` (coords + heading + label +
map blip). The floor height is detected from the table itself — the config
`z` only needs to be roughly right.

| Setting | Default | What it does |
|---|---|---|
| `TableProp` / `ChairProp` | `s_tablepierson01x` / `p_chair_barrel04b` | The furniture models |
| `SpawnFurniture` | `true` | `false` if you placed your own furniture on the map |
| `FurnitureRange` | `60.0` | Furniture exists only within this range of a player |
| `TableRotation` | `90.0` | Extra rotation of the table prop only |
| `TableSurfaceZ` | `0.799` | Table top height above the table prop origin |
| `ChairDistance` / `SeatDistance` | `0.7` / `0.7` | Chair / seated-ped distance from the table center |
| `SeatOffsetX` / `SeatOffsetZ` | `0.0` / `0.50` | Fine-tune of the sitting position |
| `SitScenario` | `MINIGAME_DOMINOES_PLAYER` | The seated animation scenario |

## Cameras, visuals, sounds

| Setting | Default | What it does |
|---|---|---|
| `CameraHeight` / `CameraPitch` / `CameraFov` | `1.25` / `-90` / `36` | The top-down camera |
| `CameraBlendMs` | `650` | Camera transition time |
| `SourceLift` / `SelectLift` | `0.012` / `0.026` | How far playable / selected tiles lift |
| `GhostAlpha` | `120` | Transparency of the placement previews |
| `TileMoveMs` / `TileArcZ` | `550` / `0.06` | The tile slide animation |
| `RoundEndSeconds` | `8` | How long the round summary stays on screen |
| `SpectateRange` | `12.0` | Players within this range see the live table |
| `Sounds` + per-event entries | native soundsets | Every game sound is a native RDR2 soundset entry; preview in game with `/domsound <soundset> <sound>` (3D) or `/domsoundf` (frontend). Any entry set to `false` is muted |

## Languages

`Locale` picks one of the **10 shipped languages** — `en ro it de fr es pt ru
pl tr` — all in one open file, `locales/locales.lua`. A key missing from a
translation falls back to English. Add your own language by copying the `en`
block.

## Notifications

`NotifyFunction` at the bottom of `config.lua` routes every message; VORP and
RSG are auto-detected by default — replace the function body to plug in your
own notification system.

## Admin & test mode

- `/domcancel <tableId>` — console or ace `silo-dom.admin`. Cancels the match
  at a table and refunds the stakes: online players immediately, offline
  players automatically at their next login.
- `SinglePlayerBot = true` — an AI opponent (visible NPC, `BotPed`) takes the
  second seat instantly; no betting, no database. **`false` on a live
  server**, same as `Dev`.

## Known limitations

- The line has two open ends (no spinner / four-way play).
- Two players per table.
- The opening player (highest double, else heaviest tile) may lead any tile —
  the announced double decides *who* opens, not what they must play.
