# API Reference

All functions live on the table returned by
`exports["silo-libs"]:GetSiloLibs()`. Server functions take `source` as their
first argument; client functions act on the local player.

!!! note "Synchronous by default"
    VORP inventory exports respond synchronously — the silo-libs wrappers
    return the value directly, no callback needed. The only exceptions are
    the explicitly **async** functions below (marked with a callback).

## Core (character / job / money)

Server-side. Works on **VORP and RSG**.

| Function | Returns | Notes |
|----------|---------|-------|
| `GetCharid(source)` | character id | VORP: `charIdentifier` · RSG: `citizenid` |
| `GetFirstname(source)` | string | |
| `GetLastname(source)` | string | |
| `GetJob(source)` | job name | |
| `GetJobGrade(source)` | number | `0` when unavailable |
| `SetJob(source, job, grade)` | boolean | grade optional |
| `GetMoney(source)` | number | cash |
| `GiveMoney(source, amount)` | boolean | |
| `RemoveMoney(source, amount)` | boolean | checks sufficient funds first |
| `Notify(source, text, duration)` | — | framework-native notification |

### FindCharacterByName

```lua
SiloLibs.FindCharacterByName(firstname, lastname, function(result)
    if result then
        print(result.charId, result.firstname, result.lastname)
    else
        print("no such character")
    end
end)
```

Looks a character up by roleplay name **directly in the framework database**,
so it works for **offline** characters too. Case-insensitive and
whitespace-trimmed.

- **VORP:** reads the `characters` table
- **RSG:** reads `players.charinfo` (JSON)
- Requires `oxmysql` — calls back with `nil` if it is not running
- **Callback-based by design.** This API crosses the resource export
  boundary, and cross-resource function refs must never yield. To use it
  sync-style, await it in your own coroutine:

```lua
local p = promise.new()
SiloLibs.FindCharacterByName(first, last, function(result)
    p:resolve(result)
end)
local character = Citizen.Await(p)
```

## Inventory — items (server)

| Function | Maps to (VORP) |
|----------|----------------|
| `GetInventory(source)` | `getUserInventoryItems` |
| `GetItem(source, itemName, metadata, percentage)` | `getItem` |
| `GetItemById(source, itemId)` | `getItemById` |
| `GetItemByName(source, itemName)` | `getItemByName` |
| `GetItemDB(itemName)` | `getItemDB` |
| `GetItemCount(source, itemName, metadata, percentage)` | `getItemCount` |
| `AddItem(source, itemName, amount, metadata)` | `addItem` |
| `RemoveItem(source, itemName, amount, metadata)` | `subItem` |
| `RemoveItemById(source, itemId, amount)` | `subItemById` |
| `SetItemMetadata(source, itemId, metadata, amount)` | `setItemMetadata` |
| `CanCarryItem(source, itemName, amount)` | `canCarryItem` |
| `RegisterUsableItem(itemName, callback, resourceName)` | `registerUsableItem` |
| `UnregisterUsableItem(itemName)` | `unRegisterUsableItem` |

## Inventory — items (client)

| Function | Maps to (VORP) |
|----------|----------------|
| `GetInventory()` | `getInventoryItems` |
| `GetItem(itemName)` | `getInventoryItem` |
| `GetServerItem(item)` | `getServerItem` |
| `CloseInventory()` | `closeInventory` |

## Inventory — weapons (server)

| Function | Maps to (VORP) |
|----------|----------------|
| `GetWeapon(source, weaponId)` | `getUserWeapon` |
| `GetWeapons(source)` | `getUserInventoryWeapons` |
| `GetWeaponBullets(source, weaponId)` | `getWeaponBullets` |
| `GetWeaponComponents(source, weaponId)` | `getWeaponComponents` |
| `GetAmmo(source, ammoType)` | `getUserAmmo` |
| `RemoveAllAmmo(source)` | `removeAllUserAmmo` |
| `AddBullets(source, weaponId, amount)` | `addBullets` |
| `RemoveBullets(source, weaponId, amount)` | `subBullets` |
| `CanCarryWeapons(source, amount, weaponName)` | `canCarryWeapons` |
| `SetWeaponLabel(source, weaponId, label)` | `setWeaponCustomLabel` |
| `SetWeaponSerial(source, weaponId, serial)` | `setWeaponSerialNumber` |
| `SetWeaponDesc(source, weaponId, desc)` | `setWeaponCustomDesc` |
| `CreateWeapon(...)` | `createWeapon` |
| `GiveWeapon(source, weaponId, target)` | `giveWeapon` |
| `RemoveWeapon(source, weaponId)` | `subWeapon` |
| `DeleteWeapon(source, weaponId)` | `deleteWeapon` |

## Custom inventories (server)

Register and manage script-owned inventories (stashes, containers):

`RegisterInventory`, `RemoveInventory`, `DeleteInventory`,
`IsInventoryRegistered`, `GetInventoryData`, `SetInventoryData`,
`OpenInventory`, `CloseInventory`, `OpenPlayerInventory`,
`GetInventorySlots`, `SetInventorySlots`, `SetInventoryItemLimit`,
`SetInventoryWeaponLimit`, `BlacklistInventoryItem`,
`AddInventoryMovePermission`, `AddInventoryTakePermission`,
`AddInventoryCharMovePermission`, `AddInventoryCharTakePermission`,
`GetInventoryItems`, `GetInventoryItemCount`, `AddItemsToInventory`,
`RemoveItemFromInventory`, `UpdateInventoryItem`, `GetInventoryWeapons`,
`GetInventoryWeaponCount`, `AddWeaponsToInventory`,
`RemoveWeaponFromInventory`, `RemoveInventoryWeaponById`

Full VORP export mapping for every function:
[README on GitHub](https://github.com/SiloTekDev/silo-libs#function-mapping-vorp_inventory).

## Writing framework-safe code

Rules learned the hard way — follow them in anything that talks to silo-libs
(or any cross-resource export):

!!! danger "Never yield inside an exported function"
    `Citizen.Await` / `Wait` inside a function that is called across the
    resource boundary errors out. Async silo-libs APIs use callbacks for
    exactly this reason.

!!! danger "Never type-check cross-resource functions"
    Functions received through exports arrive as **callable tables**, not
    `type() == 'function'`. Check for `nil`, or test callability via the
    `__call` metamethod.

!!! danger "Never pass a literal `nil` in the middle of export arguments"
    Cross-resource arguments are msgpack-marshalled and an embedded `nil`
    silently truncates everything after it. Use a no-op function for unused
    callback slots and `false` for optional flags. Trailing nils are safe.
