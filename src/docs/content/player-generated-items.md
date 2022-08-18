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

# Player-generated items

After a player has created a custom item in your game they can share it with other players or save it locally to their device by storing it in a content.

In this example a player has already created their custom item, which is just a Godot node.

<include subject="item">

[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="inventory" scenario="When a player has acquired an item">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

</include>

## Inspect player inventory

A player can inspect a particular player's inventory by using a memorable key for their inventory instead of using the content's automatically generated identifier.

```gdscript
# Save an inventory with a unique key based on the player's user id
var auth = yield(GotmAuth.fetch(), "completed")
var key = auth.user_id + "/inventory"
var inventory = []
inventory.append("sword")
var content = yield(GotmContent.create(custom_item, key), "completed")
# Load the inventory with the same key
var inspected_inventory = yield(GotmContent.get_variant_by_key(key), "completed")
```

## Craft a unique sword

A player can craft a unique sword with different characteristics depending on the crafting ingredients. In this case a player crafts a wooden sword with an anti-fire essence.

```gdscript
# Create the recipe and inventory beforehand
var recipe_data = {}
recipe_data.required_ingredients = ["wood"]
recipe_data.optional_ingredients = ["anti_fire_essence"]
var recipe_key = "recipes/wooden_sword"
var inventory_data = ["wood"]
var auth = yield(GotmAuth.fetch(), "completed")
var inventory_key = auth.user_id + "/inventory"
yield(GotmContent.create(recipe_data, recipe_key), "completed")
yield(GotmContent.create(inventory_data, inventory_key), "completed")
# Later on we want to craft a sword.
# Check if we have the required ingredients.
var sword_recipe = yield(GotmContent.get_variant_by_key(recipe_key), "completed")
var my_inventory = yield(GotmContent.get_variant_by_key(inventory_key), "completed")
for required_ingredient in sword_recipe.required_ingredients:
    if !my_inventory.has(required_ingredient):
        return
# Check if we can optionally add an anti-fire essence.
if !sword_recipe.optional_ingredients.has("anti_fire_essence"):
    return
# Create the sword
var sword = {}
sword.recipe = "wooden_sword"
sword.fire_resistance = 0
for item in my_inventory:
    if item == "anti_fire_essence":
        sword.fire_resistance += 100
# Add the sword to our inventory
my_inventory.append(sword)
yield(GotmContent.update_by_key(inventory_key, my_inventory), "completed")
```

## Trade a sword for coins

A player can trade their sword for new items or coins with other players or non-player characters. In this case a player trades their sword for a coin with the town merchant.

```gdscript
# Check if we have a sword and they have a coin
var auth = yield(GotmAuth.fetch(), "completed")
var my_inventory_key = auth.user_id + "/inventory"
var merchant_inventory_key = "town_merchant_bot/inventory"
var my_inventory = yield(GotmContent.get_variant_by_key(my_inventory_key), "completed")
var merchant_inventory = yield(GotmContent.get_variant_by_key(merchant_inventory_key), "completed")
if !my_inventory.has("sword") || !merchant_inventory.has("coin"):
    return
# Trade the items
my_inventory.erase("sword")
merchant_inventory.append("sword")
merchant_inventory.erase("coin")
my_inventory.append("coin")
# Save the inventories
yield(GotmContent.update_by_key(my_inventory_key, my_inventory), "completed")
yield(GotmContent.update_by_key(merchant_inventory_key, merchant_inventory), "completed")
```

## Enchant a sword

A player can enchant their sword with anti-fire essences to increase the sword's fire resistance.

```gdscript
# Get our equipped sword and our inventory
var auth = yield(GotmAuth.fetch(), "completed")
var my_sword_key = auth.user_id + "/equipped_sword"
var my_inventory_key = auth.user_id + "/inventory"
var my_sword = yield(GotmContent.get_variant_by_key(my_sword_key), "completed")
var my_inventory = yield(GotmContent.get_variant_by_key(my_inventory_key), "completed")
# Enchant our sword
for item in my_inventory.duplicate():
    if item == "anti_fire_essence":
        my_sword.fire_resistance += 100
        my_inventory.erase(item)
# Save our equipped sword and inventory
yield(GotmContent.update_by_key(my_sword_key, my_sword), "completed")
yield(GotmContent.update_by_key(my_inventory_key, my_inventory), "completed")
```

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
