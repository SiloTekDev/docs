# Silo-PonyExpress

A player-driven delivery job for RedM: customers order goods from a catalog,
couriers pick the orders up from a live board and deliver them across the
map — with **double escrow**, an **XP & level system**, timed **express
orders**, level-gated **high-value orders**, **distance pay** and
player-to-player **parcel lockers**.

## Highlights

- **Courier board UI** — a compact dispatch-ledger interface: payout front
  and center, route distance on every card, EXPRESS and HIGH VALUE badges,
  All / Available / Mine tabs, XP progress bar.
- **Double escrow** — the customer's money and the courier's security deposit
  are both held by the server until delivery. Nobody can scam anybody.
- **XP & levels** — every delivery earns XP; levels add a configurable bonus
  to **both pay and XP**. Rank titles from Postboy to Route Master.
- **Express orders** ⏱ — deliver inside the timer for a fat bonus; run out
  and the order returns to the pool.
- **High-value orders** ★ — big orders locked behind courier level, with an
  extra bonus.
- **Distance pay** — longer routes pay more. Measured once at order creation
  (depot → destination), so it cannot be farmed by riding in circles.
- **Parcel lockers** ✉ — send items to any character **by name, online or
  offline**; only the chosen recipient can collect, at the locker the sender
  chose.
- **NPC auto-dispatch** — the server generates delivery work when no
  customers are online, so couriers always have something to do.
- **Depot with live leaderboard** — top couriers by earnings, with level and
  rank; optional self-employment.
- **Custom delivery bag prop** — a PonyExpress leather bag appears on the
  courier's back while on duty, visible to everyone, with guaranteed
  server-side cleanup (never left floating after a crash).
- **7 languages** — EN, RO, IT, DE, FR, ES, PT.
- **Discord webhooks** — orders created / accepted / delivered / declined,
  each with its own optional channel.

## The flow in one minute

**Customer:** use the catalog item → pick goods → the total (plus a
configurable commission) goes into escrow → a courier delivers to wherever
you are now → goods handed over automatically.

**Courier:** `/ponyduty` → `/ponyorders` opens the board → accept an order
(pay the deposit, get the delivery paper) → ride to the destination → use the
paper → **order value + deposit back + every bonus you earned**.

[Installation :material-arrow-right:](installation.md){ .md-button .md-button--primary }
[Features :material-arrow-right:](features.md){ .md-button }
