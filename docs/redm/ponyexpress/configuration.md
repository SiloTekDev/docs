# Configuration

Everything lives in `config.lua` — an open file with plain comments, one
option per line, grouped by section.

## General

| Option | Default | Meaning |
|--------|---------|---------|
| `Language` | `'EN'` | `EN` `RO` `IT` `DE` `FR` `ES` `PT` — all texts in the open file `l/l.lua` |
| `Dev` | `false` | debug prints in console (testing only) |
| `JobNames` | `ponyexpress`, `postman` | jobs allowed to go on duty |

## Items & commands

| Option | Default | Meaning |
|--------|---------|---------|
| `OrderCatalogItem` | `'salt'` | using this item opens the customer catalog |
| `DeliveryPaperItem` | `'foaieordin'` | delivery paper; using it at the destination completes the delivery |
| `OpenWorkerUiCommand` | `'ponyorders'` | opens the courier board |
| `ToggleDutyCommand` | `'ponyduty'` | duty on/off |

## Delivery

| Option | Default | Meaning |
|--------|---------|---------|
| `DeliveryDistance` | `15.0` | max distance (m) from the drop-off to complete |
| `MapPreviewDuration` | `10000` | ms a previewed location stays marked on the map |
| `MaxActiveAcceptedOrders` | `1` | orders a courier can hold at once |
| `PendingOrderExpiry` | 2 h / check 5 min | unaccepted orders auto-refund and disappear |

## Economy

| Option | Default | Meaning |
|--------|---------|---------|
| `SecurityDepositMode` | `'match_order_total'` | or `'flat'` with `SecurityDepositFlatAmount` |
| `DeliveryCommissionPercent` | `10` | % added on top of the subtotal — charged to the customer, paid to the courier |
| `AllowedItemsForDelivery` | water, bread, coffee | your catalog: item, label, icon, fixed price, max per order |

!!! tip "Balance note"
    Couriers on NPC orders are paid the catalog prices. Keep those prices at
    (or below) your shop prices — if the catalog pays more than the items
    cost to buy, couriers can profit by buying stock and delivering it.

## Bonuses

```lua
DistancePay = {
    Enabled = true,
    PayPerKm = 2.0,     -- $ per km of route (depot -> destination)
    MaxBonus = 20.0     -- cap per order
},

ExpressOrders = {
    Enabled = true,
    ChancePercent = 25,      -- % of NPC orders that spawn as EXPRESS
    TimeLimitMinutes = 10,
    BonusPercent = 25        -- extra % of order value when on time
},

HighValueOrders = {
    Enabled = true,
    MinTotal = 40.0,    -- an order at/above this total is HIGH VALUE
    MinLevel = 5,       -- courier level required to accept
    BonusPercent = 10
},
```

## Experience

```lua
XpPerDelivery = 25,   -- base XP per completed delivery
XpPerLevel = 250,     -- XP needed per level
MaxLevel = 20,        -- level cap
LevelBonus = 0.03,    -- +3% pay AND XP per level above 1
Ranks = { ... },      -- rank titles per level range
```

## Parcels

```lua
Parcels = {
    Enabled = true,
    SendFee = 2.0,
    InteractionDistance = 2.0,
    Lockers = {
        { name = 'Valentine Post',  coords = vector3(...), blip = true },
        { name = 'Blackwater Post', coords = vector3(...), blip = true },
        { name = 'Rhodes Post',     coords = vector3(...), blip = true },
    }
},
```

## Delivery bag

```lua
DeliveryBag = {
    UseProp = true,                      -- attach the bag prop on duty
    Prop = 'pe_delivery_bag',
    Bone = 'SKEL_Spine3',
    Offset = vector3(0.0, -0.30, -0.25), -- sideways / back / up-down
    Rotation = vector3(0.0, 0.0, 0.0),
    CheckIntervalMs = 5000
},
```

## NPC auto-dispatch & depot

`AutoNpcOrders` controls the generator (intervals, items per order,
quantities, optional courier deposit); `RandomNpcTargets` is the list of NPC
destinations. `Config.Depot` sets the depot location, blip, leaderboard size
and optional self-employment (`AllowSelfEmploy`, `EmployJobName`).

## Webhooks

`Config.Webhooks` — four Discord events (`OrderCreated`, `OrderAccepted`,
`OrderDelivered`, `OrderDeclined`). Same URL on all four = one channel;
different URLs = separate channels; empty string = that event disabled.

## Notifications

At the bottom of the config:

```lua
function NotifyFunction(source, text, notifyType)
    -- auto-detects VORP / RSG; replace with your own system if needed
end
```
