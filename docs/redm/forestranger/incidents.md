# Incidents

Every field action is **one key held down** (++e++ by default,
`PromptAction`). Each incident type has its own pay, XP, time limit and
weight — how often it is picked — in `Config.Events`.

## Forest fire

A water wagon is parked by the blaze: fill your bucket there, carry it to a
burning spot, throw the water — repeat until every spot is out (5 by default).
Set `waterWagon = false` for plain extinguishing with a tool item instead.

| Option | Default | Meaning |
|--------|---------|---------|
| `spots` | `5` | fire spots that must be put out (one bucket each) |
| `spotRadius` | `9.0` | how spread out (m) the spots are |
| `spotFires` | `3` | real flames burning in each spot — more = wider blaze |
| `spotFx` | `ent_amb_generic_fire_area` | looped particle fire burning constantly until the bucket (`""` = off) |
| `actionSeconds` | `5` | seconds of pouring water on one spot |
| `wagon` | `cart05` | water wagon model |

## Sick animal

A hunt, not a blip. The map only marks a **search area** — the animal is
somewhere inside it. Pick up its trail with **Eagle Eye** (real tracks and
scent, like a story-mode hunt), follow the blood drops it left behind, find it
and bandage it. It recovers and runs off.

| Option | Default | Meaning |
|--------|---------|---------|
| `tracking.enabled` | `true` | `false` = classic "blip on the animal" behaviour |
| `tracking.searchRadius` | `60.0` | radius (m) of the search circle on the map |
| `tracking.trailLength` | `60.0` | how far the animal limps before collapsing — its real tracks become the Eagle Eye trail |
| `tracking.bloodTrail` | `true` | blood drops along the trail, visible without Eagle Eye |
| `models` | deer, elk, buck | animals that can be sick |
| `scenarios` | per model | how the hurt animal lies there (`WORLD_ANIMAL_DEER_INJURED_ON_GROUND` and friends) |

## Poachers

Order them to surrender (aim at them and they nearly always do), then lasso
and hogtie one with the game's own controls, throw him over your horse and
ride him to a station — or take the fight.

Poachers who give up are **disarmed on the spot** (weapon dropped and removed)
and keep their hands up. The arrest itself is pure vanilla RDR2 — your lasso,
your hogtie, the game's own carry and stow-on-horse prompts. The script only
notices who tied whom and adds one prompt: handing the poacher over at the
station. Hogtieing one mid-firefight counts as well.

| Option | Default | Meaning |
|--------|---------|---------|
| `minPoachers` / `maxPoachers` | `2` / `4` | how many spawn |
| `surrenderChance` | `45` | % chance they give up when you just shout |
| `aimSurrenderChance` | `85` | % chance with a weapon aimed at them |
| `nonLethalBonus` | `1.5` | reward multiplier when arrested instead of killed |
| `hailDistance` | `30.0` | from how far (m) you can order a surrender |
| `deliverDistance` | `60.0` | how close (m) to a station the hogtied poacher must be brought |

## Lost traveller

Talk to them, they follow you, hand them over at a station. On the way they
talk to you — long campfire-style `CHAT_STORY_*` lines, one every
`chatSeconds` (0 = silent). You tip your hat when you pick them up
(`EmoteGreet`) and they thank you with a gesture (`EmoteThanks`).

## Fallen tree

Chop the trunk blocking the trail. `actionSeconds` of chopping, with the
game's own axe animation. The trunk prop is configurable (`prop`).

## Aggressive wolves

Put the pack down, or fire a warning shot to scatter it — scaring them off
pays better (`nonLethalBonus`). A weapon must be drawn, and you must be within
`scareDistance`.

---

## Payout and participation

Every ranger on duty within `ParticipationRange` when the incident is solved
is rewarded. `SharedPayoutMode` decides how:

| Value | Effect |
|-------|--------|
| `"each"` | every ranger who helped gets the full reward |
| `"split"` | the reward is divided between them |

`PayoutMode` decides *when*:

| Value | Effect |
|-------|--------|
| `"instant"` | paid the moment an incident is solved |
| `"end_of_shift"` | paid when you end your shift at a station |

Unpaid money survives disconnects: it waits in the database until the next
time you end a shift.

If nobody handles an incident before its timer runs out it expires — the
rangers who were on the scene take a "missed" on their record
(`FailedEventXp` decides whether they still get XP).

## Multiplayer scene handling

Fires, fallen trees and camp props are **local** on every client but placed at
server-generated coordinates, so all rangers see the same scene with zero
networked objects. Peds (poachers, wolves, animals, travellers) are
**networked** and spawned by a single client — the "host" the server picks —
so everyone fights the same wolves. If the host disconnects or rides away, the
server hands the incident to another ranger nearby and the scene is rebuilt.
