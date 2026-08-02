# Playing & rules

## At the table

Walk up to a chair and press ++g++. The first player to sit is the host and
gets the start button once at least two people are seated.

The board is the interface. Almost everything is done by clicking it:

| What you want | How |
|---|---|
| Roll the dice | Click anywhere on the board |
| Move your piece | Click the space it is heading for — a see-through copy of your piece shows you where |
| Draw a Chance or REDM Chest card | Click that pile on the board |
| Read any deed | Click the space, or a name in your own deed list |
| See what another player holds | Click their name in the list on the left |
| Buy, build, mortgage, end your turn | Buttons in the interface |
| Change camera | ++v++ — top down, first person, wide shot |
| Take a break or leave | ++esc++ or ++backspace++, then choose |

A drawn card slides off its pile toward whoever drew it, and is shown full
screen to **everyone** at the table at the same time. The game is frozen for as
long as it is up.

Every property a player owns lies as a small card on the table in front of
their seat, fanned out and a little crooked. Two properties of the same colour
means two cards of that colour, so you can see at a glance who is close to
owning a whole group.

## Trading

Click a player's name, then **Propose a deal**. Pick deeds from either side,
add cash in either direction, and send it. They answer straight away.

You may only open a negotiation on your own turn, only one offer can be alive
at a time, and a colour group with buildings on it cannot be traded until the
buildings are sold back to the bank.

## Stepping away, and crashing out

Press ++esc++ and you get three choices: go back to the game, take a break, or
leave for good.

**Taking a break** hands you back your character and your screen while the seat
stays yours. Your money, your deeds and your buildings are untouched, nobody
else can sit there, and the walk-away watchdog leaves you alone. Walk back to
your chair and press ++g++ to carry on. You have `AwayMinutes` to do it.

**Crashing or disconnecting** works the same way, with `AbandonClaimMinutes` on
the clock instead. Reconnect, walk up to the table and press ++g++.

Either way **the rest of the table keeps playing** — your turn is simply
skipped until you are back. The game only stops and waits if there are fewer
than two players left who can actually take a turn.

If the clock runs out you are knocked out of that game: your buildings go back
to the bank and every deed you held returns to the market for someone else to
buy. With a buy-in, that stake is lost. The last player standing still wins, so
a two-player game where one never returns ends properly with a winner rather
than being thrown away.

**Leaving for good** forfeits immediately.

## The rules

The standard rules, so anyone who has played the classic game already knows how
this one works:

- Bare rent doubles when one player holds every deed of a colour group
- Houses must go up evenly across a group, and four houses trade up for a hotel
- The bank holds 32 houses and 12 hotels, and can run out
- Railroads pay 25 / 50 / 100 / 200 for one to four owned
- Utilities pay four times the dice, or ten times if both are owned
- A mortgaged deed collects no rent, but it still counts toward how many
  railroads or utilities its owner holds, so it keeps the rent up on the others
- Rolling a double gives you another turn; three in a row sends you to jail
- Doubles rolled to get out of jail move you, but do not earn another turn
- Jail is left by rolling a double, paying `JailFine`, using a pardon, or
  serving `MaxJailTurns` turns
- A player billed by a card who is short of cash has their buildings sold and
  their deeds mortgaged automatically, and folds if that is still not enough
- A seat held for someone who never came back is knocked out, and everything
  they owned goes back to the bank to be bought again
- The last player still standing wins

One optional house rule in `config.lua`: `FreeParkingPot`, which piles taxes
and fines on Free Parking for whoever lands there. There are no auctions — a
property nobody buys simply stays with the bank.

## Tests

The rules engine and the board geometry are covered by a test suite that runs
outside the game, on plain Lua. No server, no RedM, nothing to install beyond
`lua`. Each file finds the resource relative to itself, so it runs from any
folder:

```bash
lua test/test_rules.lua      # rents, groups, building, bankruptcy, trading
lua test/test_geometry.lua   # all 40 spaces, pieces, buildings, deed cards
lua test/test_server.lua     # a full game, one player against two AI
lua test/test_leave.lua      # the table closes when the last player walks away
lua test/test_sync.lua       # seats reach the interface, cards freeze the table
lua test/test_audit.lua      # defects found in review, so they cannot come back
lua test/test_away.lua       # breaks and crashes: held seats, skipped turns, timeouts
```

`test_geometry` checks the things you cannot see in a screenshot: that a house
and a hotel land inside the printed colour strip rather than beside it, that
four houses fit across a space without touching, that six players on one space
get six different spots, that every ownable space has a deed card which was
actually exported, and that no player's cards come to rest on the board or on
the neighbour's cards.
