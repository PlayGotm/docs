# Player-generated Godot maps or levels

Enabling players to create their own custom maps or levels is an effective way to foster a vibrant and creative community in a game. Dota 2 and Counter-Strike are today very popular games that started off as custom maps within another game.

Custom maps can also give players the creative freedom to create their own gameplay mechanics and subgenres within a game. For example, in "surf maps" in Counter-Strike, players focus on surfing on walls through an obstacle course instead of shooting people. Surf maps have even spawned subgenres within itself, with some maps focusing on speedsurfing and others on tricksurfing. The surf mechanic is originally an exploitation of a physics bug.

Before a player can create a custom map, the game must feature a map editor. Luckily, making a Godot map editor is relatively easy thanks to Godot's node structure.

After a player has created a custom map or level in the game they can share it with other players or save it locally to their device by storing it in a [Gotm content](../content.md).

This example shows how to make custom Godot maps savable and sharable. In this example a player has already created their custom map, which is just a Godot node.

<include subject="map">

[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="favorites" scenario="If a player has a list of favorite custom maps, or maps that they want to play later">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

</include>

## Share maps with friends using memorable custom keys

A player can easily send their custom map to their friends by choosing a memorable key for their map instead of using the content's automatically generated identifier.

```gdscript
# Save our map with a key
var key = "awesome_map_by_sam"
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create(custom_map, key), "completed")
# Load our map with the same key
var shared_map = yield(GotmContent.get_node_by_key(key), "completed")
```

## Group forest-themed maps in the same directory

When multiple custom maps share the same forest theme or are variations of the same existing map, a player can put their custom map in a directory that is shared with other maps.

```gdscript
# Save our map with a file-path-like key
var key = "forest_maps/my_map"
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create(custom_map, key), "completed")
# Load a map from the directory
var query = GotmQuery.new()
query.filter("directory", "forest_maps")
var forest_maps = yield(GotmContent.list(query), "completed")
var forest_map = yield(GotmContent.get_node(related_contents[0]), "completed")
```

## Browse maps by a player

When a player wants to browse custom maps made by a particular player, they can use that player's unique identifier.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("user_id", my_user_id)
var my_maps = yield(GotmContent.list(query), "completed")
var my_map = yield(GotmContent.get_node(my_maps[0]), "completed")
```

## Preview a heavy map without loading it

A custom map can be big and expensive to download or load from local storage. Before a player decides to play a heavy map, they can read the content's lightweight metadata to learn more about a map without loading the map.

```gdscript
# Save our map with custom metadata
var custom_map = get_node("custom_map")
var meta = {}
meta.difficulty = "hard"
meta.theme = "forest"
meta.tree_count = 20
var content = yield(GotmContent.create(custom_map, "", meta), "completed")
# Read metadata
var loaded_meta = content.properties
var difficulty = loaded_meta.difficulty
var theme = loaded_meta.theme
var tree_count = loaded_meta.tree_count
```

## Browse forest maps with many trees

If a player knows what kind of maps they like, they can explore new maps by using content metadata to filter and order maps. In this case the player wants to play a hard forest map with a lot of trees.

```gdscript
var query = GotmQuery.new()
query.filter("properties/difficulty", "hard")
query.filter("properties/theme", "forest")
query.filter_min("properties/tree_count", 1000)
var first_20_maps = yield(GotmContent.list(query), "completed")
var second_20_maps = yield(GotmContent.list(query, first_20_maps.back()), "completed")
```

## Upvote fun maps

After playing a map, a player can share their opinion of the map with other players and the map-creator by upvoting or downvoting it. This allows players to easier find popular maps with many upvotes and avoid unpopular maps with many downvotes.

```gdscript
# Create votes
var upvote_mark = yield(GotmMark.create(content, "upvote"))
var downvote_mark = yield(GotmMark.create(content, "downvote"))
# Count votes by all users
var total_upvote_count = yield(GotmMark.get_count(content, "upvote"), "completed")
var total_downvote_count = yield(GotmMark.get_count(content, "downvote"), "completed")
# Get my votes
var my_upvotes = yield(GotmMark.list_by_target(content, "upvote"), "completed")
var my_downvotes = yield(GotmMark.list_by_target(content, "downvote"), "completed")
```

## Browse a player's upvoted forest maps

A player can upvote a map as a way to easily find the map when they want to play it again later. Some players are also interested in what maps their friends have upvoted.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("upvote_user_id", my_user_id)
query.filter("properties/theme", "forest")
var my_upvoted_maps = yield(GotmContent.list(query), "completed")
```

## Browse top voted hard maps

Some players are only interested in the most popular maps and would like to browse maps with many upvotes and few downvotes.

```gdscript
var query = GotmQuery.new()
query.filter("properties/difficulty", "hard")
query.sort("score")
var first_top_20_maps = yield(GotmContent.list(query), "completed")
var second_top_20_maps = yield(GotmContent.list(query, first_top_20_maps.back()), "completed")
```

## Search memorable maps by name

When a player searches for a particular map, they may only remember a part of the maps' name.

```gdscript
# Save our map with a searchable name
var custom_map = get_node("custom_map")
var name = "The best map ever!"
var content = yield(GotmContent.create(custom_map, "", {}, name), "completed")
# Search maps using a part of the name
var query = GotmQuery.new()
query.filter("name_part", "best map")
var first_20_maps = yield(GotmContent.list(query), "completed")
var second_20_maps = yield(GotmContent.list(query, first_20_maps.back()), "completed")
```
