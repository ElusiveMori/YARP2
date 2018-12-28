# Contributing to the YARP2 codebase

I'm very intent on keeping the code as clean and consistent as possible, and as such, there are some strict rules regarding contributions that I'd like all contributors to follow.

Please, keep in mind that this is nothing personal, it is simply a matter of keeping the codebase in a consistent and maintainable state. I don't want to repeat the mistakes I made in YARP1.

These are the guidelines that __absolutely must be followed__:
* Code __must__ conform to Wurst's code conventions, which can be found here. https://wurstlang.org/manual.html#coding-conventions
* Code __must__ take advantage of Wurst's built-in utilities wherever possible. They are there to make our lives easier, and to avoid reinventing the wheel every time. If you are not sure what approach to take, you can always ask me, and I will also give advice in PR comments wherever applicable. Don't be discouraged if you don't know the entire Wurst's StdLib.

These are guidelines that __should be followed__, but are not critical. 
* Code __should__ take advantage of existing YARP2 systems, utilities and facilities, and integrate well with other systems. Details on this are be below.
* Code __should__ be documented, at least a little bit. This is something I haven't done to my own code yet, but I hope to remedy this. This helps both me and any other contributors that may want to help out in the future.
* Code __should__ be unit tested. If you don't know what unit testing is, there's plenty of resources online to explain that. This helps ensure that code doesn't suddenly break, and also to run CI and verify future PRs.
* Big changes __should__ be discussed in a relevant issue prior to implementation, preferably with a detailed description of what you are implementing, why you are implementing it, and how you plan on implementing it. That way I can provide my own thoughts prior to you investing any time in the implementation.

# Overview of Existing Systems
This section serves as a sort of light documentation for the systems I currently have in YARP2, which you can and should try to use when possible.

## Command System - `/wurst/core/command/`
There's a command system, full with parsers, wrappers and utilities already implemented in YARP2.

### Using Wrappers

Registering a command is very simple. There are 2 wrappers in `CommandWrappers.wurst`, designed for registering simple unit-based and ordinary commands. For example:

```
UnitCommand.register("remove") (context, arguments, what) ->
    what.remove()
```

```
SimpleCommand.register("test") (context, arguments) ->
    print("arg1:" + arguments.getString(1))
    print("arg2:" + arguments.getString(2))
    print("arg3:" + arguments.getString(3))
```

Both variants share 2 common arguments - `context` and `arguments`. `context` is an instance of the `CommandContext` class, which can be used to get:
* The player who issued the command, via `context.owner()`
* The selection of the player at the time of running, via `context.selection()`
* Run another command, via `context.run(...)`
  
`arguments` is an instance of the `Arguments` class, which is a convenience wrapper over the internal logic of how YARP2 parses commands and arguments, which can be used to get:
* Any n-th argument, via `arguments.getType(n)`; possible types are `String`, `Int`, `Real`, `Boolean`.
* The rest of the input as a raw string, up until the next command in the queue, via `arguments.getRest(parsed)`. The `parsed` flag determines whether to use the normal parser to get the input, or return it as a raw string. The difference between the two, is that the former will properly escape characters and correctly parse string literals. For example:
  * Assume input is `cmd [This is a string]`; `arguments.getRest(true)` will return `"This is a string"`, and `arguments.getRest(false)` will return `"[This is a string]"`.
  * Assume input is `cmd This is a string`; `arguments.getRest(true)`  and `arguments.getRest(false)` will both return `"This is a string"`.
  * `arguments.getRest(true)` will also eliminate any duplicate whitespace.

Another feature of the `Arguments` wrapper is that it directly integrates YARP2's custom scripting language. It's a very simple, LISP-like expression-based language, which can be used to write small expressions in arguments (like sin, cod, and rand) and set/get variables. The implementation lies in `Scripting.wurst`, and there are some examples in unit tests.

### Not using wrappers

There's a nuance in the implementation of commands in YARP2. Commands can be chained together using the pipe `|` operator, e.g. `cmd1 arg1 arg2 | cmd2 arg1 arg1 | cmd3 arg1`. Some commands are meant to be able to interrupt or delay the command execution flow, and so, each command implementation must internally call `context.continue()` at some point in order to continue executing the commands.

This is done by default in `SimpleCommand` and `UnitCommand`, however, if we are implementing a command that deals directly with the command flow, we may need to implement a command without the wrappers. To do this, just use the `CommandRegistry.register(command, callback)` function. For example:

```
CommandRegistry.register("wait") (context, arguments) ->
    doAfter(arguments.getReal(1)) ->
        context.continue()
```

That way, `wait 1` will delay the execution of the next command in the chain by 1 second.

### Note

All commands must go into the `/wurst/commands/` folder. 

## YUnit - `/wurst/core/YUnit.wurst`
YUnit is a very important package for YARP2, because it exposes the `yunit` tuple, which is an extension over the native `unit` type. It contains several extension functions for setting/getting properties of units that aren't available through natives, as well as any other unit extensions.

For example:
* Implementing `fly` on units and buildings is done using the `yunit.fly()` method, which in turn...
* Is used by the `yunit.setHeight(height)` method, which works on both units and buildings.
* It keeps track of the unit's current vertex color (tint), custom name, stats, unit tag, etc.

Internally, all this data is kept in the class `YUnitData`, an instance of which is created for every unit on the map automatically.

If you need to add some data to a unit, or to keep track of something, this is the place to put it. In particular, this is very important for Save/Load.

## Utilities - `/wurst/core/util/`
There are various utilities that live in the `/wurst/core/util/` folder. These are not *systems*, they must not register any triggers, however, any code that might be useful to more than one system should be moved here. *You are highly encouraged to skim through these classes, and use them whenever applicable.*

Noteworthy mentions:
* `YARPPrelude.wurst` - publically imports a common set of packages used by YARP2, both internal and from the StdLib. You're encouraged to import this instead of importing each dependency individually. If you add a new dependency, make sure to add it here as well.
* `YPrinting.wurst` - Logging facilities for YARP. This is mostly done to maintain a consistent style across messages, so you are encouraged to use this as well.
* `RectTileIterator.wurst` - A custom iterator which takes a rect, and iterates over all tiles in the rect. Useful in terrain code and save/load.
* `TextTagUtil.wurst` - Utility for managing and recycling text tags. Anything that uses text tags should rely on this for getting/destroying them.
* `ColorUtil.wurst` - Defines custom namings for colors.

## Save / Load - `/wurst/core/saveload/`
This is where all the Save/Load code lives. Please note, that a major part of the implementation resides in the custom StdLib fork that I use, and this is only the user-facing side of things.

Writing about save/load would be a whole tutorial in and of itself, so I'm going to skim over it here. Suffice to say, I don't expect you to deal with it, unless you really want to.

## Autogenerated Data - `/wurst/codegen/`
This is a massive generated list of unit/shop/building definitions. This is how we define all the custom shops, units and buildings in YARP, as well as any metadata associated with them.

Each custom loadable unit type is assigned a "UID", which is a string uniquely identifying the unit, for usage in saving and loading. This is done in order to avoid breaking old builds due to changing unit IDs.

## Miscelanneous Init - `/wurst/core/MiscInit.wurst`
This is where various small triggers reside, which don't really warrant a system of their own.

## Spawner - `/wurst/core/Spawner.wurst`
Contains the code for the Spawner, as well as utility methods for getting a player's spawner.

## UnitTag - `/wurst/core/UnitTag.wurst`
Contains the code for text tags which are attached to units. This is used in "speech bubbles", name tags, and potentially other things in the future.

## Region Specifier - `/wurst/core/RegionSpecifier.wurst`
Contains the code for region specifiers, which are special units used for specifying rectangular regions on the map - currently used for Save/Load, weather and atmospheric effects. There are also potentially many other uses for them.

## Effect Proxy - `/wurst/core/EffectProxy.wurst`
(WIP) Several classes for creating "effect proxies", which are wrappers over the `specialeffect` type. Currently used for implementing negative fly, pitching and rolling on units.

## Other

I've skimmed some of the other systems that are included in YARP2, but they are fairly independent from others and not nearly as important as the ones above. You are, of course, welcome to nonetheless read through them, and ask me questions if something is unclear.