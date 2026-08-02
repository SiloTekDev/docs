# Configuration

Everything a server owner is meant to touch lives in `config.lua`, and every
piece of text lives in `locales/locales.lua`. Nothing else needs editing.

## Tables & seats

| Option | Default | Meaning |
|--------|---------|---------|
| `Tables` | 3 (Valentine, Rhodes, Saint Denis) | where the tables stand: coords, heading, label and map blip (`false` hides it from the map) |
| `MinPlayers` / `MaxPlayers` | `2` / `6` | seats around the table |
| `LobbySeconds` | `45` | seconds the host waits before the start button appears |
| `SitPromptDistance` | `1.6` | how close (m) to a chair the sit prompt appears |
| `SpectateRange` | `14.0` | players within this range (m) see the board and pieces |
| `HideHudAtTable` | `true` | hides the minimap and HUD while you are seated |

## Money

| Option | Default | Meaning |
|--------|---------|---------|
| `StartingCash` | `1500` | play money every player begins with (in-game currency, not real $) |
| `GoSalary` | `200` | paid every time a player passes GO |
| `JailFine` | `50` | cost to buy your way out of jail |
| `BuyIn` | `0` | **real** money each player pays to join (`0` = free game) |
| `WinnerTakesPot` | `true` | the winner collects every buy-in |

## Rules & turns

| Option | Default | Meaning |
|--------|---------|---------|
| `FreeParkingPot` | `false` | taxes and fines pile up on Free Parking for whoever lands there |
| `MaxJailTurns` | `3` | turns in jail before the fine is forced |
| `TurnSeconds` | `60` | seconds a player has to act before the turn is auto-played |
| `BankruptcyEndsGame` | `true` | the game ends when only one player is left standing |

## Breaks & crash recovery

| Option | Default | Meaning |
|--------|---------|---------|
| `AwayMinutes` | `8` | how long a seat is held for someone who pressed ++esc++ and took a break |
| `AbandonClaimMinutes` | `30` | how long a seat, money and properties are held for someone who crashed or disconnected |
| `ForfeitDistance` | `6.0` | wandering farther than this (m) triggers a warning. Players on a break are exempt |
| `ForfeitGraceSeconds` | `45` | seconds to come back after the warning, then the player is out |

## Visuals

| Option | Default | Meaning |
|--------|---------|---------|
| `TokenProps` | `p_thimble01x` ×6 | one piece per seat, in seat order — any prop model works |
| `HouseProp` / `HotelProp` | `mnp_house` / `mnp_hotel` | the buildings. The shipped models are sized to the printed colour strip; a different model may not sit on it |
| `ShowMoveGhost` / `GhostAlpha` | `true` / `110` | the see-through preview piece on the space you are about to move to |
| `CardSeconds` | `5` | how long a drawn card stays up — the table is frozen for exactly this long |
| `CardTilt` | `7.0` | how crooked the two card piles lie on the board |
| `ShowCardFlight` / `CardFlyMs` | `true` / `750` | the drawn card sliding off the board toward the player who drew it |
| `ShowDeedCards` | `true` | every player's properties lie as small cards in front of their seat |
| `DeedTilt` / `DeedPushOut` | `8.0` / `0.0` | how crooked those cards lie, and how far they slide away from the board. Raise `DeedPushOut` on a wide table so they sit within reach |
| `ShowRules` | `true` | shows the house rules once per session before your first game |

## Furniture

The script can spawn its own table and chairs (`SpawnFurniture = true`,
`TableProp` / `ChairProp`) or use furniture you placed on the map
(`SpawnFurniture = false`). `TableSurfaceZ` is the table top height above the
prop origin, `ChairDistance` / `SeatDistance` the distance from the table
centre, and `SeatOffsetX` / `SeatOffsetZ` fine-tune the sitting position.

## Sounds

Every game sound is a native RDR2 soundset entry `{ soundset, sound }` — dice
roll, piece landing, card drawn, property bought, rent paid, turn start, win,
knocked out. Set any entry to `false` to mute it, or `Sounds = false` to mute
everything.

Preview sounds in game:

```
/mpsound <soundset> <sound>    (3D, plays at your position)
/mpsoundf <soundset> <sound>   (frontend, 2D)
```

## Camera

`CameraHeight`, `CameraPitch` (−90 = perfectly vertical), `CameraFov`,
`CameraBlendMs`. Press ++v++ in game to cycle top down → first person → wide
shot.

## Test mode

| Option | Default | Meaning |
|--------|---------|---------|
| `SinglePlayerBot` | `false` | `true` = sit down alone and AI players fill the empty seats, the game starts instantly, no buy-in, no database. **Set `false` on a live server!** |
| `BotCount` | `2` | how many AI players join you (1–5) |
| `BotName` | `Drifter` | base name (`Drifter 1`, `Drifter 2`, …) |
| `BotPed` | `u_m_m_valpokerplayer_01` | ped model seated as an AI player (`false` = empty seat) |
| `BotThinkMs` | `1800` | AI thinking time before rolling / acting (ms) |

## Language

`Locale = "en"` — English ships complete. Every card, every button and every
message is in `locales/locales.lua`, so the whole game can be translated
without touching a line of code: copy the `en` block, translate it and point
`Locale` at your new key.

## Notifications

At the bottom of the config:

```lua
function NotifyFunction(source, text)
    -- auto-detects VORP / RSG; replace with your own system if needed
end
```
