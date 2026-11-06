# ulx_luadev

A Garry's Mod addon that gives [ULX](https://github.com/TeamUlysses/ulx) commands for
[luadev](https://github.com/Metastruct/luadev) and easylua, on a server with ULX and no aowl.
Install it in `garrysmod/addons/` beside ULX and luadev.

## Commands

Each is `ulx <name>` in the console and `!<name>` in chat. `!lm` is for every player (luadev's
`sv_allowcslua` rule applies); the rest are for superadmins. ULX's access decides who may call them.

| Command | What it does |
|---|---|
| `!l <lua>` | Runs Lua on the server, with easylua's `me`, `this`, `there`, `trace` |
| `!ls <lua>` | Runs Lua on the server and every client |
| `!lc <lua>` | Runs Lua on every client |
| `!lm <lua>` | Runs Lua on your own client |
| `!lb <lua>` | Runs Lua on the server and your own client |
| `!lsc <player> <lua>` | Runs Lua on one player's client |
| `!print <value>` | Prints a value on the server |
| `!table <table>` | Prints a table on the server |
| `!printc <value>` | Prints a value from every client in the server console |
| `!printm <value>` | Prints a value from your own client in the server console |
| `!printb <value>` | Prints a value on the server and from your own client |
| `!keys <table> [search]` | Lists a table's keys on the server |
| `!keysm <table> [search]` | Lists a table's keys on your own client |
| `!lfile <file> <server\|shared\|clients>` | Runs a Lua file, as `lua_send_*` does |
| `!lent <file>`, `!lwep <file>`, `!leffect <file>` | Registers an entity, weapon or effect from its file or folder |
| `!lwatch <file> <server\|shared\|clients>` | Sends a file again each time it changes, as `lua_watch` does |
| `!lunwatch [index]` | Stops one watch, or all of them |
| `!lcopy <value>` | Copies a value from the server to your clipboard |
| `!lspawn <class or model>` | Spawns an entity where you aim |

## Output

A run on the server tells its caller what it printed in chat, else what it returned, else `ok`.
Runs only on clients tell nothing, because clients do not answer.

The caller's client also plays a sound: success, or error. If a run returns a transaction (a value
with `State()` and `OnSettle(fn)`) still pending a frame later, it plays a blip, then success or
error as the transaction settles. If it is still pending at the timeout, the caller is told and
hears a neutral sound, and its end goes unheard.

## Showing easylua's relative names

`!print` of `me`, `this`, `we`, `wep`, `veh`, `phys`, `there`, `here`, `dir` or `trace` also draws
it on the caller's client for a moment, without needing `developer`:

- a box around each entity, with lines for its name, class, position and angles; an NPC's or
  NextBot's activity, animation and target; and an NPC's schedule
- a line from your eyes to a position, along your aim for `dir`, and along a trace with a box around
  what it hit

Another addon can add lines to an entity's description:

```lua
hook.Add("ULXLuaDevDescribe", "my_addon", function(ent, lines)
	lines[#lines + 1] = "health: " .. ent:Health()
end)
```

## Convars

Set on the server, for example in `server.cfg`, and read as they are used.

| Convar | Default | What it sets |
|---|---|---|
| `sv_luadev_output_maxlines` | `1` | Lines of output told in chat; `0` for none |
| `sv_luadev_output_maxchars` | `120` | Length of a line in chat; `0` for uncut |
| `sv_luadev_output_overlay` | `2` | Seconds `!print` shows a relative name; `0` for not |
| `sv_luadev_output_timeout` | `10` | Seconds a run's work is listened for |
| `sv_luadev_output_success` | `buttons/button14.wav` | Success sound; empty for none |
| `sv_luadev_output_error` | `buttons/button10.wav` | Error sound; empty for none |
| `sv_luadev_output_progress` | `buttons/blip1.wav` | In-progress sound; empty for none |
| `sv_luadev_output_neutral` | `buttons/button16.wav` | Sound at the timeout; empty for none |
