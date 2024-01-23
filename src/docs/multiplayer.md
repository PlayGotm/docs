# Multiplayer

Your players can connect peer-to-peer to each other using `GotmMultiplayer` without port-forwarding.

Try it out!

<game>[](/gdgotm-examples/multiplayer)</game>

_Press "Host" to create a server. Join the game by pasting the invitation link into a new browser window and pressing "Join"._

[<button outlined>Browse demo source code</button>](https://github.com/PlayGotm/gdgotm-examples/tree/master/examples/multiplayer)

The above game example uses `GotmMultiplayer` to connect players in a shared drawing session.

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Host server

###### Create server

```gdscript
var peer: WebRTCMultiplayerPeer = await GotmMultiplayer.create_server()
multiplayer.multiplayer_peer = peer
multiplayer.peer_connected.connect(func(id): print("peer connected!"))
# Players can join using this address.
var address: String = await GotmMultiplayer.get_address()
```

# Join server

Clients can join the server using the host's virtual IP address. So the host needs to somehow send its address to the clients. A common way to do so is to use [lobbies](./lobby.md) or let players send it through their own social channels, such as Discord.

The [example above](#multiplayer) solved it by creating a link that players can use to join the host.

###### Create client

```gdscript
# The client needs to get this from the host somehow.
var address: String = ""
var peer: WebRTCMultiplayerPeer = await GotmMultiplayer.create_client(address)
multiplayer.multiplayer_peer = peer
multiplayer.connected_to_server.connect(func(): print("connected!"))
multiplayer.connection_failed.connect(func(): print("connection failed"))

await multiplayer.connected_to_server
peer.put_var("hello world!")
```
