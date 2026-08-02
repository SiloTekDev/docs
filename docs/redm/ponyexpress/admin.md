# Admin & Security

## Admin tools

| Command | Where | Effect |
|---------|-------|--------|
| `pecancel <orderId>` | server console, or in game with ace `silo-pe.admin` | force-cancels any order and refunds everyone involved: the customer gets the order total back, the courier gets the deposit back (queued automatically if offline) |
| `pefindchar <firstname> <lastname>` | server console only | diagnostic: walks the whole parcel recipient-lookup chain (silo-libs API, oxmysql state, framework, DB query result) and prints where it breaks |

## Automatic recovery

- Orders left `accepted` by a crash are **released back to the pool
  automatically on the next resource start**, with the deposit refunded.
- If a **courier disconnects** with an accepted order, it returns to the
  pending pool with no penalty and the deposit is refunded.
- Every payment owed to an **offline player** (refunds, returned deposits)
  is queued and paid automatically the moment they reconnect.
- **Pending orders nobody accepts** expire automatically (default: 2 hours)
  with a customer refund — money never gets stuck.
- The orders table stays slim by design: finished orders are removed right
  after their effects are applied.

## Security hardening

- Every money/state transition is **atomic** (conditional SQL updates) — a
  spammed or duplicated event can never double-pay a delivery or refund.
- All player-triggered events are **rate-limited** server-side.
- Depot and locker interactions are **distance-checked on the server** — the
  client prompts are cosmetic.
- Couriers **cannot accept their own orders** (no free leaderboard farming).
- Distance pay is measured **once at order creation** — it cannot be farmed
  by riding around.
- Parcel collection is **atomic** and recipient-locked — no item
  duplication, and nobody but the addressee can open a parcel.
- The delivery paper is bound to the order and courier; replaying it after
  delivery pays nothing.
- All player-provided text is HTML-escaped in the UI.
- The delivery bag prop uses **server-side orphan-mode cleanup** — it can
  never be left floating in the world after a crash.

## Discord webhooks

Four separate staff-log events (in English, independent of the player
language): order created (real customers only — NPC orders don't spam),
order accepted, order delivered (with payout), order declined / returned to
pool. Configure the URLs in `Config.Webhooks`.
