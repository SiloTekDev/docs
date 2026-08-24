# Silo-LuckyWheel

A hand-painted **1899 wheel of fortune standing in the world** — a real wooden
wheel on a real stand with brass fittings and a ratchet pointer that clicks
past every peg.

Nothing about the outcome lives on the client: the server takes the money,
rolls the winning segment, and pays the prize when the wheel stops. Every
player standing nearby watches the same wheel slow down onto the same slice.

## Highlights

- **A real prop, not a menu** — streamed stand and wheel sharing one texture
  dictionary. Walk up, press ++g++, watch it spin.
- **Nine seconds of suspense** — 4 to 6 full rotations, the pointer clacking
  over every wooden peg, a cash-register bell when it settles.
- **Server-rolled outcome** — the winning segment is decided server-side and
  the wheel is animated to land on it. All money moves server-side.
- **Money or items, per slice** — every wheel value pays what you configure:
  cash, inventory items, or both.
- **Pay to spin with cash or with an item** — `SpinCost` takes dollars or an
  inventory item (a gold bar, a casino chip, anything in your item database),
  and each wheel can charge its own price.
- **Odds that match the paint** — the default `Chances` mirror how many
  segments of each value physically exist on the wheel, so the odds are what
  players see. About **84%** return by default: a 16% house edge, tunable
  per value.
- **Big-win announcements** — wins of 30 and 100x can be broadcast so the
  whole saloon knows who just got lucky.
- **Discord logging** — every spin can post to a webhook: who spun, what they
  paid, what they won and the net swing. Busy log and highlights channel can
  be kept apart.
- **Prize stock** — cap how many times a prize can be won per restart. One
  jackpot a night, five of the second tier; a prize that runs out leaves the
  wheel until the server restarts.
- **Unlimited wheels**, each spinning independently, each with its own map
  blip and its own price.
- **10 languages** — en, ro, it, de, fr, es, pt, ru, pl, tr.
- **No database, no NUI** — the lightest install in the family.

## How a spin plays

1. Walk up to the wheel → prompt (++g++) within `PromptDistance`.
2. `SpinCost` is taken — cash or an item, whichever that wheel charges; the
   wheel spins for `SpinDurationMs`.
3. The pointer clicks over the pegs; everyone within `SpectateRange` (40 m)
   watches the same spin.
4. The slice it stops on is what you win — money, items, or both.
5. Wins at or above `AnnounceMinValue` are announced.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Configuration :material-arrow-right:](configuration.md){ .md-button }
