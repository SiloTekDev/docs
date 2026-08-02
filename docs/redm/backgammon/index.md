# Silo-Backgammon

Two-player backgammon played on **physical 3D tables in the world**, with
betting (escrow), a live board visible to everyone nearby and **automatic
game persistence** — if anyone crashes, the match resumes exactly where it
stopped once both players sit back down.

## Highlights

- **Real 3D play** — custom streamed board, checkers and native dice props on
  a real table. You click directly on the 3D board: movable checkers lift up,
  ghost checkers show every legal destination.
- **Betting with escrow** — the stake is withdrawn from both players the
  moment it is accepted. Nobody can run away with the pot.
- **Fully server-authoritative** — dice are rolled on the server, every move
  is validated against a complete rules engine, all money moves server-side.
- **Crash & disconnect proof** — interrupted matches persist to the database
  and resume from the exact position, dice included. Server restarts restore
  all running games.
- **Spectators** — everyone near the table sees the live board, checkers and
  dice animations.
- **Three cameras** (++v++) — top-down, first-person, cinematic side shot.
- **10 languages** — en, de, fr, it, es, pt, ro, ru, pl, tr; all texts in one
  open file.
- **Native RDR2 sounds** — every game sound is a configurable native
  soundset; preview them in game with `/bgsound`.
- **Test mode** — a `SinglePlayerBot` with a visible NPC opponent lets you
  test alone before going live.

## How a match plays

1. Walk up to a free chair → prompt **Play Backgammon** (++g++).
2. Sit down; the board appears while you wait for an opponent.
3. Second player sits → **betting phase**: either player proposes a stake,
   the other accepts or declines. On accept both stakes go into escrow.
4. **Opening roll** decides who starts; the camera switches to a top-down
   view and you play directly on the 3D board.
5. **Winning** pays the whole pot automatically. A "Gammon!" win is announced
   when the loser has not borne off a single checker.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Configuration :material-arrow-right:](configuration.md){ .md-button }
