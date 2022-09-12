# Lobbies

[GDGotm](/src/docs/gdgotm.md) gives you a complete solution for managing player-hosted lobbies in your Godot game. It lets your players host and join multiplayer sessions within your game without the need of an invitation link.

[Matchmaking](#matchmaking) - Match players based on their rank.

[Quick-join](#quick-join) - Let your players join a lobby in a single click without browsing or searching in a lobby browser.

[Global lobbies](#global-lobbies) - Give your players the illusion of always-running dedicated servers!

[Lobby browser](#lobby-browser) - Design your own lobby browser.

Try it out!

<game>[](/gdgotm-examples/lobbies)</game>
<game>[](/gdgotm-examples/lobbies)</game>

_Host a lobby in one screen and refresh the list in the other screen. Click "Join" on the listed lobby to join it._

[<button outlined>Download demo project</button>](https://github.com/PlayGotm/gdgotm-examples/archive/refs/heads/master.zip)
[<button outlined>Browse demo source code</button>](https://github.com/PlayGotm/gdgotm-examples)

The above game example uses GDGotm and [NetworkedMultiplayerENet](https://docs.godotengine.org/en/stable/classes/class_networkedmultiplayerenet.html) to connect players in a shared drawing session.

<include>

[](/src/utility/gdgotm-notice.md)

</include>

You do not need to run your game on Gotm to test lobbies on your local network.

# Host lobby

Call `Gotm.host_lobby` to host a lobby.

###### Suppress invitation link popup

```gdscript
# Suppress invitation popup with 'false'.
Gotm.host_lobby(false)
```

You can now change its name and other properties by accessing it at `Gotm.lobby`.

###### Set lobby name

```gdscript
Gotm.lobby.name = "My Lobby!"
```

Lobbies are hidden from fetch results by default. Make your lobby visible by setting its `hidden` property to `false`.

###### Make lobby visible to other players

```gdscript
Gotm.lobby.hidden = false
```

If your game uses networked multiplayer, you are now ready to run your game's server startup code.
Here is an example using NetworkedMultiplayerENet.

###### Create server

```gdscript
var peer = NetworkedMultiplayerENet.new()
peer.create_server(8070)
```

# Fetch lobbies

Before you can join the lobby you must fetch it by calling `GotmLobbyFetch.first`.
This call is asynchronous, which means that you must use [yield](https://docs.godotengine.org/en/stable/getting_started/scripting/gdscript/gdscript_basics.html?#coroutines-with-yield) to retrieve its return value.

###### Get lobbies with GotmLobbyFetch

```gdscript
var fetch = GotmLobbyFetch.new()
var lobbies = yield(fetch.first(), "completed")
```

If your lobby is not fetched, make sure you set its `hidden` property to `false` in the previous step.

# Join lobby

Using the lobbies fetched in the previous step, you can join the lobby using `GotmLobby.join`.
This call is asynchronous, which means that you must use [yield](https://docs.godotengine.org/en/stable/getting_started/scripting/gdscript/gdscript_basics.html?#coroutines-with-yield) to retrieve its return value.

###### Join a lobby

```gdscript
var lobby = lobbies[0]
var success = yield(lobby.join(), "completed")
```

You have now joined the lobby and are able to communicate with other peers in it.
If your game uses networked multiplayer, you are now ready to connect to the `GotmUser.address` of `GotmLobby.host`.
Here is an example using NetworkedMultiplayerENet.

###### Create client

```gdscript
var peer = NetworkedMultiplayerENet.new()
peer.create_client(Gotm.lobby.host.address, 8070)
```

# Browse lobbies

[GDGotm](/src/docs/gdgotm.md) gives you access to Gotm's powerful lobby system that lets you fetch lobbies with custom filters and sorting.
Try it out!

<game>[](/gdgotm-examples/lobbies-fetch)</game>

_You can filter and sort your lobbies any way you want with custom properties._

[<button outlined>Download demo project</button>](https://github.com/PlayGotm/gdgotm-examples/archive/refs/heads/master.zip)
[<button outlined>Browse demo source code</button>](https://github.com/PlayGotm/gdgotm-examples)

The above game example uses the [GDGotm](/src/docs/gdgotm.md) to fetch existing lobbies with custom properties.
It fetches lobbies depending on its map, difficulty and name.
It also sorts the results depending on the lobby's creation time or difficulty.

GDGotm provides you with the basic building blocks of a lobby system, which gives you much freedom in your own design of your game's lobby experience.

## Lobby browser

See the [example project](/gdgotm-examples/lobbies-fetch) for a live lobby browser example.

`GotmLobbyFetch` provides stateful pagination out-of-the-box. It can fetch lobbies page-by-page both forwards and backwards.

###### Get lobbies page by page

```gdscript
var fetch = GotmLobbyFetch.new()
var lobbies

# Get the next 5 lobbies...
lobbies = yield(fetch.next(5), "completed")

# And the next...
lobbies = yield(fetch.next(5), "completed")

# And so on...
```

## Quick-join

Let your players join a lobby without browsing or searching in a lobby browser.
With quick-joining you could present your players with a simple "Play" button that automatically joins the lobby with most players.

###### Join most popular lobby

```gdscript
func quick_join():
    var fetch = GotmLobbyFetch.new()
    fetch.sort_property = "player_count"
    fetch.sort_ascending = false

    var lobbies = yield(fetch.first(1), "completed")
    var success = yield(lobbies[0].join(), "completed")
```

## Global lobbies

Give your players the illusion of always-running dedicated servers!

With global lobbies you gather your players in a limited set of lobbies with preset modes, such as `deathmatch`, `free for all` and `king of the hill`.
You then present your players with a button for each mode. When pressed, join the matching lobby or host one if it doesn't exist.

###### Join or create a global lobby

```gdscript
func join_global_lobby(mode):
    var fetch = GotmLobbyFetch.new()
    fetch.filter_properties.mode = mode

    var lobbies = yield(fetch.first(1), "completed")
    if not lobbies.empty():
        var success = yield(lobbies[0].join(), "completed")
    else:
        Gotm.host_lobby()
        Gotm.lobby.mode = mode
        Gotm.lobby.hidden = false
```

## Matchmaking

Match players based on their rank by limiting the fetched lobbies to a rank range using `GotmLobbyFetch.sort_min` and `GotmLobbyFetch.sort_max`.

Keep widening the range until a match is found.

###### Find a lobby within a rank threshold

```gdscript
func do_matchmaking(rank):
    var fetch = GotmLobbyFetch.new()
    fetch.sort_property = "rank"
    fetch.sort_min = rank
    fetch.sort_max = rank

    while true:
        var lobbies = yield(fetch.first(1), "completed")
        if not lobbies.empty():
            var success = yield(lobbies[0].join(), "completed")
            return
        else:
            fetch.sort_min -= 1
            fetch.sort_max += 1
```

## Create test lobby

To try out Gotm's lobby-fetching system you first need lobbies to fetch.
GDGotm lets you create test lobbies that behave as real lobbies.
This makes it quick and easy to try out the system.

Call `GotmDebug.add_lobby` to host a lobby without joining it.

###### Create a test lobby

```gdscript
var lobby = GotmDebug.add_lobby()
lobby.name = "My Test Lobby!"
```

Lobbies are hidden from fetch results by default. Make the lobby visible by setting its `hidden` property to `false`.

###### Make test lobby visible

```gdscript
lobby.hidden = false
```

You can now fetch it as if it was a real lobby.

###### Get test lobby

```gdscript
var fetch = GotmLobbyFetch.new()
var lobbies = yield(fetch.first(), "completed")
```

## Set custom properties

You can attach your own metadata to a lobby as custom properties with `GotmLobby.set_property`.

###### Set custom properties on lobby

```gdscript
lobby.set_property("difficulty", "easy")
lobby.set_property("map", "classic")
```

You can now fetch the lobby and see its custom properties by calling `GotmLobby.get_property`.

###### Get custom properties of a lobby

```gdscript
var difficulty = lobby.get_property("difficulty")
var map = lobby.get_property("classic")
```

## Filter

To only fetch lobbies with a particular difficulty or map, you can make them filterable by calling `GotmLobby.set_filterable`.

###### Make custom properties filterable

```gdscript
lobby.set_filterable("difficulty")
lobby.set_filterable("map")
```

Now you can filter lobbies with `GotmLobbyFetch.filter_properties`.
For example, fetch lobbies with difficulty `easy` and map `classic` with the following code:

###### Set filters when getting lobbies

```gdscript
fetch.filter_properties.difficulty = "easy"
fetch.filter_properties.map = "classic"
```

To fetch lobbies with any difficulty you can set its value to `null`.

###### Clear a filter when getting lobbies

```gdscript
fetch.filter_properties.difficulty = null
fetch.filter_properties.map = "classic"
```

When using `GotmLobbyFetch.filter_properties` you must set every value that was registered with `GotmLobby.set_filterable`.
For example, the following will fail to fetch our lobby because it does not set `map` to any value.

###### Set uncomplete filters when getting lobbies

```gdscript
fetch.filter_properties.difficulty = null
```

Always make sure you are setting all properties.

Disable filtering by setting all filter properties to `null`.

###### Disable filters when getting lobbies

```gdscript
fetch.filter_properties.difficulty = null
fetch.filter_properties.map = null
```

## Sort

By default lobbies are sorted by their creation time in descending order, so newer lobbies appear before older lobbies.

Make your lobby sortable by difficulty with `GotmLobby.set_sortable`.

###### Make custom property sortable

```gdscript
lobby.set_sortable("difficulty")
```

Now you can sort lobbies by setting `GotmLobbyFetch.sort_property`.
For example, the following sort lobbies by difficulty with the following code:

###### Set sort property when getting lobbies

```gdscript
fetch.sort_property = "difficulty"
```

When using `GotmLobbyFetch.sort_property` only lobbies that have registered that property with `GotmLobby.set_sortable` will be fetched.
For example, the following will fail to fetch our lobby because `map` is not registered as sortable.

###### Set invalid sort property when getting lobbies

```gdscript
fetch.sort_property = "map"
```

Disable sorting by setting it to `""`.

###### Disable sorting when getting lobbies

```gdscript
fetch.sort_property = ""
```

## Search by name

You can search lobbies by name with `GotmLobbyFetch.filter_name`.

###### Set name search when getting lobbies

```gdscript
fetch.filter_name = "My Test Lobby!"
```

This searches for all lobbies whose names contains `My Test Lobby!`.
Since the search is tolerant, these example values would successfully fetch our lobby too:

###### Test the error-tolerance of the name search

```gdscript
fetch.filter_name = "my test lobby"
fetch.filter_name = "tEsT"
fetch.filter_name = "MY!!!     "
```

Disable name-searching by setting it to `""`.

###### Disable name search when getting lobbies

```gdscript
fetch.filter_name = ""
```

## Paginate

You can fetch lobbies page-by-page by using `GotmLobbyFetch.next`.

###### Get lobbies page by page

```gdscript
var lobbies

# Get the next 5 lobbies...
lobbies = yield(fetch.next(5), "completed")

# And the next...
lobbies = yield(fetch.next(5), "completed")

# And so on...
```

You can refresh your current page with `GotmLobbyFetch.current`.

###### Refresh a page of lobbies

```gdscript
lobbies = yield(fetch.current(5), "completed")
```

Modifying any property in `GotmLobbyFetch` will reset its state to the first page.

###### Reset fetch state

```gdscript
# Page 1
lobbies = yield(fetch.first(5), "completed")

# Page 2
lobbies = yield(fetch.next(5), "completed")

# Modify state
fetch.name = "abc"

# Page 1
lobbies = yield(fetch.next(5), "completed")
```

# Limitations

Gotm provides you the basic building blocks of a lobby system, which gives you much freedom in your own design of your game's lobby experience.
However, there are some restrictions:

- Custom properties:
  - Must be of types [String](https://docs.godotengine.org/en/stable/classes/class_string.html#class-string), [int](https://docs.godotengine.org/en/stable/classes/class_int.html#class-int), [float](https://docs.godotengine.org/en/stable/classes/class_float.html#class-float) or [bool](https://docs.godotengine.org/en/stable/classes/class_bool.html#class-bool).
  - Strings and property names longer than 64 characters are truncated.
  - A lobby can have up to 10 custom properties.
  - A lobby can have up to 3 filterable custom properties.
  - A lobby can have up to 3 sortable custom properties.
- Up to 8 lobbies can be fetched at a time.
