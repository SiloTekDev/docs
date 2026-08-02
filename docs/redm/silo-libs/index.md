# silo-libs

Unified, framework-agnostic API for SiloTek RedM scripts. Supports
**VORP Core** and **RSG Core** — write your script once, run it on both.

**Free & open source (MIT):** [github.com/SiloTekDev/silo-libs](https://github.com/SiloTekDev/silo-libs)

```lua
local SiloLibs = exports["silo-libs"]:GetSiloLibs()
SiloLibs.AddItem(source, "bread", 1)   -- same call on VORP and RSG
```

Your scripts never call `exports.vorp_inventory:...` or
`exports.rsg_inventory:...` directly. They call `SiloLibs.FunctionName(...)`,
and silo-libs decides at startup which framework is running and which adapter
to use. If VORP or RSG change their internal API, you update the adapter in
silo-libs — not every script.

## Installation

1. Download the latest release: [silo-libs releases](https://github.com/SiloTekDev/silo-libs/releases)
2. Put the `silo-libs` folder in your server's `resources` directory.
3. Add it to `server.cfg` **after** your framework, **before** the scripts
   that use it:

```
ensure vorp_core          # or rsg-core
ensure vorp_inventory     # VORP only
ensure silo-libs
ensure Silo-PonyExpress   # ...and any other SiloTek script
```

On startup the console prints a fingerprint line so you always know what is
running:

```
[silo-libs] v1.2.0 loaded (framework: vorp)
```

## Usage

```lua
-- client or server, identical
local SiloLibs = exports["silo-libs"]:GetSiloLibs()

-- server
local added = SiloLibs.AddItem(source, "bread", 1)
local count = SiloLibs.GetItemCount(source, "bread")
local cash  = SiloLibs.GetMoney(source)

-- client
local items = SiloLibs.GetInventory()
```

The API table is built once when silo-libs starts, based on the detected
framework. If neither VORP nor RSG is running, the table stays empty and a
warning is printed to the console.

!!! warning "Async functions use callbacks"
    Functions that touch the database (like
    [`FindCharacterByName`](api.md#findcharacterbyname)) deliver their result
    through a **callback** — cross-resource function references must never
    yield. Await the callback in your own coroutine if you want sync-style
    code. See the [API Reference](api.md) for details.

## Implementation status

| Category | VORP | RSG |
|----------|------|-----|
| Inventory (items) | :material-check: | stub (logs "not implemented yet") |
| Inventory (weapons) | :material-check: | stub |
| Custom inventories | :material-check: | stub |
| Core (charid / job / money) | :material-check: | :material-check: |
| Character DB lookup | :material-check: | :material-check: |
