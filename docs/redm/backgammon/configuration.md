# Configuration

Everything lives in `config.lua` — an open file with plain comments, one
option per line. The highlights:

## Tables

Add as many as you want. `blip` is a map blip hash, or `false` to hide the
table from the map.

```lua
Tables = {
    { coords = vector3(-173.92, 626.60, 113.03), heading = 145.25,
      label = "Backgammon Valentine", blip = -1081805875 },
}
```

## Betting

| Option | Default | Meaning |
|--------|---------|---------|
| `MinBet` | `1` | minimum stake in $ |
| `MaxBet` | `500` | maximum stake in $ |
| `GammonMultiplier` | `true` | announce a "Gammon!" win (announcement only — the pot stays the same) |

## Crash recovery

| Option | Default | Meaning |
|--------|---------|---------|
| `AbandonClaimMinutes` | `30` | minutes a disconnected player has to return before the opponent can claim the whole pot |
| `ForfeitDistance` | `5.0` | walking farther than this (meters) from the table triggers a warning |
| `ForfeitGraceSeconds` | `30` | seconds to come back after the warning, then the game is lost |

## Language

`Locale = "en"` — 10 languages included (`en de fr it es pt ro ru pl tr`).
All texts live in one open file, `locales/locales.lua`. Add your own language
by copying the `en` block.

## Sounds

Every game sound is a native RDR2 soundset entry `{ soundset, sound }` you
can change — dice roll, checker place, bet locked, turn start, win, lose.
Set any entry to `false` to mute it, or `Sounds = false` to mute everything.

Preview sounds in game:

```
/bgsound <soundset> <sound>    (3D, plays at your position)
/bgsoundf <soundset> <sound>   (frontend, 2D)
```

## Camera

`CameraHeight`, `CameraPitch` (−90 = perfectly vertical), `CameraFov`,
`CameraBlendMs`. Press ++v++ in game to cycle top-down → first-person →
cinematic side shot.

## Furniture & fine-tuning

The script can spawn its own table and chairs (`SpawnFurniture = true`) or
use furniture you placed on the map. Fine-tune the sitting position with
`SeatOffsetX` / `SeatOffsetZ`, and visuals with checker lift heights, ghost
transparency and animation timings.

## Test mode

| Option | Default | Meaning |
|--------|---------|---------|
| `SinglePlayerBot` | `false` | `true` = an AI takes the second seat instantly — no betting, no database. **Set `false` on a live server!** |
| `BotPed` | `"u_m_m_valpokerplayer_01"` | visible NPC opponent model (`false` = empty seat) |
| `BotThinkMs` | `2000` | AI thinking time (ms) |

## Notifications

At the bottom of the config:

```lua
function NotifyFunction(source, text)
    -- auto-detects VORP / RSG; replace with your own system if needed
end
```
