# KrisssLib Documentation

A universal library for ESX and QB-Core frameworks in FiveM, providing a unified API for common operations.

## Table of Contents

- [Installation](#installation)
- [Features](#features)
- [Configuration](#configuration)
- [API Reference](#api-reference)
  - [Player Functions](#player-functions)
  - [Money Functions](#money-functions)
  - [Item Functions](#item-functions)
  - [Job Functions](#job-functions)
  - [Gang Functions](#gang-functions)
  - [UI Functions](#ui-functions)
  - [Vehicle Functions](#vehicle-functions)
  - [Target Functions](#target-functions)
  - [Utility Functions](#utility-functions)
- [Examples](#examples)
- [Dependencies](#dependencies)
- [Troubleshooting](#troubleshooting)

## Installation

1. Download the latest release from [GitHub](https://github.com/yourusername/krissslib)
2. Extract the files to your FiveM resources folder
3. Add `ensure krissslib` to your server.cfg
4. Start your server

## Features

- **Framework Agnostic**: Works with both ESX and QB-Core
- **Auto-Detection**: Automatically detects your framework
- **Unified API**: Same functions work across both frameworks
- **Modern UI**: Prioritizes ox_lib for better user experience
- **Comprehensive**: Covers all common operations
- **Easy to Use**: Simple and intuitive function names

## Configuration

KrisssLib automatically detects your framework on startup. No configuration required!

Supported frameworks:
- ESX (es_extended)
- QB-Core (qb-core)

## API Reference

### Player Functions

#### `getPlayerData()`
**Client-side only**

Returns comprehensive player data in a unified format.

```lua
local playerData = exports.krissslib:getPlayerData()
print(playerData.name) -- Player name
print(playerData.job.name) -- Job name
print(playerData.money.cash) -- Cash amount
```

**Returns:**
```lua
{
  source = number,
  citizenid = string,
  name = string,
  job = {
    name = string,
    label = string,
    grade = number,
    grade_name = string,
    grade_label = string,
    onduty = boolean
  },
  gang = table,
  money = {
    cash = number,
    bank = number,
    crypto = number
  },
  metadata = table,
  items = table
}
```

#### `hasJob(jobName, minGrade)`
**Client-side only**

Check if player has a specific job with minimum grade.

```lua
local isPolice = exports.krissslib:hasJob('police', 2)
if isPolice then
    print("Player is police with grade 2 or higher")
end
```

**Parameters:**
- `jobName` (string): The job name to check
- `minGrade` (number, optional): Minimum grade required (default: 0)

**Returns:** `boolean`

#### `hasGang(gangName, minGrade)`
**Client-side only** | **QB-Core only**

Check if player has a specific gang with minimum grade.

```lua
local isBallas = exports.krissslib:hasGang('ballas', 1)
if isBallas then
    print("Player is in Ballas gang")
end
```

**Parameters:**
- `gangName` (string): The gang name to check
- `minGrade` (number, optional): Minimum grade required (default: 0)

**Returns:** `boolean`

### Money Functions

#### `addMoney(moneyType, amount)`
**Client-side** (triggers server event)

Add money to player account.

```lua
exports.krissslib:addMoney('cash', 1000)
exports.krissslib:addMoney('bank', 5000)
```

**Parameters:**
- `moneyType` (string): 'cash', 'bank', or 'crypto'
- `amount` (number): Amount to add

#### `removeMoney(moneyType, amount)`
**Client-side** (triggers server event)

Remove money from player account.

```lua
exports.krissslib:removeMoney('cash', 500)
exports.krissslib:removeMoney('bank', 2000)
```

**Parameters:**
- `moneyType` (string): 'cash', 'bank', or 'crypto'
- `amount` (number): Amount to remove

#### `getMoney(moneyType)`
**Client-side only**

Get current money amount.

```lua
local cash = exports.krissslib:getMoney('cash')
local bank = exports.krissslib:getMoney('bank')
print("Cash: $" .. cash .. ", Bank: $" .. bank)
```

**Parameters:**
- `moneyType` (string): 'cash', 'bank', or 'crypto'

**Returns:** `number`

### Item Functions

#### `hasItem(itemName, amount)`
**Client-side only**

Check if player has a specific item.

```lua
local hasPhone = exports.krissslib:hasItem('phone', 1)
if hasPhone then
    print("Player has a phone")
end
```

**Parameters:**
- `itemName` (string): Item name to check
- `amount` (number, optional): Amount required (default: 1)

**Returns:** `boolean`

#### `addItem(itemName, amount, metadata)`
**Client-side** (triggers server event)

Add item to player inventory.

```lua
exports.krissslib:addItem('phone', 1)
exports.krissslib:addItem('weapon_pistol', 1, {ammo = 30})
```

**Parameters:**
- `itemName` (string): Item name to add
- `amount` (number, optional): Amount to add (default: 1)
- `metadata` (table, optional): Item metadata

#### `removeItem(itemName, amount)`
**Client-side** (triggers server event)

Remove item from player inventory.

```lua
exports.krissslib:removeItem('phone', 1)
exports.krissslib:removeItem('bread', 2)
```

**Parameters:**
- `itemName` (string): Item name to remove
- `amount` (number, optional): Amount to remove (default: 1)

### Job Functions

#### `setJob(jobName, grade)`
**Client-side** (triggers server event)

Set player job and grade.

```lua
exports.krissslib:setJob('police', 3)
exports.krissslib:setJob('mechanic', 0)
```

**Parameters:**
- `jobName` (string): Job name to set
- `grade` (number, optional): Job grade (default: 0)

### Gang Functions

#### `setGang(gangName, grade)`
**Client-side** (triggers server event) | **QB-Core only**

Set player gang and grade.

```lua
exports.krissslib:setGang('ballas', 2)
exports.krissslib:setGang('none', 0)
```

**Parameters:**
- `gangName` (string): Gang name to set
- `grade` (number, optional): Gang grade (default: 0)

### UI Functions

#### `notify(message, type, duration)`
**Client-side only**

Show notification to player.

```lua
exports.krissslib:notify('Hello World!', 'success', 5000)
exports.krissslib:notify('Error occurred!', 'error')
exports.krissslib:notify('Information', 'inform')
```

**Parameters:**
- `message` (string): Notification message
- `type` (string, optional): 'success', 'error', 'inform' (default: 'inform')
- `duration` (number, optional): Duration in milliseconds (default: 5000)

#### `progressBar(data, callback)`
**Client-side only**

Show progress bar with optional animation and props.

```lua
exports.krissslib:progressBar({
    name = 'repairing',
    duration = 10000,
    label = 'Repairing vehicle...',
    canCancel = true,
    disableMovement = true,
    animDict = 'mini@repair',
    anim = 'fixing_a_player'
}, function(success)
    if success then
        print('Repair completed!')
    else
        print('Repair cancelled!')
    end
end)
```

**Parameters:**
- `data` (table): Progress bar configuration
  - `name` (string, optional): Progress name
  - `duration` (number, optional): Duration in milliseconds (default: 5000)
  - `label` (string, optional): Progress label (default: 'Processing...')
  - `canCancel` (boolean, optional): Allow cancellation (default: true)
  - `disableMovement` (boolean, optional): Disable movement (default: false)
  - `animDict` (string, optional): Animation dictionary
  - `anim` (string, optional): Animation name
  - `prop` (string, optional): Prop model
- `callback` (function, optional): Callback function with success parameter

#### `inputDialog(header, rows, callback)`
**Client-side only**

Show input dialog to collect user input.

```lua
exports.krissslib:inputDialog('Enter Information', {
    {type = 'input', label = 'Name', placeholder = 'Enter your name'},
    {type = 'number', label = 'Age', placeholder = 'Enter your age'},
    {type = 'select', label = 'Gender', options = {
        {value = 'male', label = 'Male'},
        {value = 'female', label = 'Female'}
    }}
}, function(result)
    if result then
        print('Name: ' .. result[1])
        print('Age: ' .. result[2])
        print('Gender: ' .. result[3])
    end
end)
```

#### `showTextUI(text, position)`
**Client-side only**

Show persistent text UI.

```lua
exports.krissslib:showTextUI('Press [E] to interact', 'right')
```

**Parameters:**
- `text` (string): Text to display
- `position` (string, optional): Position on screen (default: 'right')

#### `hideTextUI()`
**Client-side only**

Hide the text UI.

```lua
exports.krissslib:hideTextUI()
```

#### `alertDialog(data, callback)`
**Client-side only**

Show alert dialog.

```lua
exports.krissslib:alertDialog({
    header = 'Information',
    content = 'This is an important message!',
    centered = true
}, function(result)
    print('Alert closed')
end)
```

#### `confirmDialog(data, callback)`
**Client-side only**

Show confirmation dialog.

```lua
exports.krissslib:confirmDialog({
    header = 'Confirm Action',
    content = 'Are you sure you want to proceed?'
}, function(result)
    if result == 'confirm' then
        print('User confirmed')
    else
        print('User cancelled')
    end
end)
```

### Vehicle Functions

#### `spawnVehicle(model, coords, heading, callback)`
**Client-side only**

Spawn a vehicle at specified coordinates.

```lua
exports.krissslib:spawnVehicle('adder', vector3(0, 0, 0), 0.0, function(vehicle)
    print('Vehicle spawned: ' .. vehicle)
    SetVehicleNumberPlateText(vehicle, 'KRISSS')
end)
```

**Parameters:**
- `model` (string): Vehicle model name
- `coords` (vector3): Spawn coordinates
- `heading` (number): Vehicle heading
- `callback` (function, optional): Callback with vehicle entity

**Returns:** `number` (vehicle entity)

#### `deleteVehicle(vehicle)`
**Client-side only**

Delete a vehicle entity.

```lua
local vehicle = GetVehiclePedIsIn(PlayerPedId(), false)
exports.krissslib:deleteVehicle(vehicle)
```

**Parameters:**
- `vehicle` (number): Vehicle entity to delete

### Target Functions

#### `addBoxZone(name, coords, size, options, targetOptions)`
**Client-side only**

Add a box zone for targeting system.

```lua
exports.krissslib:addBoxZone('shop_zone', vector3(0, 0, 0), vector3(2, 2, 2), {
    rotation = 0,
    debug = false
}, {
    {
        name = 'shop',
        icon = 'fas fa-shopping-cart',
        label = 'Open Shop',
        onSelect = function()
            print('Shop opened!')
        end
    }
})
```

**Parameters:**
- `name` (string): Zone name
- `coords` (vector3): Zone center coordinates
- `size` (vector3): Zone size
- `options` (table): Zone options
- `targetOptions` (table): Target interaction options

#### `removeZone(name)`
**Client-side only**

Remove a target zone.

```lua
exports.krissslib:removeZone('shop_zone')
```

**Parameters:**
- `name` (string): Zone name to remove

### Utility Functions

#### `getFramework()`
**Client & Server**

Get the detected framework name.

```lua
local framework = exports.krissslib:getFramework()
print('Framework: ' .. framework) -- 'esx' or 'qb'
```

**Returns:** `string` ('esx', 'qb', or nil)

#### `getFrameworkObject()`
**Client & Server**

Get the framework object (ESX or QBCore).

```lua
local frameworkObj = exports.krissslib:getFrameworkObject()
if frameworkObj then
    print('Framework object available')
end
```

**Returns:** `table` (ESX or QBCore object)

#### `isFrameworkReady()`
**Client & Server**

Check if framework is loaded and ready.

```lua
local isReady = exports.krissslib:isFrameworkReady()
if isReady then
    print('Framework is ready!')
end
```

**Returns:** `boolean`

## Server-Side Functions

### Player Functions

#### `getPlayer(source)`
**Server-side only**

Get player object by source ID.

```lua
local player = exports.krissslib:getPlayer(source)
if player then
    print('Player found: ' .. source)
end
```

#### `getPlayerByIdentifier(identifier)`
**Server-side only**

Get player object by identifier.

```lua
local player = exports.krissslib:getPlayerByIdentifier('license:abc123')
if player then
    print('Player found by identifier')
end
```

#### `getPlayerData(source)`
**Server-side only**

Get player data by source ID.

```lua
local playerData = exports.krissslib:getPlayerData(source)
if playerData then
    print('Player name: ' .. playerData.name)
end
```

### Server Money Functions

#### `addMoney(source, moneyType, amount)`
**Server-side only**

Add money to player account.

```lua
exports.krissslib:addMoney(source, 'cash', 1000)
```

#### `removeMoney(source, moneyType, amount)`
**Server-side only**

Remove money from player account.

```lua
exports.krissslib:removeMoney(source, 'cash', 500)
```

#### `getMoney(source, moneyType)`
**Server-side only**

Get player money amount.

```lua
local cash = exports.krissslib:getMoney(source, 'cash')
```

### Server Item Functions

#### `addItem(source, itemName, amount, metadata)`
**Server-side only**

Add item to player inventory.

```lua
exports.krissslib:addItem(source, 'phone', 1, {battery = 100})
```

#### `removeItem(source, itemName, amount)`
**Server-side only**

Remove item from player inventory.

```lua
exports.krissslib:removeItem(source, 'phone', 1)
```

#### `hasItem(source, itemName, amount)`
**Server-side only**

Check if player has item.

```lua
local hasPhone = exports.krissslib:hasItem(source, 'phone', 1)
```

#### `getItemCount(source, itemName)`
**Server-side only**

Get item count in player inventory.

```lua
local phoneCount = exports.krissslib:getItemCount(source, 'phone')
```

## Examples

### Basic Player Information
```lua
-- Client-side
local playerData = exports.krissslib:getPlayerData()
if playerData then
    exports.krissslib:notify('Welcome ' .. playerData.name .. '!', 'success')
    
    if exports.krissslib:hasJob('police') then
        exports.krissslib:notify('You are on duty as police!', 'inform')
    end
end
```

### Shop System
```lua
-- Client-side
local shopItems = {
    {name = 'bread', price = 5, label = 'Bread'},
    {name = 'water', price = 3, label = 'Water'},
    {name = 'phone', price = 100, label = 'Phone'}
}

function openShop()
    local options = {}
    for _, item in pairs(shopItems) do
        table.insert(options, {
            title = item.label,
            description = 'Price: $' .. item.price,
            onSelect = function()
                buyItem(item)
            end
        })
    end
    
    exports.krissslib:showContextMenu(options)
end

function buyItem(item)
    local playerMoney = exports.krissslib:getMoney('cash')
    if playerMoney >= item.price then
        exports.krissslib:removeMoney('cash', item.price)
        exports.krissslib:addItem(item.name, 1)
        exports.krissslib:notify('Purchased ' .. item.label .. ' for $' .. item.price, 'success')
    else
        exports.krissslib:notify('Not enough money!', 'error')
    end
end
```

### Job System
```lua
-- Server-side
RegisterCommand('setjob', function(source, args)
    local player = exports.krissslib:getPlayer(source)
    if not player then return end
    
    local jobName = args[1]
    local grade = tonumber(args[2]) or 0
    
    if jobName then
        exports.krissslib:setJob(source, jobName, grade)
        exports.krissslib:notify(source, 'Job set to ' .. jobName .. ' grade ' .. grade, 'success')
    else
        exports.krissslib:notify(source, 'Usage: /setjob [job] [grade]', 'error')
    end
end, true)
```

### Crafting System
```lua
-- Client-side
function craftItem()
    local hasItems = exports.krissslib:hasItem('metal', 5) and exports.krissslib:hasItem('rubber', 3)
    
    if not hasItems then
        exports.krissslib:notify('You need 5 metal and 3 rubber to craft!', 'error')
        return
    end
    
    exports.krissslib:progressBar({
        name = 'crafting',
        duration = 15000,
        label = 'Crafting item...',
        canCancel = true,
        disableMovement = true,
        animDict = 'mini@repair',
        anim = 'fixing_a_player'
    }, function(success)
        if success then
            exports.krissslib:removeItem('metal', 5)
            exports.krissslib:removeItem('rubber', 3)
            exports.krissslib:addItem('weapon_pistol', 1)
            exports.krissslib:notify('Crafting completed!', 'success')
        else
            exports.krissslib:notify('Crafting cancelled!', 'error')
        end
    end)
end
```

## Dependencies

### Required
- None (auto-detects framework)

### Optional (Recommended)
- `ox_lib` - For modern UI components
- `ox_target` or `qb-target` - For targeting system
- `progressBars` - ESX progress bars (if not using ox_lib)
- `qb-input` - QB input dialogs (if not using ox_lib)
- `qb-menu` - QB menus (if not using ox_lib)

## Troubleshooting

### Framework Not Detected
**Problem:** KrisssLib prints "No supported framework detected!"

**Solution:**
1. Ensure ESX or QB-Core is installed and started
2. Check that the framework resource name is correct:
   - ESX: `es_extended`
   - QB-Core: `qb-core`
3. Make sure KrisssLib starts after your framework

### Functions Not Working
**Problem:** Functions return nil or don't work

**Solution:**
1. Check if framework is ready: `exports.krissslib:isFrameworkReady()`
2. Ensure you're using the correct side (client vs server)
3. Check console for any error messages

### UI Components Not Showing
**Problem:** Notifications, progress bars, etc. not showing

**Solution:**
1. Install `ox_lib` for the best experience
2. Ensure alternative UI resources are installed and started
3. Check that UI resources are compatible with your framework

### Performance Issues
**Problem:** Script causing performance issues

**Solution:**
1. Avoid calling functions in tight loops
2. Cache player data when possible
3. Use server-side functions for data validation

## Support

If you encounter any issues or have questions:

1. Check the [documentation](#) thoroughly
2. Search existing [issues](https://github.com/yourusername/krissslib/issues)
3. Create a new issue with detailed information

## Contributing

We welcome contributions! Please see our [contributing guidelines](CONTRIBUTING.md) for more information.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

**KrisssLib** - Making FiveM development easier, one function at a time! 🎮
