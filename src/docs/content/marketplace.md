# Marketplace in Godot

A marketplace lets a player efficiently sell items to other players without both parties having to be near each other at the same time. Unlike direct [trading](./trading.md), a player can put an item for sale on the marketplace and go do something else while any potential buyers buys that item from the marketplace.

Some items can only be acquired from other players, such as specially [crafted](./crafting.md) or [enchanted](./enchanting.md) items.

A marketplace is a game mechanic that fosters a community and allows an in-game economy to form. The prices of items on the marketplace are usually based on supply and demand, and are organically determined by the players.

Another variant of the marketplace is the [auction houses](./auction-house.md), where multiple players can place bids on the same item sale.

This example shows how to make a marketplace in Godot by storing the [inventories](./inventory.md) of the seller and buyer in [Gotm contents](/src/docs/content.md). The marketplace items are also stored as [Gotm content](/src/docs/content.md), which allows a player to use advanced searching, filtering and sorting when browsing the marketplace.

To learn how a game developer can manually add items to the marketplace, see [shops](./shop.md).

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Add a precious gem to the marketplace

When a player wants to sell a precious gem through the marketplace, they can add it to the marketplace for a desired price and hope that another player buys it. In this case the player wants to sell the gem for 50 coins.

###### Move gem from inventory to marketplace

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

# Buy gem from the marketplace

When a player wants to buy a gem through the marketplace, they can list all gem sales in the marketplace and buy it for the listed price.

When buying the gem, the buyer cannot send their coins directly to the seller because they are not allowed to modify another player's inventory for security reasons.

Instead, the buyer stores the receipt of the purchase in a new content as a child to the gem sale. When the seller sees the receipt, they add the coins to their own inventory and remove the gem sale. Because the buyer created the receipt as a child to the gem sale, the receipt is automatically removed when the gem sale is removed.

###### Buy gem and create receipt

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
# Remove the coins and create a receipt
buyer_inventory.erase(coin_stack)
var receipt = {}
receipt.sale_user_id = gem_sale_content.user_id
receipt.price = gem_sale.price
yield(GotmContent.create(null, "", receipt, "", false, gem_sale_content.id), "completed")
# Add the gem to the buyer's inventory
buyer_inventory.append(gem_sale.name)
# Save the buyer's inventory
var auth = yield(GotmAuth.fetch(), "completed")
var buyer_inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(buyer_inventory_key, buyer_inventory), "completed")
```

Later on, when the buyer checks if they have sold anything, they can list the receipts.

###### Cash in receipts

```gdscript
# Define seller's inventory
var seller_inventory = []
# Get receipts of all the seller's sales
var auth = yield(GotmAuth.fetch(), "completed")
var receipt_query = GotmQuery.new()
receipt_query.filter("properties/sale_user_id", auth.user_id)
var receipt_contents = yield(GotmContent.list(receipt_query), "completed")
# Add coins to seller inventory and remove gem sale, which also automatically removes the receipt.
for receipt_content in receipt_contents:
    var receipt = receipt_content.properties
    var coin_stack = {}
    coin_stack.name = "coins"
    coin_stack.amount = receipt.price
    seller_inventory.append(coin_stack)
    yield(GotmContent.delete(receipt_content.parents[0]), "completed")
# Save the seller's inventory
var seller_inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(seller_inventory_key, seller_inventory), "completed")
```

<include medium="marketplace">

[](/src/docs/content/utility/marketplace-auction-house-common.md)

</include>
