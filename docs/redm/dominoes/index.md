# Silo-Dominoes

Two-player dominoes played on physical 3D tables in the world, with betting,
a live table visible to everyone nearby and automatic game persistence. Every
tile, the table, the seated animations and the sounds are **native RDR2
assets** — nothing to stream, nothing extra to install.

## Highlights

- **You play on the table, not in a menu** — your playable tiles lift up on
  your turn; click one, translucent previews show where it fits, click the
  end to play it. The boneyard is a real pile you click to draw from.
- **Three rulesets** — All Fives (the RDR2 saloon game), Draw and Block
  dominoes; one config line to switch.
- **Stakes in escrow** — either player proposes, the other accepts, and both
  stakes are withdrawn at once. The winner is paid automatically.
- **Crash-safe** — the match is saved after every move; disconnects resume,
  restarts restore, abandoned pots can be claimed, and money owed to offline
  players is paid automatically at their next login.
- **Nobody can peek** — a client only ever receives its **own** hand; the
  deal, every placement and every point happen on the server.
- **Six native tile skins**, three cameras on ++v++, spectator view for
  everyone within range, **10 languages**.

## How a match plays

1. Walk up to a free chair → prompt **Play Dominoes** (++g++). You sit down
   with the native dominoes animation; the closed set rests on the table.
2. A second player sits → **betting**: one proposes a stake, the other
   accepts or declines. On accept the money leaves both pockets — escrow.
3. Tiles are shuffled and dealt server-side — 7 each, the rest is the
   boneyard. The highest double opens.
4. The camera locks over the table (++v++ cycles top-down, first person,
   cinematic). Click a raised tile, then the end where it fits;
   right-click cancels. No playable tile? Click the boneyard to draw, or
   the turn passes by itself in Block mode.
5. Rounds are scored until `TargetScore`; the pot goes to the match winner.

| What you want | How |
|---|---|
| Sit down / start | ++g++ at a free chair |
| Play a tile | click the tile, then the end |
| Draw from the boneyard | click the boneyard pile |
| Cancel a selection | right-click |
| Switch camera | ++v++ |
| Leave the table | ++backspace++ (mid-game = forfeit) |

## When things go wrong

- **Disconnect** — the match becomes *interrupted* and the table stays
  reserved. When both players sit back down it resumes from the exact saved
  position: hands, line, boneyard, scores.
- **Opponent never returns** — after `AbandonClaimMinutes` the remaining
  player claims the whole pot at the table. Whoever walked out themselves
  waits twice as long.
- **Nobody returns** — the game expires after `AbandonExpireHours`; both
  stakes are refunded. Offline players get the money **automatically at
  their next login**.
- **Server restart** — every running game is restored from the database.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Configuration :material-arrow-right:](configuration.md){ .md-button }
