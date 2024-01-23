# Lobbies

[GDGotm](/src/docs/gdgotm.md) gives you a complete solution for managing player-hosted lobbies in your Godot game. It lets your players discover existing servers in a public list.

Try it out!

<game>[](/gdgotm-examples/lobby)</game>
<game>[](/gdgotm-examples/lobby)</game>

_Host a lobby in one screen and refresh the list in the other screen. Click "Join" on the listed lobby to join it._

[<button outlined>Browse demo source code</button>](https://github.com/PlayGotm/gdgotm-examples/tree/master/examples/lobby)

The above game example uses `GotmLobby` and `GotmMultiplayer` to connect players in a shared drawing session.

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Host a lobby

###### Create lobby

```gdscript
# Create a lobby with a name.
var lobby_name = "My lobby"
var lobby: GotmLobby = await GotmLobby.create(lobby_name)
```

The lobby is only used to broadcast your address to other players. You still need to listen to incoming connections by [creating a server](./multiplayer.md#host-server).

The lobby contains your virtual IP address, which is used by other players to connect to your server

# Fetch lobbies

###### Get a lobby's address

```gdscript
var lobbies: Array = await GotmLobby.list()
if !lobbies:
    print("no lobbies found")
    return
var address: String = lobbies[0].address
```

Now that you have the host's address, you can connect by [creating a client](./multiplayer.md#create-client)
