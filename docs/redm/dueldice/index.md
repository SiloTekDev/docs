# Silo-DuelDice

A street dice duel for RedM. One player kneels down in the dirt, a passer-by
takes them on, both throw, highest total wins. No table, no menu, no interface —
just two people crouched in an alley and a pair of dice on the ground.

## Highlights

- **No UI at all** — the whole game is two kneeling animations, a prompt and
  real dice props on the ground.
- **Nothing to install** — no database, no SQL, no streamed props. The dice are
  a model the game already ships (`p_dice01x`).
- **One item starts it** — using the dice item kneels you down; a passer-by
  presses ++g++ to take you on.
- **Fully server-authoritative** — every die is rolled on the server; clients
  just place props with the faces they are told.
- **House rules in one function** — `EvaluateResult` at the bottom of
  `config.lua` decides who wins. Rewrite that one function and nothing else has
  to change.
- **6 languages** — English, Romanian, Italian, German, French and Spanish, all
  complete.

## How a duel plays

1. A player **uses the dice item** → their character kneels down and waits for
   someone to take them on.
2. A passer-by walks up → prompt **Join the game** (++g++). Players are only
   told about games close enough to actually reach — the server shares games
   near you and nothing else.
3. Once the two are kneeling face to face, both throw with ++g++. The dice land
   on the ground between them, showing the faces the server rolled, and the
   score is read out to both.
4. Highest total wins; equal totals are a draw. After a few seconds the dice
   disappear and both players stand back up.

| What you want | How |
|---|---|
| Start a game | Use the dice item |
| Join someone waiting | Walk up and press ++g++ |
| Throw your dice | ++g++ |
| Walk away | ++backspace++ |

A duel ends by itself if either player goes down, walks too far from their own
spot, stands up out of the animation, or leaves the server. Nobody is left
kneeling in a game that no longer exists — a player who is never taken on gives
up on their own after `WaitSeconds`.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Configuration :material-arrow-right:](configuration.md){ .md-button }
