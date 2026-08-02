# Silo-ForestRanger

A full forest ranger job for RedM. No fixed routes, no repeated deliveries:
you take a shift at a ranger station and the forest starts calling you —
wildfires, sick animals, poachers, lost travellers, fallen trees and wolf
packs, generated at random inside the patrol zones you are actually walking
through. Rangers gain XP, climb ranks and can work the same incident together.

## Highlights

- **Six incident types** — forest fire, sick animal, poachers, lost traveller,
  fallen tree, aggressive wolves. Each with its own gameplay, pay, XP and
  time limit.
- **Nothing is scripted twice** — incidents spawn at random spots inside your
  patrol zones, around the rangers who are actually out there.
- **Vanilla RDR2 mechanics** — Eagle Eye tracking on the sick animal hunt, your
  own lasso and hogtie for arresting poachers, the game's carry and
  stow-on-horse prompts.
- **Everyone helps, everyone gets paid** — every ranger on duty near a solved
  incident is rewarded, either the full amount each or a split.
- **XP, levels and ranks** — Trainee to Head Ranger, with a configurable
  per-level bonus on both money and XP.
- **Station panel** — level, XP, statistics, active calls and a paginated
  **Top Rangers** leaderboard.
- **Management sidebar** — rangers at `BossGrade` or higher can hire, promote
  and fire from the station panel.
- **Compass & optional GPS** — a compass points at the tracked incident with
  distance, remaining time and progress; the GPS route is opt-in per player.
- **Uniform** — configurable clothing put on at shift start, the ranger's own
  clothes come back at shift end.
- **10 languages** — en, ro, it, de, fr, es, pt, ru, pl, tr.
- **Discord webhooks** — shifts, solved incidents, failed incidents.

## A shift in one minute

1. Walk up to a ranger station → prompt **Ranger station** (++g++) opens the
   Forest Ranger panel.
2. **Start shift.** The server generates incidents around you while you
   patrol, every `MinEventMinutes`–`MaxEventMinutes`.
3. An incident is announced to every ranger on duty within `AlertRadius`, gets
   a map blip and shows up on the compass.
4. Ride out and handle it — every field action is one key held down (++e++ by
   default).
5. Get paid on the spot or at the end of the shift, back at a station.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Incidents :material-arrow-right:](incidents.md){ .md-button }
