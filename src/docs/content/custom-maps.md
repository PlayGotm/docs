# Custom Godot maps

Enabling players to create their own custom maps or levels is an effective way to foster a vibrant and creative community in a game. Dota 2 and Counter-Strike are today very popular games that started off as custom maps within another game.

Custom maps can also give players the creative freedom to create their own gameplay mechanics and subgenres within a game. For example, in "surf maps" in Counter-Strike, players focus on surfing on walls through an obstacle course instead of shooting people. Surf maps have even spawned subgenres within itself, with some maps focusing on speedsurfing and others on tricksurfing. The surf mechanic is originally an exploitation of a physics bug.

<include subject="map">

[](/src/docs/content/utility/editor-intro.md)
[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="favorites" scenario="If a player has a list of favorite custom maps, or maps that they want to play later">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

</include>

<include subject="map">

[](/src/docs/content/utility/share-subjects-with-friends-using-memorable-custom-keys.md)

</include>

# Group forest-themed maps in the same directory

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

<include subject="map">

[](/src/docs/content/utility/browse-subjects-by-a-player.md)

</include>

# Preview a heavy map without loading it

A custom map can be big and expensive to download or load from local storage. Before a player decides to play a heavy map, they can read the content's lightweight metadata to learn more about a map without loading the map.

<include subject="map" prop1="difficulty" value1='"hard"' prop2="theme" value2='"forest"' prop3="tree_count" value3="20">

[](/src/docs/content/utility/save-metadata-snippet.md)

</include>

# Browse forest maps with many trees

If a player knows what kind of maps they like, they can explore new maps by using content metadata to filter and order maps. In this case the player wants to play map with at least 20 trees.

<include subject="map" prop="tree_count" value1="20">

[](/src/docs/content/utility/filter-min-snippet.md)

</include>

<include subject="map">

[](/src/docs/content/utility/upvote-good-subjects.md)
[](/src/docs/content/utility/browse-a-players-upvoted-subjects.md)
[](/src/docs/content/utility/browse-top-voted-subjects.md)
[](/src/docs/content/utility/search-memorable-subjects-by-name.md)

</include>
