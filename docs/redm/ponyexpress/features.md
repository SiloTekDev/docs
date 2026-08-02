# Features

## Experience & levels

Every completed delivery earns the courier XP (`XpPerDelivery`, default 25).
XP builds levels (`XpPerLevel` per level, capped at `MaxLevel`), and every
level above 1 adds `LevelBonus` (+3% by default) **on top of the order value
— both money and XP**. The extra pay is minted by the system; the customer
never pays more.

Rank titles are configurable (`Ranks`):

| Level | Default rank |
|-------|--------------|
| 1 | Postboy |
| 4 | Courier |
| 9 | Senior Courier |
| 14 | Express Rider |
| 18 | Route Master |

The courier's own **level, rank and XP progress bar** sit in the order board
header; the depot leaderboard shows every courier's level and rank; level-ups
are announced with a notification. Existing installations are upgraded
automatically (the `xp` column is added on first start).

## Express orders ⏱

A configurable share of auto-dispatch orders spawn with a delivery timer
(`ExpressOrders`):

- The board shows a red **EXPRESS** badge with the time limit.
- Accepting one warns the courier about the timer.
- Deliver inside `TimeLimitMinutes` → `BonusPercent` extra (default +25% of
  the order value).
- Run out of time → the order returns to the pool, the deposit is refunded,
  the run was for nothing. The bonus exists to make the timer matter.

## High-value orders ★

Orders at or above `MinTotal` are badged **HIGH VALUE** (`HighValueOrders`):

- They require courier level `MinLevel` to accept — a level-1 courier gets a
  clear message about the requirement.
- Delivering one pays `BonusPercent` extra (default +10%).

## Distance pay

The courier payout grows with the route length (`DistancePay`) — measured
**once, at order creation, from the depot to the destination**. It is a fixed
number per order, shown on every card (`~2.4 km`), so riding around in
circles earns nothing. `PayPerKm` and `MaxBonus` are configurable.

## Parcel lockers ✉

Configurable pickup points (`Parcels.Lockers` — ship with 3, add as many as
you want). At any locker a player can send a parcel to **any character by
name — online or offline**:

1. Type `Firstname Lastname` and hit **Check** — verified live against the
   framework database (case-insensitive).
2. Pick the items from your satchel, choose **at which locker** the parcel
   will wait, pay a small fee (`SendFee`).
3. Only the chosen recipient can collect it — **not even the sender** — and
   only at the chosen locker.
4. Recipients with parcels waiting get a reminder notification when they
   join the server.

Everything is server-validated: the name is re-checked server-side at send,
items and locker distance are verified, and collection is atomic — no
duplication possible.

## NPC auto-dispatch

When enabled (`AutoNpcOrders`), the server generates system orders at random
intervals with random items and quantities, delivered to configurable NPC
destinations (`RandomNpcTargets`) — couriers always have work, even with no
customers online.

## Depot / HQ

A physical depot location (`Config.Depot`) with a map blip and an interaction
prompt (++g++) that opens a **live leaderboard** — top couriers by total
earned, each with level and rank. Optionally (`AllowSelfEmploy`) the depot
shows a **Become a Courier** button that sets the player's job directly.

## Delivery bag

While on duty, a custom **PonyExpress leather bag** (streamed prop) is
attached to the courier's back — visible to everyone. The prop is
server-tracked with orphan-mode cleanup, so it can never be left floating in
the world after a crash or disconnect. Bone, position offset and rotation are
configurable (`DeliveryBag`).
