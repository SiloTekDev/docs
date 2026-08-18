# Changelog

Version currently shipping: **1.0.0** — paired with
[`silo-libs`](../silo-libs/index.md). No database, no streamed props.

## v1.0.0 — 2026-08-13

First release. Everything below is in the shipped build.

### Gameplay

- Street dice played **on the ground** — no table, no lobby. Using the dice
  item opens a stake card; the player crouches and waits.
- The waiting host can **crouch-walk** and the game follows; standing up
  cancels the wait and returns the stake.
- A passer-by within `JoinDistance` sees a prompt with **the pot written on
  it** — nobody accepts a bet blind.
- Both throw with ++g++. Dice land in front of each thrower showing the faces
  the server rolled. `NetworkDice` makes them real networked props everyone
  nearby can watch.
- 1 to 6 dice per player, result held for `ResultSeconds`.
  `ForceFirstPerson` locks the camera once the duel is on.

### Money

- The stake is **escrowed the moment a game is created**, and again when a
  joiner matches it. Once both stakes are down there is no backing out.
- Highest total takes the pot, or lowest with `Rule = "lower_total"`. A draw
  returns each player their own stake.
- Every ending — win, draw, cancel, disconnect, restart — settles the pot
  **exactly once**, guarded against double payouts.

### Security

- Every die is **rolled on the server**.
- Starting a game requires having **used the dice item** — the stake dialog
  answers through a short-lived server-side window, so a bare net event
  cannot conjure a game or dodge the item.
- Join distance, waiting spot and dice origins are read from **where the
  server sees the players**, never from client-sent coordinates.
- The stake is validated server-side and every net event is rate-limited.

### Configuration

- **VORP Core / RSG Core auto-detected**.
- `DiceItem` (default `zaruri`) must exist in the framework item database and
  be **usable**; `ConsumeItem` decides whether it is used up.
- **6 languages**: `en ro it de fr es`. A key missing from a translation
  falls back to English.
- `EvaluateResult(hostRoll, guestRoll)` decides the winner. Rewrite that one
  function for a local variant and the pot follows it.
- `NotifyFunction` hook for your own notification system.

### Test mode

- `Dev = true` — debug prints. Set `false` on a live server.
