# Developer API & limitations

## Security

- The server decides **which** incident spawns, **where** and **when**; the
  client is only asked for a valid piece of ground inside the zone, and the
  answer is re-validated (inside the zone, inside the distance band, not on
  top of another incident).
- Every field action is validated server-side: on duty, close enough, correct
  incident, correct state, required tool in the satchel, rate-limited.
- All money and XP movements happen exclusively on the server — rewards are
  computed from the config, never sent by the client.
- Poacher surrender rolls happen on the server, not in the client.

## Exports

**Server**

```lua
exports["Silo-ForestRanger"]:IsOnDuty(source)        -- boolean
exports["Silo-ForestRanger"]:GetOnDutyRangers()      -- { serverId, ... }
exports["Silo-ForestRanger"]:GetActiveEvents()       -- { {id, type, zone, coords, remainingMs}, ... }
exports["Silo-ForestRanger"]:GetRangerStats(source)  -- { xp, level, solved, failed, earned } or nil
```

**Client**

```lua
exports["Silo-ForestRanger"]:IsOnDuty()              -- boolean
exports["Silo-ForestRanger"]:GetActiveEvents()       -- { {id, type, zone, coords}, ... }
```

For dispatch and logging scripts there is also the
`EventStartedFunction(type, coords, zone)` hook at the bottom of
`config.lua`, called on the server whenever a new incident spawns.

## Known limitations

- Wildfires use RDR2 script fires: they are visual and dangerous up close, but
  they do not permanently scar the map.
- The RSG inventory adapter in `silo-libs` is still a stub, so the optional
  `RequiredItems` check only works fully on VORP today. Leave the items empty
  on RSG and everything else works normally.
- Incidents spawn around rangers who are **inside** a patrol zone; a ranger
  standing in a town will not generate anything.
