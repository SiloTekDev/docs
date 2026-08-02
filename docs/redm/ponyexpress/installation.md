# Installation

The package contains **three resources** — all three go into your server's
`resources` folder:

| Resource | What it is |
|----------|------------|
| `Silo-PonyExpress-Props` | streamed props (delivery bag, textures) — **must start first** |
| `silo-libs` | SiloTek framework bridge (VORP / RSG auto-detection) — [free & open source](https://github.com/SiloTekDev/silo-libs), **v1.2.0+** |
| `Silo-PonyExpress` | the delivery job itself |

## 1. server.cfg — start order matters

```
ensure vorp_core          # or rsg-core — the framework is auto-detected
ensure vorp_inventory     # VORP only
ensure oxmysql

ensure Silo-PonyExpress-Props   # props FIRST — the job needs the bag model
ensure silo-libs          # then the framework bridge
ensure Silo-PonyExpress   # then the job
```

!!! warning "silo-libs v1.2.0 or newer"
    The parcel system looks recipients up in the framework database through
    `silo-libs`. On an older silo-libs the server console prints a warning at
    startup and the recipient check always answers "not found". Grab the
    latest from [GitHub releases](https://github.com/SiloTekDev/silo-libs/releases).

## 2. First start

- The four database tables create themselves automatically
  (`silo_ponyexpress`, `silo_ponyexpress_payouts`, `silo_ponyexpress_stats`,
  `silo_ponyexpress_parcels`) — **no SQL to import**. On every start the
  schema is audited, and upgrades (new columns) are applied automatically.
- The framework (VORP or RSG-Core) is detected automatically.

## 3. Add the two items

Add `OrderCatalogItem` and `DeliveryPaperItem` (defaults: `salt`,
`foaieordin`) to your inventory / items database. The first opens the
customer catalog, the second is the delivery paper couriers use at the
drop-off point.

## 4. Set up the job

- Put your courier job name(s) in `JobNames` (defaults: `ponyexpress`,
  `postman`).
- Adjust the item catalog under `AllowedItemsForDelivery` (item, label,
  icon, fixed price, max per order).

That is the only required configuration — everything else ships with sane
defaults.

## Known limitations

- On RSG, the silo-libs **inventory** adapter is not finished yet — VORP is
  the fully supported framework today.
- Old installations using the legacy `ponyexpress_orders` table are not
  migrated automatically — the new tables start empty.
