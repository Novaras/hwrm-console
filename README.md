# hwrm console

This is a mod for Homeworld Remastered which adds the ability to bring up an ingame console capable of executing Lua code.

The console can run whole files, and also comes prepackaged with some commands.

This mod is brand new, expect some bugs!

## Controls

The console is brought up with the `P` key. Close it again with `ESC`.

Due to the way the mod had to be made (there appears to be no way to retrieve the text content of a regular text input element), the text input of the console is a custom solution, so some of the keybinds are a little unusual:

- Shift doesn't work; use caps lock toggling
- Alphanumeric keys are bound as expected
- Quotes are on the `\`/`"` key (under backspace)
- Period (full stop) is on `f1`
- Comma is on `f2`
- Semicolon is on `f3`
- Square brackets are on `f4` and `f5`

## Executing Lua In-Console

You can run any Lua you like in-console using `do` like so:

```
do <lua code>
```

Examples:
```
do Player_SetRU(0, 10000000)

do Player_Kill(1)

do SobGroup_SetHealth("Player_Ships0", 0.1)

do print('hi!')
```

## Running Lua From a Script

You can execute existing scripts using `run`:

```
run <path to file>
```

This internally calls [`dofilepath`](https://github.com/HWRM/KarosGraveyard/wiki/Function;-dofilepath), which usually expects a relative path root to be set (see [this explanation](https://github.com/HWRM/KarosGraveyard/wiki/Tutorial;-Relative-File-Paths)). If the supplied path has no prefix, the `"data:"` prefix will be added by default.

Example:

```
run scripts/my-script.lua
```

This is equivalent to `dofilepath("data:scripts/my-script.lua");`.

## Premade Commands

The console currently has a small group of premade commands.

Required arguments are denoted by `<angle-brackets>`, parameters are denoted by `{curly-braces}`. **Parameters may be given in any order.** Parameters like `{x=<y>}` indicate that it should be written as `x=10`, for example. If a parameter is followed by a question mark `?`, it is **optional** (may be omitted).

### `spawn`:

Spawns ships of a given type.

```
spawn <ship-type> {p?=<player-id>}? {c?=<count>}? {pos?=<x y z>}?
```

Defaults:
- `player-id` is `0`
- `count` is `1`
- `pos` is `0 0 0`

Examples:

Spawn one Sajuuk for player `0` at pos `0 0 0`
```
spawn kpr_sajuuk
```

Spawn 10 Hgn DDs for player `1` at `10000, 6000, 0`:
```
spawn hgn_destroyer c=10 pos=10000 6000 0
```

### `attack`:

Causes one player's ships to attack another player's ships.

```
attack {target=<player-id>} {attacker=<player-id>}?
```

Defaults:
- `attacker` is `0`

Examples:

Cause `0` to attack `1`
```
attack target=1
```

Cause `2` to attack `1`
```
attack target=1 attacker=2
```

### `fight`:

Causes two players to attack eachother.

```
fight <player-id> <player-id>
```

Examples:

Cause player `1` and `2` to attack eachother with all ships:

```
fight 1 2
```

### `ru`:

Sets or bestows RU for a player.

```
ru <verb> {value=<amount>} {p=<player-id>}?
```

The `<verb>` may be one of `set` or `give`.

Defaults:
- `p` is `0`

Examples:

Bestow 1000RU to player 1:
```
ru give value=1000 p=1
```

Set player 0's RUs to 9999999:
```
ru set p=0 value=9999999
```

### `kill`:

Kills the specified player.

```
kill <player-id>
```

Examples:

Kills player 1:
```
kill 1
```

### `destroy`:

Destroys ships of a given type or family. By default, this effects ships for all players, unless a player is specified.

```
destroy ({t=<ship-type>} | {f=<attack-family>}) {p=<player-index>}?
```

The `t` and `f` parameters are mutually exclusive.

Examples:

Destroys all Hgn Interceptors for any player:
```
destroy t=hgn_interceptor
```

Destroys all Kushan Scouts for player 2:
```
destroy t=kus_scout p=2
```

Destroys all corvettes for all players:
```
destroy f=corvette
```

### `gametime`:

Logs the current gametime.

```
gametime
```
