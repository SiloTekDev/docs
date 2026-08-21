# Changelog

Version currently shipping: **1.0.1** — paired with
`Silo-Mayorpoly-Props` **1.0.0** and [`silo-libs`](../silo-libs/index.md).

## v1.0.1 — 2026-08-21

**Fixed**

- The owned-properties panel could name the wrong spaces — a bought ranch
  listed as a railroad, or Chance / Chest / GO appearing as owned deeds —
  once the early board spaces were all bought. Board ownership now crosses
  to the interface as explicit-index lists, immune to the array conversion
  that shifted every deed by one space.
- Display-only bug: cash, rents and actual ownership on the server were
  always correct. No config or database changes are needed — replace the
  resource and restart.

**Changed**

- The fix is locked in by a new automated regression test that drives the
  real interface code with the real board data, so it cannot come back.

## v1.0.0 — 2026-08-14

First release. Everything below is in the shipped build.

### The table

- Two to six players on a real board, with real pieces, real dice and cards
  drawn off the table. Sixteen streamed models share one texture dictionary.
- The first player to sit is the **host** and starts the game once at least
  two are seated.
- **The board is the interface** — click to roll, click a space to move (with
  a see-through preview piece), click a pile to draw, click a name to inspect
  a player's holdings.
- A drawn card **slides off its pile toward whoever drew it** and is shown
  full screen to everyone at once. The table is frozen for `CardSeconds`.
- Every owned property lies as a **small card in front of its owner's seat**,
  fanned and crooked — who is close to a full colour group is visible at a
  glance.
- **Three cameras** on ++v++. One token prop per seat. `HideHudAtTable`,
  `ShowRules`.

### Rules

Standard ruleset: colour-group rent doubling, even building, four houses to a
hotel, a bank of 32 houses and 12 hotels that can run out, railroad and
utility scaling, mortgages that still count toward group size, doubles and
jail, automatic sell-and-mortgage before bankruptcy, last player standing
wins. Optional `FreeParkingPot`. **No auctions** — unsold property stays with
the bank.

### Trading

Click a player's name, then **Propose a deal**: deeds either way, cash either
way, answered straight away. Only on your turn, one live offer at a time, and
a group with buildings cannot be traded until they are sold back.

### Money

- `StartingCash`, `GoSalary`, `JailFine` are play money.
- Real money is separate: the host dials the stake between `BuyInMin` and
  `BuyInMax`, everyone pays at start, `WinnerTakesPot` hands the pot over.

### Stepping away & crashing out

- ++esc++ offers three choices: back to the game, take a break, leave for
  good.
- **A break** (`AwayMinutes`) and **a crash** (`AbandonClaimMinutes`) both
  hold the seat with money, deeds and buildings untouched.
- **The rest of the table keeps playing** — the absent player's turn is
  skipped. The game only pauses if fewer than two can take a turn.
- If the clock runs out that player is knocked out and everything they held
  returns to the bank. The last player standing still wins.

### Security

Nothing about the game lives on the client: the server rolls every die, moves
every dollar and decides every rule. The NUI renders state and sends intents.

### Test mode

- `SinglePlayerBot = true` — AI players fill the other seats and play on
  their own. No buy-in, nothing written to the database. Set `false` on a
  live server.

### Known limitations

- **English only.** Every other product in the family ships 10 languages;
  this one ships `en`. The locale file is complete and open.
- No auctions.
