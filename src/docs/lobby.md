### Lobbies

The [GDScript plugin](/about/plugin) gives you a complete solution for managing player-hosted lobbies.
It lets your players host and join multiplayer sessions within your game without the need of an invitation link.
Try it out!

<HorizontalList>
  <Game src="game-examples/lobbies" />
  <Game src="game-examples/lobbies" />
</HorizontalList>

_Host a lobby in one screen and refresh the list in the other screen. Click "Join" on the listed lobby to join it._

<HorizontalList>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/releases/latest/download/Lobbies.-.Host.and.join.zip">
    <OutlinedButton padding="10px" margin="10px">
      <FiDownload size="24px" />
      <span>Download project</span>
    </OutlinedButton>
  </ExternalLink>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/tree/master/Lobbies%20-%20Host%20and%20join">
    <OutlinedButton padding="10px" margin="10px">
      <FiCode size="24px" />
      <span>Browse source</span>
    </OutlinedButton>
  </ExternalLink>
</HorizontalList>

The above game example uses the GDScript plugin and [NetworkedMultiplayerENet](https://docs.godotengine.org/en/stable/classes/class_networkedmultiplayerenet.html) to connect players in a shared drawing session.
See the [source code](https://github.com/PlayGotm/Game-Examples/tree/master/Lobbies%20-%20Host%20and%20join) for more details.

<Anchor id="get-started"></Anchor>

#### Get started

Install the plugin from the [AssetLib](https://docs.godotengine.org/en/stable/tutorials/assetlib/using_assetlib.html#in-the-editor) in the Godot Editor and follow its installation instructions.
The [example project](https://github.com/PlayGotm/Game-Examples/releases/latest/download/Lobbies.-.Host.and.join.zip) already comes with the plugin installed.

You do not need to run your game on Gotm to test the plugin on your local network.
To test it across networks, use the [web player](/web-player).

<Anchor id="1-host-lobby"></Anchor>

##### 1. Host lobby

Call [Gotm.host_lobby](/docs/api-reference#gotm-host-lobby) to host a lobby.

```
### Suppress invitation popup with 'false'.
Gotm.host_lobby(false)
```

You can now change its name and other properties by accessing it at [Gotm.lobby](/docs/api-reference#gotm-lobby).

```
Gotm.lobby.name = "My Lobby!"
```

Lobbies are hidden from fetch results by default. Make your lobby visible by setting its [hidden](/docs/api-reference#gotmlobby-hidden) property to `false`.

```
Gotm.lobby.hidden = false
```

If your game uses networked multiplayer, you are now ready to run your game's server startup code.
Here is an example using NetworkedMultiplayerENet.

```
var peer = NetworkedMultiplayerENet.new()
peer.create_server(8070)
```

<Anchor id="2-fetch-lobbies"></Anchor>

##### 2. Fetch lobbies

Before you can join the lobby you must fetch it by calling [GotmLobbyFetch.first](/docs/api-reference#gotmlobbyfetch-first).
This call is asynchronous, which means that you must use [yield](https://docs.godotengine.org/en/stable/getting_started/scripting/gdscript/gdscript_basics.html?#coroutines-with-yield) to retrieve its return value.

```
var fetch = GotmLobbyFetch.new()
var lobbies = yield(fetch.first(), "completed")
```

If your lobby is not fetched, make sure you set its [hidden](/docs/api-reference#gotmlobby-hidden) property to `false` in the previous step.

<Anchor id="3-join-lobby"></Anchor>

##### 3. Join lobby

Using the lobbies fetched in the previous step, you can join the lobby using [GotmLobby.join](/docs/api-reference#gotmlobby-join).
This call is asynchronous, which means that you must use [yield](https://docs.godotengine.org/en/stable/getting_started/scripting/gdscript/gdscript_basics.html?#coroutines-with-yield) to retrieve its return value.

```
var lobby = lobbies[0]
var success = yield(lobby.join(), "completed")
```

You have now joined the lobby and are able to communicate with other peers in it.
If your game uses networked multiplayer, you are now ready to connect to the [GotmUser.address](/docs/api-reference#gotmuser-address) of [GotmLobby.host](/docs/api-reference#gotmlobby-host).
Here is an example using NetworkedMultiplayerENet.

```
var peer = NetworkedMultiplayerENet.new()
peer.create_client(Gotm.lobby.host.address, 8070)
```

### Fetch lobbies

The [GDScript plugin](/about/plugin) gives you access to Gotm's powerful lobby system that lets you fetch lobbies with custom filters and sorting.
Try it out!

<Game src="game-examples/lobbies-fetch" />

_You can filter and sort your lobbies any way you want with custom properties._

<HorizontalList>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/releases/latest/download/Lobbies.-.Fetch.zip">
    <OutlinedButton padding="10px" margin="10px">
      <FiDownload size="24px" />
      <span>Download project</span>
    </OutlinedButton>
  </ExternalLink>
  <ExternalLink href="https://github.com/PlayGotm/Game-Examples/tree/master/Lobbies%20-%20Fetch">
    <OutlinedButton padding="10px" margin="10px">
      <FiCode size="24px" />
      <span>Browse source</span>
    </OutlinedButton>
  </ExternalLink>
</HorizontalList>

The above game example uses the GDScript plugin to fetch existing lobbies with custom properties.
It fetches lobbies depending on its map, difficulty and name.
It also sorts the results depending on the lobby's creation time or difficulty.
See the [source code](https://github.com/PlayGotm/Game-Examples/tree/master/Lobbies%20-%20Fetch) for more details.

<Anchor id="use-cases"></Anchor>

#### Use cases

Gotm's GDScript plugin provides you the basic building blocks of a lobby system, which gives you much freedom in your own design of your game's lobby experience.
Install the plugin directly from the [AssetLib](https://docs.godotengine.org/en/stable/tutorials/assetlib/using_assetlib.html#in-the-editor) in the Godot Editor.

Explore the use cases below to find some inspiration for your game's lobby system.

<Anchor id="lobby-browser"></Anchor>

##### Lobby browser

See the [example project](/game-examples/lobbies-fetch) for a live lobby browser example.

[GotmLobbyFetch](/docs/api-reference#gotmlobbyfetch) provides stateful pagination out-of-the-box. It can fetch lobbies page-by-page both forwards and backwards.

```
var fetch = GotmLobbyFetch.new()
var lobbies

### Get the next 5 lobbies...
lobbies = yield(fetch.next(5), "completed")

### And the next...
lobbies = yield(fetch.next(5), "completed")

### And so on...
```

<Anchor id="quick-join"></Anchor>

##### Quick-join

Let your players join a lobby without browsing or searching in a lobby browser.
With quick-joining you could present your players with a simple "Play" button that automatically joins the lobby with most players.

```
func quick_join():
  var fetch = GotmLobbyFetch.new()
  fetch.sort_property = "player_count"
  fetch.sort_ascending = false

  var lobbies = yield(fetch.first(1), "completed")
  var success = yield(lobbies[0].join(), "completed")
```

<Anchor id="global-lobbies"></Anchor>

##### Global lobbies

Give your players the illusion of always-running dedicated servers!

With global lobbies you gather your players in a limited set of lobbies with preset modes, such as `deathmatch`, `free for all` and `king of the hill`.
You then present your players with a button for each mode. When pressed, join the matching lobby or host one if it doesn't exist.

```
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

<Anchor id="matchmaking"></Anchor>

##### Matchmaking

Match players based on their rank by limiting the fetched lobbies to a rank range using [GotmLobbyFetch.sort_min](/docs/api-reference#gotmlobbyfetch-sort-min) and [GotmLobbyFetch.sort_max](/docs/api-reference#gotmlobbyfetch-sort-max).

Keep widening the range until a match is found.

```
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

<Anchor id="get-started"></Anchor>

#### Get started

Install the plugin from the [AssetLib](https://docs.godotengine.org/en/stable/tutorials/assetlib/using_assetlib.html#in-the-editor) in the Godot Editor and follow its installation instructions.
The [example project](https://github.com/PlayGotm/Game-Examples/releases/latest/download/Lobbies.-.Fetch.zip) already comes with the plugin installed.

This example uses [GotmDebug.add_lobby](/docs/api-reference#gotmdebug-add-lobby) to add test lobbies.
Test lobbies do not appear when you run your game on Gotm.

<Anchor id="1-create-test-lobby"></Anchor>

##### 1. Create test lobby

To try out Gotm's lobby-fetching system you first need lobbies to fetch.
The plugin lets you create test lobbies that behave as real lobbies.
This makes it quick and easy to try out the system.

Call [GotmDebug.add_lobby](/docs/api-reference#gotmdebug-add-lobby) to host a lobby without joining it.

```
var lobby = GotmDebug.add_lobby()
lobby.name = "My Test Lobby!"
```

Lobbies are hidden from fetch results by default. Make the lobby visible by setting its [hidden](/docs/api-reference#gotmlobby-hidden) property to `false`.

```
lobby.hidden = false
```

You can now fetch it as if it was a real lobby.

```
var fetch = GotmLobbyFetch.new()
var lobbies = yield(fetch.first(), "completed")
```

<Anchor id="2-set-custom-properties"></Anchor>

##### 2. Set custom properties

You can attach your own metadata to a lobby as custom properties with [GotmLobby.set_property](/docs/api-reference#gotmlobby-set-property).

```
lobby.set_property("difficulty", "easy")
lobby.set_property("map", "classic")
```

You can now fetch the lobby and see its custom properties by calling [GotmLobby.get_property](/docs/api-reference#gotmlobby-get-property).

```
var difficulty = lobby.get_property("difficulty")
var map = lobby.get_property("classic")
```

<Anchor id="3-filter"></Anchor>

##### 3. Filter

To only fetch lobbies with a particular difficulty or map, you can make them filterable by calling [GotmLobby.set_filterable](/docs/api-reference#gotmlobby-set-filterable).

```
lobby.set_filterable("difficulty")
lobby.set_filterable("map")
```

Now you can filter lobbies with [GotmLobbyFetch.filter_properties](/docs/api-reference#gotmlobbyfetch-filter-properties).
For example, fetch lobbies with difficulty `easy` and map `classic` with the following code:

```
fetch.filter_properties.difficulty = "easy"
fetch.filter_properties.map = "classic"
```

To fetch lobbies with any difficulty you can set its value to `null`.

```
fetch.filter_properties.difficulty = null
fetch.filter_properties.map = "classic"
```

When using [GotmLobbyFetch.filter_properties](/docs/api-reference#gotmlobbyfetch-filter-properties) you must set every value that was registered with [GotmLobby.set_filterable](/docs/api-reference#gotmlobby-set-filterable).
For example, the following will fail to fetch our lobby because it does not set `map` to any value.

```
fetch.filter_properties.difficulty = null
```

Always make sure you are setting all properties.

Disable filtering by setting all filter properties to `null`.

```
fetch.filter_properties.difficulty = null
fetch.filter_properties.map = null
```

<Anchor id="4-sort"></Anchor>

##### 4. Sort

By default lobbies are sorted by their creation time in descending order, so newer lobbies appear before older lobbies.

Make your lobby sortable by difficulty with [GotmLobby.set_sortable](/docs/api-reference#gotmlobby-set-sortable).

```
lobby.set_sortable("difficulty")
```

Now you can sort lobbies by setting [GotmLobbyFetch.sort_property](/docs/api-reference#gotmlobbyfetch-sort-property).
For example, the following sort lobbies by difficulty with the following code:

```
fetch.sort_property = "difficulty"
```

When using [GotmLobbyFetch.sort_property](/docs/api-reference#gotmlobbyfetch-sort-property) only lobbies that have registered that property with [GotmLobby.set_sortable](/docs/api-reference#gotmlobby-set-sortable) will be fetched.
For example, the following will fail to fetch our lobby because `map` is not registered as sortable.

```
fetch.sort_property = "map"
```

Disable sorting by setting it to `""`.

```
fetch.sort_property = ""
```

<Anchor id="5-search-by-name"></Anchor>

##### 5. Search by name

You can search lobbies by name with [GotmLobbyFetch.filter_name](/docs/api-reference#gotmlobbyfetch-filter-name).

```
fetch.filter_name = "My Test Lobby!"
```

This searches for all lobbies whose names contains `My Test Lobby!`.
Since the search is tolerant, these example values would successfully fetch our lobby too:

```
fetch.filter_name = "my test lobby"
fetch.filter_name = "tEsT"
fetch.filter_name = "MY!!!     "
```

Disable name-searching by setting it to `""`.

```
fetch.filter_name = ""
```

<Anchor id="6-paginate"></Anchor>

##### 6. Paginate

You can fetch lobbies page-by-page by using [GotmLobbyFetch.next](/docs/api-reference#gotmlobbyfetch-next).

```
var lobbies

### Get the next 5 lobbies...
lobbies = yield(fetch.next(5), "completed")

### And the next...
lobbies = yield(fetch.next(5), "completed")

### And so on...
```

You can refresh your current page with [GotmLobbyFetch.current](/docs/api-reference#gotmlobbyfetch-current).

```
lobbies = yield(fetch.current(5), "completed")
```

Modifying any property in [GotmLobbyFetch](/docs/api-reference#gotmlobbyfetch) will reset its state to the first page.

```
### Page 1
lobbies = yield(fetch.first(5), "completed")

### Page 2
lobbies = yield(fetch.next(5), "completed")

### Modify state
fetch.name = "abc"

### Page 1
lobbies = yield(fetch.next(5), "completed")
```

<Anchor id="limitations"></Anchor>

#### Limitations

Gotm provides you the basic building blocks of a lobby system, which gives you much freedom in your own design of your game's lobby experience.
However, there are some restrictions:

- Custom properties:
  - Must be of types [String](https://docs.godotengine.org/en/stable/classes/class_string.html#class-string), [int](https://docs.godotengine.org/en/stable/classes/class_int.html#class-int), [float](https://docs.godotengine.org/en/stable/classes/class_float.html#class-float) or [bool](https://docs.godotengine.org/en/stable/classes/class_bool.html#class-bool).
  - Strings and property names longer than 64 characters are truncated.
  - A lobby can have up to 10 custom properties.
  - A lobby can have up to 3 filterable custom properties.
  - A lobby can have up to 3 sortable custom properties.
- Up to 8 lobbies can be fetched at a time.
