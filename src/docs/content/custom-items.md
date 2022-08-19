# Custom Godot items

Creative players love to customize whatever they can in a game because it lets them express themselves and influence their gaming experience in their own way. When a player has created something impressive or funny, they can also share it with friends and other players.

Lego Racers and "Gary Gadget: Building Cars" are two games where players can construct cars any way they like using parts they have collected in the game world. Without the car customization mechanic, these games would probably not have become as popular as they were.

<include subject="car">

[](/src/docs/content/utility/editor-intro.md)

</include subject="item">

<include subject="car">

[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="garage" scenario="When a player wants to save their car to their garage">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

</include>

## Group forest-themed items in the same directory

When multiple custom items share the same forest theme or are variations of the same existing item, a player can put their custom item in a directory that is shared with other items.

```gdscript
# Save our item with a file-path-like key
var key = "forest_items/my_item"
var custom_item = get_node("custom_item")
var content = yield(GotmContent.create(custom_item, key), "completed")
# Load a item from the directory
var query = GotmQuery.new()
query.filter("directory", "forest_items")
var forest_items = yield(GotmContent.list(query), "completed")
var forest_item = yield(GotmContent.get_node(related_contents[0]), "completed")
```

## Browse items by a player

When a player wants to browse custom items made by a particular player, they can use that player's unique identifier.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("user_id", my_user_id)
var my_items = yield(GotmContent.list(query), "completed")
var my_item = yield(GotmContent.get_node(my_items[0]), "completed")
```

## Preview a heavy item without loading it

A custom item can be big and expensive to download or load from local storage. Before a player decides to play a heavy item, they can read the content's light metadata to learn more about a item without loading the item.

```gdscript
# Save our item with custom metadata
var custom_item = get_node("custom_item")
var metadata_properties = {"difficulty": "hard", "theme": "forest", "tree_count": 20}
var content = yield(GotmContent.create(custom_item, "", metadata_properties), "completed")
# Read metadata
var difficulty = content.properties.difficulty
var theme = content.properties.theme
var tree_count = content.properties.tree_count
```

## Browse forest items with many trees

If a player knows what kind of items they like, they can explore new items by using content metadata to filter and order items. In this case the player wants to play a hard forest item with a lot of trees.

```gdscript
var query = GotmQuery.new()
query.filter("properties/difficulty", "hard")
query.filter("properties/theme", "forest")
query.filter_min("properties/tree_count", 1000)
var first_20_items = yield(GotmContent.list(query), "completed")
var second_20_items = yield(GotmContent.list(query, first_20_items.back()), "completed")
```

## Upvote fun items

After playing a item, a player can share their opinion of the item with other players and the item-creator by upvoting or downvoting it. This allows players to easier find popular items with many upvotes and avoid unpopular items with many downvotes.

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

## Browse a player's upvoted forest items

A player can upvote a item as a way to easily find the item when they want to play it again later. Some players are also interested in what items their friends have upvoted.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("upvote_user_id", my_user_id)
query.filter("properties/theme", "forest")
var my_upvoted_items = yield(GotmContent.list(query), "completed")
```

## Browse top voted hard items

Some players are only interested in the most popular items and would like to browse items with many upvotes and few downvotes.

```gdscript
var query = GotmQuery.new()
query.filter("properties/difficulty", "hard")
query.sort("score")
var first_top_20_items = yield(GotmContent.list(query), "completed")
var second_top_20_items = yield(GotmContent.list(query, first_top_20_items.back()), "completed")
```

## Search memorable items by name

When a player searches for a particular item, they may only remember a part of the items' name.

```gdscript
# Save our item with a searchable name
var custom_item = get_node("custom_item")
var name = "The best item ever!"
var content = yield(GotmContent.create(custom_item, "", {}, name), "completed")
# Search items using a part of the name
var query = GotmQuery.new()
query.filter("name_part", "best item")
var first_20_items = yield(GotmContent.list(query), "completed")
var second_20_items = yield(GotmContent.list(query, first_20_items.back()), "completed")
```
