# xnet

A typed networking module for Roblox. Requires the new Luau type solver.

- [Get it on the Creator Store](https://create.roblox.com/store/asset/70549339899196/xnet)
- [Documentation](DOCS_URL)

## Install

Insert xnet from the Toolbox and move it to `ReplicatedStorage`.

## Example

Define your channels in one shared module:

```lua
-- ReplicatedStorage.Net
local xnet = require(game.ReplicatedStorage.xnet)

return xnet:Namespace({
	Chat = xnet:Channel("Across", xnet.string({ maxLength = 200 })),
	GetCoins = xnet:Channel("Across", xnet.nothing(), xnet.integer({ min = 0 })),
})
```

Server:

```lua
local Net = require(game.ReplicatedStorage.Net)

Net.Chat:ConnectFromClient(function(player, text)
	Net.Chat:SendToAllClients(`{player.Name}: {text}`)
end)

Net.GetCoins:ConnectFromClient(function(player)
	return coins[player] or 0
end)
```

Client:

```lua
local Net = require(game.ReplicatedStorage.Net)

Net.Chat:ConnectFromServer(print)
Net.Chat:SendToServer("hello")

local amount = Net.GetCoins:SendToServer()
```

## Features

- Handlers and return values are typed from the schema.
- Incoming data is checked on arrival. Bad values are cleaned or dropped.
- Events and requests use the same API. Add an output schema to make a request.
- Per-player rate limits, middleware, timeouts and unreliable channels.
- Remote names are obfuscated by default.

## Schemas

| Kind | Schemas |
|---|---|
| Values | `bool` `number` `integer` `range` `string` `buffer` `vector2` `vector3` `cframe` `color3` `numberRange` `numberSequence` `ray` `dateTime` `player` `character` `instance` `any` `nothing` |
| Choices | `literal` `enum` `robloxEnum` `union` |
| Collections | `struct` `array` `tuple` `map` `set` |
| Modifiers | `optional` `default` `refine` `transform` |

See the [documentation](DOCS_URL) for every method, option and setting.

## Author

[@xerseer](https://www.roblox.com/users/3081378486/profile)
