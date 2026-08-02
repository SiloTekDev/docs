# Configuration

Everything a server owner is meant to touch lives in `config.lua`, and every
piece of text lives in `locales/locales.lua`. Nothing else needs editing.

## Settings

| Setting | Default | What it does |
|---|---|---|
| `Locale` | `"en"` | Language of every message: `en` `ro` `it` `de` `fr` `es` |
| `Dev` | `false` | debug prints in console (testing only) |
| `DiceItem` | `"zaruri"` | The inventory item that starts a game |
| `ConsumeItem` | `false` | Whether that item is used up each time |
| `DiceCount` | `2` | Dice each player throws (1–6) |
| `WaitSeconds` | `120` | How long someone kneels waiting before giving up |
| `ResultSeconds` | `7` | How long the dice and the score stay up afterwards |
| `JoinDistance` | `2.5` | How close a passer-by must be to join |
| `FaceDistance` | `1.35` | How far apart the two players kneel |
| `WalkAwayDistance` | `2.2` | Wandering farther than this from your spot forfeits |
| `DiscoverRange` | `25.0` | Players are only told about games this close to them |
| `Scenario` | `WORLD_PLAYER_DYNAMIC_KNEEL` | The animation both players hold |
| `ScenarioGraceSeconds` | `4` | Grace given for that animation to start before it is checked |
| `CancelOnScenarioBreak` | `true` | Whether standing up forfeits the duel |
| `ForceFirstPerson` | `false` | Locks the camera to first person during the duel |
| `KeyJoin` / `KeyRoll` / `KeyCancel` | ++g++ / ++g++ / ++backspace++ | The three keys |
| `DiceProp` / `DiceSpacing` / `DiceZOffset` | `p_dice01x` / `0.16` / `0.03` | The dice on the ground |
| `DiceFaces` | per face | Which rotation shows each face upward |

!!! note "DiceFaces"
    Only touch these if you swap `DiceProp` for a model whose faces sit
    differently.

## House rules

The winner is decided by one function at the bottom of `config.lua`:

```lua
function EvaluateResult(hostRoll, guestRoll)
```

Both throws arrive as a plain list of numbers. Return the winner — `"host"`,
`"guest"` or `"draw"` — and the two scores to display. The default is highest
total.

If your server plays a local variant where doubles beat everything, or the
lowest throw wins, or a certain pair is an instant win, rewrite that one
function and nothing else has to change.

## Languages

Six languages ship complete: English, Romanian, Italian, German, French and
Spanish. Every message the player can see is in `locales/locales.lua`, so a new
language is a matter of copying one block and translating it — no code is
involved. A key missing from a translation falls back to English rather than
showing a blank.

## Notifications

Two hooks at the bottom of `config.lua` — the server one takes a player id, the
client one only ever talks to you:

```lua
function NotifyFunction(source, text)        -- server
function NotifyClientFunction(text)          -- client
```

Both auto-detect VORP / RSG; replace them with your own system in one line.

## Security

- Every die is **rolled on the server**; clients just place props with the
  faces they are told.
- Starting a game happens **inside the usable-item callback** — there is no
  open "start a game" net event a modified client could fire without ever
  owning the item.
- Join distance and roll distance are measured against **where the server sees
  the player**, never against coordinates a client sends.
- Cancel reasons coming from a client are checked against a whitelist, so a
  modified client cannot put arbitrary text on the other player's screen.
- Every net event is rate-limited per player.
