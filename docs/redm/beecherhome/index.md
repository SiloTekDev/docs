# Silo-BeecherHome

Rebuild the finished Beecher's Hope ranch — John's epilogue house — **anywhere
on the map**. A builder tool for server owners, not a player-facing job.

The package is two resources: `Silo-BeecherHome-Props` streams the original
`bee_*` models globally, and `Silo-BeecherHome` places them.

## How it works

The ranch layout ships pre-baked in `placements.json` (~400 entities: house,
barn, chicken coop, chimney, woodpile, room walls, crafting fire, plus the full
house and barn **interiors**), extracted directly from the original ymap/ytyp
binaries. `/beebuild` rebuilds everything at any position, rotated to your
heading — no in-game capture needed.

Placed ranches are saved in `houses.json`, survive restarts, and are spawned
client-side (local, non-networked, frozen props) for every player within
`Config.SpawnRange`.

The vanilla archetypes are only registered near the original map tile, which is
why the props resource re-registers them globally through a custom YTYP.

## Setup

!!! warning "One manual step before first start"
    The YTYP must be converted from XML to binary in **CodeX Explorer**:
    open CodeX → import `source/beecherhome_props.ytyp.rsc.xml` → export the
    binary as `beecherhome_props.ytyp` → put it in `stream/` next to the
    models. See `Silo-BeecherHome-Props/README.md`.

1. server.cfg:

    ```
    ensure Silo-BeecherHome-Props
    ensure Silo-BeecherHome
    add_ace group.admin silobeecherhome.admin allow
    ```

2. Go anywhere, face the direction the house should face, run **`/beebuild`**.
3. **`/beeremove`** removes the nearest placed ranch.
4. Check the F8 console for
   `built: X/400 pieces (Y unavailable models skipped)`.

## Commands

| Command | What it does |
|---------|--------------|
| `/beebuild` | Place a ranch copy at your position, rotated to your heading |
| `/beeremove` | Remove the nearest placed ranch (300 m) |
| `/beedump` | Fallback: re-capture the original layout from the live map, at Beecher's Hope. **Overwrites `placements.json`** |

## Configuration

`config.lua` is deliberately tiny:

| Option | Default | Meaning |
|--------|---------|---------|
| `BuildCommand` | `beebuild` | command that places a copy of the ranch |
| `RemoveCommand` | `beeremove` | command that removes the nearest ranch |
| `AdminOnly` | `true` | commands require the ace permission `silobeecherhome.admin` |
| `SpawnRange` | `600.0` | players closer than this to a placed ranch will see it (m) |

`NotifyFunction` at the bottom auto-detects VORP / RSG — replace it with your
own in one line.

## Known limitations

- **No collision** — map pieces have no embedded collision, so the ranch is
  visual-only for now. The original `.ybn` collision is world-locked at
  Beecher's Hope; rebasing it is planned.
- **Missing structures**: silo, gazebo, outhouse, both road entrances, mailbox
  and the ranch sign are streamed and ready, but their *placements* live in
  ymaps still inside `bee_01_.rpf` (AES-encrypted TOC). Export those ymaps with
  CodeX and the layout can be extended.
- ~275 entities are native props referenced by hash; any that are area-tied
  archetypes will be skipped and counted in the F8 console.
- Interior entities are the final-stage named set. MLO room lighting and
  portals do not replicate, so interiors use ambient world light.
- Near the original Beecher's Hope the archetypes are registered twice (vanilla
  + ours). Content is identical, but test once in that area.
