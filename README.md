# Starbound Keybinds
Easily bind functions to one or more input methods.

**Note: Parts of this README may be moved to [the Wiki](https://github.com/Silverfeelin/Starbound-Keybinds/wiki) in the future to keep everything neat and tidy.**

## Table of Contents
- [Installation](#installation)
- [Usage](#usage)
- [Syntax Options](#syntax-options)
- [Value Types](#value-types)
- [Bind Object](#bind-object)
- [Planned](#planned)
- [Potential Issues](#potential-issues)
- [Contributing](#contributing)

## Installation

#### Mod pack
This file ensures that, as long it is present in your mods folder, the library will be loaded and will be available in other scripts. It allows this mod to be used as a dependency in other mods, if these other mods don't include a copy of the script.
* Place the `Keybinds.pak` file in your mods folder (eg. `D:\Steam\steamapps\common\Starbound\mods\`).

#### Stand-alone script
This script can be included in your own mod. This way, users don't have to install the Keybinds library themselves to use your mod.
* Place the script in `/<yourMod>/scripts/`. The file name should remain `keybinds.lua`.

[Back to Top](#starbound-keybinds)

## Usage
Before you can create keybinds in your script, you'll first have to load the library.
* At the top of your script, add `require "/scripts/keybinds.lua"`.  

To create a keybind, use the below function anywhere in your script. When a keybind is created, it will automatically activate.  
You can press on any of the function parameters for more information.

> Bind.create([syntax](#syntax), [func](#function), [repeatable](#repeatable))

#### Syntax
Default syntax for one key: `key=value`. Please note the following:

1. Every input key and value pair should be separated with a space.  
  `"f down"`
2. Key and value in a pair should be separated with `=`.  
  `"f=true down=true up=false"`
3. The value is optional. By default, `true` will be set if no value is specified.
⋅⋅* Note that keys which don't accept a boolean will not work without a value.  
  `"f down up=false"`
4. Keys are case-insensitive.  
  `"facingDirection=1"` equals `"facingdirection=1"`.

You can also pass a table instead of a formatted string. Every parameter must include a value here, though.  
``` lua
{f = true, right = true, up = false}
{aimOffset = {10, 10}, aimRelative = {25,20} }
```

#### Function
When the current gmae input matches the arguments of a created and active keybind, the function will be called. You can either pass a function that's already defined, or declare an anonymous function.  
Keep in mind that function parameters are not supported.
``` lua
function myFunction() sb.logInfo("You pressed the F key!") end
Bind.create("f", myFunction)
Bind.create("g", function() sb.logInfo("You pressed the G key!") end)
```

#### Repeatable
This value indicates whether the keybind should activate only once (false), or every game tick as long as the current game input matches the arguments of the keybind (true). Defaults to false.  
Note that a keybind will reset once one or more input options of it no longer match the current game input. This means the keybind `Bind.create("f", func, false)` will trigger once every time the `f` key is pressed down.

[Back to Top](#starbound-keybinds)

## Syntax Options
For every input option, please refer to the documentation of it's value type. The descriptions are based on the default keyboard configuration of the game.

1. `Up` ([boolean](#boolean))  
True while holding W.
2. `Left` ([boolean](#boolean))  
True while holding A.
3. `Down` ([boolean](#boolean))  
True while holding S.
4. `Right` ([boolean](#boolean))  
True while holding D.
5. `primaryFire` ([boolean](#boolean))  
True while holding down the left mouse button.
6. `altFire` ([boolean](#boolean))  
True while holding down the right mouse button.
7. `onGround` ([boolean](#boolean))  
True while standing on the ground.
8. `running` ([boolean](#boolean))  
True while actively running.
9. `walking` ([boolean](#boolean))  
True while actively walking (running while holding shift).
10. `facingDirection` ([float](#float))  
`-1` while facing left. `1` while facing right.
11. `liquidPercentage` ([float](#float))  
Value between `0` and `1`, indicating what percentage of your hitbox is submerged.
12. `position` ([vec2](#vec2))  
Position of your character on the world, in blocks.
13. `aimPosition` ([vec2](#vec2))  
Position of your cursor on the world, in blocks.
14. `aimRelative` ([vec2](#vec2))  
`aimPosition` relative to `position`.
15. `f` ([boolean](#boolean))  
True while holding F, without holding G or H.
16. `g` ([boolean](#boolean))  
True while holding G, without holding F or H.
17. `h` ([boolean](#boolean))  
True while holding H, without holding F or G.
18. `aimOffset` ([vec2](#vec2))  
Value indicating how far `position`, `aimPosition` and `aimRelative` may be off. With no value set, the default value `2,2` is used.  
An example:  `"aimRelative=20,25 aimOffset=2,10"`  
If your relative aim is `(21, 17)`, the result would be true. The horizontal position `21` falls in the range of `20±2` and the vertical position `17` falls in the range of `25±10`.

[Back to Top](#starbound-keybinds)

## Value Types

#### Boolean
`true` or `false`
```lua
Bind.create("f=true, up=false", myFunction)
```

#### Float
Any positive or negative number. Decimals are allowed, but may not work in certain input options.
```lua
Bind.create("facingDirection=1", myFunction)
```

#### Vec2
Two floats, separated with a comma.
```lua
Bind.create("position=100,200 aimOffset=10,10", myFunction)
```

[Back to Top](#starbound-keybinds)

## Bind Object
The `Bind.create` function returnbs a bind object, that you can manipulate with the functions described below.
```lua
-- Create a bind
local bind = Bind.create(args, f, repeatable)
-- Set new input options.
bind:change(newArgs, f, repeatable)
-- Set repeatable to true. Nil values are ignored; the old values will be kept.
bind:change(nil, nil, true)
-- Swap the functions of both binds. Input options and repeatable remain untouched.
bind:swap(otherBind)
-- Unbind; make the binding inactive. Doesn't alter bindings that are already inactive.
bind:unbind()
-- Rebind; make the binding active. Doesn't alter bindings that are already active.
bind:rebind()
```

[Back to Top](#starbound-keybinds)

## Planned
Nothing yet. Feel free to suggest things on [the discussion page](http://community.playstarbound.com/threads/library-tech-keybinds.112606/)!

[Back to Top](#starbound-keybinds)

## Potential Issues
* Multiple tech scripts could bind the same keys. This is actually a double-edged sword, since there may be scenarios where you'd want multiple different actions bound to the same key.

[Back to Top](#starbound-keybinds)

## Contributing
If you have any suggestions or feedback that might help improve this mod, please do post them [the discussion page](http://community.playstarbound.com/threads/library-tech-keybinds.112606/).  
You can also create pull requests and contribute directly to the mod!

[Back to Top](#starbound-keybinds)
