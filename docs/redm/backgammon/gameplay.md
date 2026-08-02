# Gameplay & Admin

## Crash / disconnect handling

- If a player disconnects mid-game, the match becomes **interrupted** and is
  saved to the database. The table stays reserved for the two players.
- When both return and sit at the same table, the match **resumes** from the
  exact saved position — including the remaining dice.
- If the missing player does not return within `AbandonClaimMinutes`
  (default 30), the opponent can **claim the whole pot** at the table.
- Server restarts restore all running games from the database.
- Leaving the table **on purpose** during a game (standing up or walking away
  past `ForfeitDistance`) forfeits the match — the opponent wins the pot.

## Security

- Dice are rolled **on the server only**; clients just animate the result.
- Every move is validated server-side against a complete backgammon rules
  engine — a modified client cannot make illegal moves.
- All money movements happen exclusively on the server.
- Server-side rate limiting on every player-triggered event.

## Admin

| Command | Where | Effect |
|---------|-------|--------|
| `/bgcancel <tableId>` | server console, or in game with ace `silo-bg.admin` | force-cancels the match at a table and refunds the stake to online players |

## Known limitations

- The strict "must play both dice if possible" rule is simplified: every
  single move is validated, but the engine does not search ahead for the
  sequence that maximizes dice usage.
- The doubling cube is not implemented.
