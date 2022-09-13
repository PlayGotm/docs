
# Game Saves
Using [GotmContent](https://gotm.io/docs/content) we can easily create Game Saves for players, which is useful as players probably would not like to lose their data.

## Save a game 

We could do this by creating a Dictionary which stores all information and saving this dictionary to [GotmContent](https://gotm.io/docs/content).

```gdscript
#Create a new dictionary for storing player datas
var player_data := {}
#Store player's position into player_data
player_data["player_position"] = player.position
#Store the level of player
player_data["player_lvl"] = player.level
#Save player_data into GotmContent
var content = yield(GotmContent.create(player_data), "completed")
```
Now we have stored it in GotmContent, its time to load it!

## Load a game

We need to load player"s data for the game to pick up where the player left.

```gdscript
#Load player_data using content     
var player_data = yield(GotmContent.get_variant(content), "completed")
#Set player's position to the value in player_data
player.position = player_data["player_position"]
#Set player's level to the value in player_data
player.level = player_data["player_lvl"]
```

## Delete a game
After players complete the game, they might want to delete the save file and beat the game again,
so it would be great to let them delete a save file. here's how to do it:

```gdscript
#Set player_data to an empty dictionary
var player_data = {}
#Update player data
yield(GotmContent.update(content, player_data), "completed")
```
