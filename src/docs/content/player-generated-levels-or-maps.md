<!--
MIT License

Copyright (c) 2020-2022 Macaroni Studios AB

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
-->

## Player-generated levels or maps

After a player has created a custom map or level in your game they can share it with other players or save it locally to their device by storing it in a content.

In this example a player has already created their custom map, which is just a Godot node.

### Share map with other players

A player has finished creating their custom map and is ready to make it accessible to other players by storing it in a content.

```gdscript
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create(custom_map))
```

<gdscript>

### Save map locally

If a player is not finished with their custom map, or does not want to or can't share it with other players, they can save it locally to their own device.

```gdscript
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create_local(custom_map))
```

</gdscript>

### Load map

When a player wants to play an existing map that is stored in a content, they can load it by using the content's unique identifier that was automatically generated when the content was created.

```gdscript
var instanced_custom_map = yield(GotmContent.get_node(content.id), "completed")
add_child(instanced_custom_map)
```

### Save maps in a favorite list

If a player has a list of favorite custom maps, or maps that they want to play later, they can save a list of lightweight content identifiers instead of the original content data. The content identifiers can be used to retrieve the original content data.

```gdscript
var favorites = [content.id]
var favorites_content = yield(GotmContent.create(favorites), "completed")
var loaded_favorites = yield(GotmContent.get_variant(favorites_content.id), "completed")
var favorite_map = yield(GotmContent.get_node(loaded_favorites[0]), "completed")
```

### Share maps with friends using memorable custom keys

A player can easily send their custom map to their friends by choosing a memorable key for their map instead of using the content's automatically generated identifier.

```gdscript
var key = "awesome_map_by_sam"
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create(custom_map, key), "completed")
var shared_content = yield(GotmContent.get_by_key(key), "completed")
var shared_map = yield(GotmContent.get_node(shared_content), "completed")
```

### Group forest-themed maps in the same directory

When multiple custom maps share the same forest theme or are variations of the same existing map, a player can put their custom map in a directory that is shared with other maps.

```gdscript
var key = "forest_maps/my_map"
var custom_map = get_node("custom_map")
var content = yield(GotmContent.create(custom_map, key), "completed")
var forest_maps = yield(GotmContent.list(GotmQuery.new().filter("directory", "forest_maps")), "completed")
var forest_map = yield(GotmContent.get_node(related_contents[0]), "completed")
```

### Browse maps by a player

When a player wants to browse custom maps made by a particular player, they can use that player's unique identifier.

```gdscript
var my_user_id = Gotm.user.id
var my_maps = yield(GotmContent.list(GotmQuery.new().filter("user_id", my_user_id)), "completed")
var my_map = yield(GotmContent.get_node(my_maps[0]), "completed")
```

### Preview a heavy map without loading it

A custom map can be big and expensive to download or load from local storage. Before a player decides to play a heavy map, they can read the content's light metadata to learn more about a map without loading the map.

```gdscript
var custom_map = get_node("custom_map")
var metadata_properties = {"difficulty": "hard", "theme": "forest", "tree_count": 20}
var content = yield(GotmContent.create(custom_map, "", metadata_properties), "completed")
var difficulty = content.properties.difficulty
var theme = content.properties.theme
var tree_count = content.properties.tree_count
```

### Browse forest maps with many trees

If a player knows what kind of maps they like, they can explore new maps by using content metadata to filter and order maps. In this case the player wants to play a hard forest map with a lot of trees.

```gdscript
var query = GotmQuery.new()
query.filter("properties/difficulty", "hard")
query.filter("properties/theme", "forest")
query.filter_min("properties/tree_count", 1000)
var first_20_maps = yield(GotmContent.list(query), "completed")
var second_20_maps = yield(GotmContent.list(query, first_20_maps.back()), "completed")
```

### Upvote fun maps

After playing a map, a player can share their opinion of the map with other players and the map-creator by upvoting or downvoting it. This allows players to easier find fun maps with many upvotes and avoid less fun maps with many downvotes.

```gdscript
yield(GotmMark.create(content, "upvote"))
yield(GotmMark.create(content, "downvote"))
var total_upvote_count = yield(GotmMark.get_count(content, "upvote"), "completed")
var total_downvote_count = yield(GotmMark.get_count(content, "downvote"), "completed")
var is_upvoted_by_me = yield(GotmMark.exists(content, "upvote"), "completed")
var is_downvoted_by_me = yield(GotmMark.exists(content, "downvote"), "completed")
```

### Browse my favorite maps

### Browse top voted maps

### Search memorable maps by name

Who
What
When
Where
Why
