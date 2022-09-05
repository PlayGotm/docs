# Shops in Godot

Shops sell various items to players for a predefined price. The items a shop offers are predefined by the game developer.

Since shops are predefined, they can take a role in the progression of the game. Depending on how far a player has gotten into a game, the items a shop is offering may change, or there can be new shops in new areas with more powerful items.

A player can also sell items to the shop for a predefined price, and is a common way for a player to earn coins which they can spend on more powerful items and progress further in the game.

A shop is a way to acquire items in a game, just like enemy [drop tables](./drop-tables.md) or [quests](./quests.md). Unlike [marketplaces](./marketplace.md) or [auction houses](./auction-house.md), shops are usually not used for transactions between players, but are instead used for transactions between a player and a non-player character.

This example shows how to make a shop in Godot by storing the player's [inventory](./inventory.md) and the shop's items in [Gotm contents](/src/docs/content.md).

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Buy a sword with coins

A player can buy a sword from a shop by spending coins.

###### Trade coin stack for sword

```gdscript
# Define the shop's inventory
var shop_inventory = []
var sword_for_sale = {}
sword_for_sale.name = "sword"
sword_for_sale.price = 50
shop_inventory.append(sword_for_sale)
# Define the player's inventory
var inventory = []
var coin_stack = {}
coin_stack.name = "coins"
coin_stack.amount = 50
inventory.append(coin_stack)
# Buy the sword with the coins
inventory.erase(coin_stack)
inventory.append(sword_for_sale.name)
# Save the player's inventory
var auth = yield(GotmAuth.fetch(), "completed")
var inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(inventory_key, inventory), "completed")
```

# Sort items by price

When a player wants to look at the cheapest items in a shop, they can sort the shop's items by price so that the cheapest items come first. In this case, the shop's items are stored in separate contents so that they can be listed in a more flexible way.

###### Get items by price

```gdscript
var query = GotmQuery.new()
query.sort("properties/price", true)
var sword_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "sword", "price": 50}
var sword = sword_contents[0].properties
```

# Add a sword to the shop without updating the game

A game developer can add new items to the shop without updating the game. This example shows how to add a sword. When the sword is added it will be immediately accessible to all players.

1. Go to the [game's dashboard](/dashboard/_/_?p=tools&highlight=contents).
1. Navigate to the Content card.
1. Click the Create button.
1. Type `Sword` into the Name text field to make it searchable by name.
1. Paste the contents below into the Properties text field.

```json
{
  "name": "sword",
  "price": 50
}
```

6. Click the Save button.

# Remove a sword from the shop without updating the game

A game developer can delete existing recipes without updating the game. This example shows how to delete the iron axe recipe. When the recipe is deleted it will immediately no longer be accessible to any player.

1. Go to the [game's dashboard](/dashboard/_/_?p=tools&highlight=contents).
1. Navigate to the Content card.
1. Type `Sword` into the Name text field.
1. Click the Get button.
1. Click the Delete button.

# Search items by name

When a player wants to look up an item in a big shop with many items, they can easily find it by searching for its name. In this example the player searches for "swo", which matches "Sword".

###### Search items

```gdscript
var query = GotmQuery.new()
query.filter("name_part", "swo")
var sword_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "sword", "price": 50}
var sword = sword_contents[0].properties
```

# Exclude expensive items

When a player is searching for swords in the shop, they can exclude expensive swords from their search. In this case the player is only interested in swords that cost less than 100 coins.

###### Filter items by price range

```gdscript
var query = GotmQuery.new()
query.filter("properties/name", "sword")
query.filter_max("properties/price", 100)
var sword_contents = yield(GotmContent.list(query), "completed")
var sword_content = sword_contents[0]
# Is {"name": "sword", "price": 50}
var sword = sword_content.properties
```
