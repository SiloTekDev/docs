# Configuration

Everything lives in `config.lua` — an open file with plain comments, no
internals. Every piece of text lives in `locales/locales.lua`.

## Job access

| Option | Default | Meaning |
|--------|---------|---------|
| `RequireJob` | `true` | `false` = anyone can work as a ranger |
| `JobNames` | `ranger`, `forestranger` | framework job names allowed on duty |
| `AllowSelfEmploy` | `false` | `true` = adds a **Become a Ranger** button at the station |
| `EmployJobName` / `EmployJobGrade` | `ranger` / `0` | job applied on self-employ or hire |
| `BossGrade` | `3` | rangers at this grade or higher get the **Management** sidebar |
| `FiredJob` | `unemployed` | job a fired ranger is moved to |

The management sidebar lets a boss hire any player by server ID, promote (up
to `BossGrade`) and fire the rangers currently online. It works with any
`RequireJob` setting.

## Uniform

Clothing items (shop `drawable` + `data`) put on automatically at shift start;
the ranger's own clothes come back at shift end through
`RestoreClothesFunction`. **Clothes only — no face parts.**

`RemoveCategories` lists every clothing category taken off *before* the
uniform goes on (boots and belts stay on purpose). Set `Enabled = false` for
no uniform at all.

## Stations, zones and timing

| Option | Default | Meaning |
|--------|---------|---------|
| `StationDistance` | `2.5` | how close (m) to a station the prompt appears |
| `PromptStation` | ++g++ | opens the station panel |
| `PromptAction` | ++e++ | held for every field action |
| `MinEventMinutes` / `MaxEventMinutes` | `5` / `15` | wait between two incidents on the server |
| `MaxActiveEvents` | `2` | how many can run at the same time |
| `EventMinDistance` / `EventMaxDistance` | `120.0` / `450.0` | how far (m) an incident spawns from the ranger it is generated for |
| `AlertRadius` | `1200.0` | rangers within this range get the alert and the blip |
| `ParticipationRange` | `80.0` | rangers within this range when it is solved get paid |

## Compass & GPS

| Option | Default | Meaning |
|--------|---------|---------|
| `Compass` | `true` | compass at the top of the screen with direction, distance and timer |
| `GpsRoute` | `false` | default state of the GPS route — each player toggles it themselves |
| `GpsRouteCommand` | `rangergps` | chat command that toggles the route |

**Track** in the station panel picks which incident the compass and the route
follow.

## Tools

Optional items per incident, each as `{ item = "medicine", consume = true }`:

- `consume = true` — one item is taken from the satchel on every use (doses).
- `consume = false` — the ranger only has to carry it (tools like an axe).
- `item = ""` — the check is disabled.

```lua
RequiredItems = {
    forest_fire = { item = "", consume = false },  -- only used when waterWagon = false
    sick_animal = { item = "", consume = true },   -- e.g. "medicine"
    fallen_tree = { item = "", consume = false },  -- e.g. "axe"
},
```

!!! warning "RSG"
    The RSG inventory adapter in `silo-libs` is still a stub, so
    `RequiredItems` only works fully on VORP today. Leave the items empty on
    RSG and everything else works normally.

## Leveling

```lua
XpPerLevel = 250,   -- XP needed to advance one level
MaxLevel = 20,      -- the bonus stops growing here (XP keeps counting)
LevelBonus = 0.03,  -- +3% money AND XP for every level above 1
Ranks = { ... },    -- rank title shown per level range
```

| Level | Default rank |
|-------|--------------|
| 1 | Trainee |
| 4 | Ranger |
| 9 | Senior Ranger |
| 14 | Warden |
| 18 | Head Ranger |

## Animations

`ScenarioFire` (putting out a fire), `AnimTreat` (the bandage animation for
treating an animal), `EmoteGreet` (your hat-tip salute when you pick up the
lost traveller) and `EmoteThanks` (his relieved gesture). Any
`KIT_EMOTE_GREET_*` from the game works; `""` turns one off.

## Language

`Locale = "en"` — 10 languages included (`en ro it de fr es pt ru pl tr`). All
texts live in one open file, `locales/locales.lua`. Add your own by copying
the `en` block.

## Webhooks

`Config.Webhooks` — three Discord events: `Shift` (a ranger starts or ends a
shift), `EventSolved` (with who helped and the payout), `EventFailed` (an
incident expired unsolved). Same URL on all three = one channel; different
URLs = separate channels; empty string = that event disabled.

## Config hooks

Six functions at the bottom of `config.lua`, each replaceable in one line:

| Hook | Runs on | Fires when |
|------|---------|------------|
| `NotifyFunction(source, text, type)` | server | any notification (VORP / RSG auto-detected) |
| `ClientNotifyFunction(text, type)` | client | field messages (action cancelled, GPS toggled…) |
| `ProgressFunction(label, seconds, action)` | client | an action needs a progress bar — plug in your own; block until done, return `true`/`false` |
| `OnDutyFunction()` | client | your shift starts — play a sound, extra gear… |
| `RestoreClothesFunction()` | client | puts the player's own clothes back after the uniform (default: VORP skin reload) |
| `OffDutyFunction()` | client | your shift ends |
| `EventStartedFunction(type, coords, zone)` | server | a new incident spawns — hook dispatch / log scripts |
