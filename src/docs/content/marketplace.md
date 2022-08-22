# Marketplace in Godot

A marketplace lets a player efficiently sell items to other players without both parties having to be near each other at the same time. Unlike direct [trading](./trading.md), a player can put an item for sale on the marketplace and go do something else while any potential buyers buys that item from the marketplace.

A marketplace is a game mechanic that fosters a community and allows an in-game economy to form. The prices of items on the marketplace are usually based on supply and demand, and are organically determined by the players.

Another variant of the marketplace is the [auction houses](./auction-house.md), where multiple players can place bids on the same item sale.

This example shows how to make a marketplace Godot by storing the [inventories](./inventory.md) of the seller and buyer in [Gotm contents](/src/docs/content.md). The marketplace items are also stored as [Gotm content](/src/docs/content.md), which allows a player to use advanced searching, filtering and sorting when browsing the marketplace.

To learn how a game developer can manually add items to the marketplace, see [shops](./shop.md)

## Add a precious gem to the marketplace

When a player wants to sell a precious gem through the marketplace, they can add it to the marketplace for a desired price and hope that another player buys it. In this case the player wants to sell the gem for 50 coins.

```gdscript
# Define seller's inventory
var seller_inventory = ["gem"]
# Remove gem from seller's inventory and put it on sale.
seller_inventory.erase("gem")
var gem_sale = {}
gem_sale.name = "gem"
gem_sale.price = 50
yield(GotmContent.create(null, "", gem_sale, "Gem"), "completed")
# Save seller's inventory
var auth = yield(GotmAuth.fetch(), "completed")
var seller_inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(seller_inventory_key, seller_inventory), "completed")
```

## Buy gem from the marketplace

When a player wants to buy a gem through the marketplace, they can list all gem sales in the marketplace and buy it for the listed price. When buying the gem, the buyer moves their coins to the seller's inventory and then moves the gem from the marketplace to the buyer's own inventory.

```gdscript
# Define buyer's inventory
var buyer_inventory = []
var coin_stack = {}
coin_stack.name = "coins"
coin_stack.amount = 50
buyer_inventory.append(coin_stack)
# Find the cheapest gem sale on the marketplace
var query = GotmQuery.new()
query.filter("properties/name", "gem")
query.sort("properties/price")
var gem_sale_contents = yield(GotmContent.list(query), "completed")
var gem_sale_content = gem_sale_contents[0]
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_content.properties
# Move the coins to the seller's inventory
buyer_inventory.erase(coin_stack)
var seller_inventory_key = gem_sale_content.user_id + "/inventory"
var seller_inventory = yield(GotmContent.get_variant_by_key(seller_inventory_key), "completed")
seller_inventory.append(coin_stack)
# Add the gem to the buyer's inventory
buyer_inventory.append(gem_sale.name)
# Save the buyer's and seller's inventories and remove gem sale from marketplace
var auth = yield(GotmAuth.fetch(), "completed")
var buyer_inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(buyer_inventory_key, buyer_inventory), "completed")
yield(GotmContent.update_by_key(seller_inventory_key, seller_inventory), "completed")
yield(GotmContent.delete(gem_sale_content), "completed")
```

## Find items for sale by a player

When a player wants to find items that are being sold by a particular player, they can use that player's unique identifier.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("user_id", my_user_id)
var game_sale_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "gem", "price": 50}
var gem_sale = game_sale_contents[0].properties
```

## Search items for sale by name

When a player wants to look up a recipe, they can easily find it by searching for its name. In this example the player searches for "ge", which matches "Gem".

```gdscript
var query = Query.new()
query.filter("name_part", "ge")
var game_sale_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "gem", "price": 50}
var gem_sale = game_sale_contents[0].properties
```

## Exclude expensive gems

When a player is searching for gems on the marketplace, they can exclude expensive gems from their search. In this case the player is only interesting in gems that cost less than 100 coins.

```gdscript
var query = GotmQuery.new()
query.filter("properties/name", "gem")
query.filter_max("properties/price", 100)
var gem_sale_contents = yield(GotmContent.list(query), "completed")
var gem_sale_content = gem_sale_contents[0]
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_content.properties
```
