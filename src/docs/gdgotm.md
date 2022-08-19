### Gotm Documentation

Learn how to enhance your game with Gotm's Godot-optimized APIs.

#### Networked multiplayer

On Gotm your players can connect without plugins or port-forwarding.

<a href="/docs/networked-multiplayer">
  <OutlinedButton padding="10px 15px" margin="10px">
    Get started
  </OutlinedButton>
</a>
<br />

#### Lobbies

Connect players without an invitation URL.

<a href="/docs/lobbies">
  <OutlinedButton padding="10px 15px" margin="10px">
    Get started
  </OutlinedButton>
</a>
<br />

#### Fetch lobbies

Access Gotm's powerful lobby system that lets you fetch lobbies with custom filters and sorting.

[Matchmaking](/docs/fetch-lobbies#matchmaking) - Match players based on their rank.

[Quick-join](/docs/fetch-lobbies#quick-join) - Let your players join a lobby in a single click without browsing or searching in a lobby browser.

[Global lobbies](/docs/fetch-lobbies#global-lobbies) - Give your players the illusion of always-running dedicated servers!

[Lobby browser](/docs/fetch-lobbies#lobby-browser) - Design your own lobby browser.

<a href="/docs/fetch-lobbies">
  <OutlinedButton padding="10px 15px" margin="10px">
    Get started
  </OutlinedButton>
</a>
<br />

#### Leaderboards

Let your players compete against eachother or themselves.

<a href="/docs/leaderboards">
  <OutlinedButton padding="10px 15px" margin="10px">
    Get started
  </OutlinedButton>
</a>
<br />

### API reference

API reference for the GDScript plugin.

<ExternalLink href="https://github.com/PlayGotm/GDGotm">
  <OutlinedButton padding="10px" margin="10px">
    <FiCode size="24px" />
    <span>Browse source</span>
  </OutlinedButton>
</ExternalLink>

#### Gotm

##### signal lobby_changed()

You connected or disconnected from a lobby. Access it at 'Gotm.lobby'

##### signal files_dropped(files: Array, screen: int)

Files were drag'n'dropped into the screen.
The 'files' argument is an array of 'GotmFile'.

##### var user: GotmUser

Player information.

<Anchor id="gotm-lobby"></Anchor>

##### var lobby: GotmLobby

Current lobby you are in.
Is null when not in a lobby.

##### func is_live() -> bool

The API is live when the game runs on gotm.io.
Running the game in the web player (gotm.io/web-player) also counts as live.

<Anchor id="gotm-host-lobby"></Anchor>

##### static func host_lobby(show_invitation: bool = true) -> GotmLobby

Create a new lobby and join it.

If 'show_invitation' is true, show an invitation link in a popup.

By default, the lobby is hidden and is only accessible directly through
its 'invite_link'.

Set 'lobby.hidden' to false to make it fetchable with 'GotmLobbyFetch'.

Returns the hosted lobby (also accessible at 'Gotm.lobby').

##### func text_to_speech(message: String, language: String = "en-US") -> bool

Play an audio snippet with 'message' as a synthesized voice.

'language' is in BCP 47 format (e.g. "en-US" for american english).

If specified language is not available "en-US" is used.

Return true if playback succeeded.

##### func pick_files(types: Array = Array(), only_one: bool = false) -> Array

Asynchronously open up the browser's file picker.

If 'types' is specified, limit the file picker to files with matching [file
types](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/file#Unique_file_type_specifiers).
If 'only_one' is true, only allow the user to pick one file.

If a picking-session is already in progress, an empty
array is asynchronously returned.

Asynchronously return an array of 'GotmFile'.
Use 'yield(pick_files(), "completed")' to retrieve the return value.

#### GotmDebug

Helper library for testing against the API locally, as if it would be live.

These functions do not make real API calls. They fake operations and
trigger relevant signals as if they happened live.

These functions do nothing when the game is running live on gotm.io.
Running the game in the web player (gotm.io/web-player) also counts as live.

<Anchor id="gotmdebug-add-lobby"></Anchor>

##### static func add_lobby() -> GotmLobby

Host a lobby without joining it.

Note that the lobby is hidden by default and not fetchable with
'GotmLobbyFetch'. To make it fetchable, set 'hidden' to false.

The lobby is only fetchable and joinable in this game process.

Returns added lobby.

##### static func remove_lobby(lobby: GotmLobby) -> void

Remove a lobby created with 'add_lobby', as if its host (you) disconnected from it.
Triggers 'lobby_changed' if you are in that lobby.

##### static func clear_lobbies() -> void

Remove all lobbies.

##### static func add_lobby_peer(lobby: GotmLobby) -> GotmUser

Add yourself to the lobby, without joining it.
Triggers 'peer_joined' if you are in that lobby.
Returns joined peer.

##### static func remove_lobby_peer(lobby: GotmLobby, peer: GotmUser) -> void

Remove a peer created with 'add_lobby_peer' from the lobby, as if the peer (you) disconnected
from the lobby.
Triggers 'peer_left' if you are in that lobby.

#### GotmFile

A simple in-memory file descriptor used by 'Gotm.pick_files' and
'Gotm.files_dropped'.

##### var name: String

File name.

##### var data: PoolByteArray

File data.

##### var modified_time: int

Last time the file was modified in unix time (seconds since epoch).

##### func download() -> void

Save the file to the browser's download folder.

#### GotmLobby

A lobby is a way of connecting players with eachother as if they
were on the same local network.

Lobbies can be joined either directly through an 'invite_link', or by
joining lobbies fetched with the 'GotmLobbyFetch' class.

##### signal peer_joined(peer_user)

Peer joined the lobby.

'peer_user' is a 'GotmUser' instance.

This is only emitted if you are in this lobby.

##### signal peer_left(peer_user)

Peer left the lobby.

'peer_user' is a 'GotmUser' instance.

This is only emitted if you are in this lobby.

##### var id: String

Globally unique identifier.

##### var peers: Array = []

Other peers in the lobby with addresses.
Is an array of 'GotmUser'.

##### var me: GotmUser = GotmUser.new()

You with address.

<Anchor id="gotmlobby-host"></Anchor>

##### var host: GotmUser = GotmUser.new()

Host user with address.

##### var invite_link: String

Peers can join the lobby directly through this link.

##### var name: String = ""

Name that is searchable using 'GotmLobbyFetch'
Names longer than 64 characters are truncated.

<Anchor id="gotmlobby-hidden"></Anchor>

##### var hidden: bool = true

Prevent the lobby from showing up in fetches?
Peers may still join directly through 'invite_link'

##### var locked: bool = false

Prevent new peers from joining?
Also prevents the lobby from showing up in fetches.

<Anchor id="gotmlobby-join"></Anchor>

##### func join() -> bool:

Asynchronously join this lobby after leaving current lobby.

Use 'var success = yield(lobby.join(), "completed")' to wait for the call to complete
and retrieve the return value.

Sets 'Gotm.lobby' to the joined lobby if successful.

Asyncronously returns true if successful, else false.

##### func leave() -> void:

Leave this lobby.

##### func is_host() -> bool:

Am I the host of this lobby?

<Anchor id="gotmlobby-get-property"></Anchor>

##### func get_property(name: String):

Get a custom property.

##### func kick(peer: GotmUser) -> bool:

Kick peer from this lobby.
Returns true if successful, else false.

<Anchor id="gotmlobby-set-property"></Anchor>

##### func set_property(name: String, value) -> void:

Store up to 10 of your own custom properties in the lobby.
These are visible to other peers when fetching lobbies.
Only properties of types String, int, float or bool are allowed.
Integers are converted to floats.
Strings longer than 64 characters are truncated.
Setting 'value' to null removes the property.

<Anchor id="gotmlobby-set-filterable"></Anchor>

##### func set_filterable(property_name: String, filterable: bool = true) -> void:

Make this lobby filterable by a custom property.
Filtering is done when fetching lobbies with 'GotmLobbyFetch'.
Up to 3 properties can be set as filterable at once.

<Anchor id="gotmlobby-set-sortable"></Anchor>

##### func set_sortable(property_name: String, sortable: bool = true) -> void:

Make this lobby sortable by a custom property.
Sorting is done when fetching lobbies with 'GotmLobbyFetch'.
Up to 3 properties can be set as sortable at once.

<Anchor id="gotmlobbyfetch"></Anchor>

#### GotmLobbyFetch

Used for fetching non-hidden and non-locked lobbies.

All fetch methods asynchronously fetch up to 8 non-hidden
and non-locked lobbies.

Modifying any filtering or sorting option resets the state of this
'GotmLobbyFetch' instance and causes the next fetch call to
fetch the first lobbies.

All fetch methods asynchronously return an array of fetched lobbies.
Use 'yield(fetch.next(), "completed")' to retrieve it.

<Anchor id="gotmlobbyfetch-filter-name"></Anchor>

##### var filter_name: String = ""

If empty, sort by the lobby's creation time.

If not empty, fetch lobbies whose 'Lobby.name' contains 'name'.

<Anchor id="gotmlobbyfetch-filter-properties"></Anchor>

##### var filter_properties: Dictionary = {}

If not empty, fetch lobbies whose filterable custom properties
matches those in 'filter_properties'.

For example, setting 'filter_properties.difficulty = 2' will
only fetch lobbies that have been set up with both 'lobby.set_property("difficulty", 2)'
and 'lobby.set_filterable("difficulty", true)'.

If your lobby has multiple filterable props, you must provide every filterable
prop in 'filter_properties'. Setting a prop's value to 'null' will match any
value of that prop.

<Anchor id="gotmlobbyfetch-sort-property"></Anchor>

##### var sort_property: String = ""

If not empty, sort by a sortable custom property.

For example, setting 'sort_property = "difficulty"' will
only fetch lobbies that have been set up with both 'lobby.set_property("difficulty", some_value)'
and 'lobby.set_sortable("difficulty", true)'.

If your lobby has a sortable prop, you must always provide a 'sort_property'.

##### var sort_ascending: bool = false

Sort results in ascending order?

<Anchor id="gotmlobbyfetch-sort-min"></Anchor>

##### var sort_min = null

If not null, fetch lobbies whose sort property's value is equal to or greater than 'sort_min'.

<Anchor id="gotmlobbyfetch-sort-max"></Anchor>

##### var sort_max = null

If not null, fetch lobbies whose sort property's value is equal to or lesser than 'sort_max'.

##### var sort_min_exclusive = false

If true, and 'sort_min' is provided, exclude lobbies whose sort property's value is equal to 'sort_min'.

##### var sort_max_exclusive = false

If true, and 'sort_max' is provided, exclude lobbies whose sort property's value is equal to 'sort_max'.

<Anchor id="gotmlobbyfetch-next"></Anchor>

##### func next(count: int = 8) -> Array:

Fetch the next lobbies, starting after the last lobby fetched
in the previous call.

<Anchor id="gotmlobbyfetch-previous"></Anchor>

##### func previous(count: int = 8) -> Array:

Fetch the previous lobbies, ending before the first lobby
that was fetched in the previous call.

<Anchor id="gotmlobbyfetch-first"></Anchor>

##### func first(count: int = 8) -> Array:

Fetch the first lobbies.

<Anchor id="gotmlobbyfetch-current"></Anchor>

##### func current(count: int = 8) -> Array:

Fetch lobbies at the current position.
Useful for refreshing lobbies without changing the page.

#### GotmUser

Holds information about a Gotm user.

<Anchor id="gotmuser-address"></Anchor>

##### var address: String = ""

The IP address of the user.
Is empty if you are not in the same lobby.
