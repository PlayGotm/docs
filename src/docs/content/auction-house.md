# Auction house in Godot

An auction house lets a player efficiently sell items to other players without both parties having to be near each other at the same time. Multiple buyers can bid on the same item, and the highest bid wins the item.

Unlike direct [trading](./trading.md), a player can put an item for sale on the auction house and go do something else while any potential buyers buys that item from the auction house.

Some items can only be acquired from other players, such as specially [crafted](./crafting.md) or [enchanted](./enchanting.md) items.

An auction house is a game mechanic that fosters a community and allows an in-game economy to form. The prices of items on the auction house are usually based on supply and demand, and are organically determined by the players.

A simpler variant of the auction house is the [marketplace](./auction-house.md), where items are bought at the listed price without a bidding process. In some games, such as the multiplayer game "World of Warcraft", the auction house and marketplace are combined so that there's both a bidding process and a "buyout" price. If a buyer is not interested in participating in a bidding process, they can pay the "buyout" amount to instantly get the item, like a marketplace.

This example shows how to make an auction house in Godot by storing the [inventories](./inventory.md) of the seller and buyer in [Gotm contents](/src/docs/content.md). The auction house items are also stored as [Gotm content](/src/docs/content.md), which allows a player to use advanced searching, filtering and sorting when browsing the auction house.

To learn how a game developer can manually add items to the auction house, see [shops](./shop.md).

## Add a precious gem to the auction house

When a player wants to sell a precious gem through the auction house, they can add it to the auction house for a desired price and hope that another player bids on it. In this case the player wants to sell the gem for a starting price of 50 coins.

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

## Bid on gem

When a player wants to bid on a gem through the auction house, they can list all gem sales in the auction house and place a bid above the currently listed price.

When bidding on the gem, the buyer cannot store their bid directly on the gem sale content, because a player is not allowed to modify another player's content for security reasons.

Instead, the buyer stores the bid in a new content. When the seller wants to end the auction, they look at the highest bid, add the coins to their own inventory and removes the gem sale.

The seller also informs the bidders who won the auction by creating a child content for each bid. When the buyer sees the auction result they add the gem to their inventory and removes the bid. Because the auction result is a child to the bid, it is automatically removed when the bid is removed.

```gdscript
# Define buyer's inventory
var buyer_inventory = []
var coin_stack = {}
coin_stack.name = "coins"
coin_stack.amount = 100
buyer_inventory.append(coin_stack)
# Find the cheapest gem sale on the auction house
var query = GotmQuery.new()
query.filter("properties/name", "gem")
query.sort("properties/price")
var gem_sale_contents = yield(GotmContent.list(query), "completed")
var gem_sale_content = gem_sale_contents[0]
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_content.properties
# Remove coins from buyer's inventory and place a bid of 100 coins
buyer_inventory.erase(coin_stack)
var bid = {}
bid.sale_id = gem_sale_content.id
bid.amount = 100
yield(GotmContent.create(null, "", bid), "completed")
```

When the seller ends the auction, they can inform the highest bidder that they won.

```gdscript
# Get the seller's oldest gem sale
var auth = yield(GotmAuth.fetch(), "completed")
var sale_query = GotmQuery.new()
sale_query.filter("properties/name", "gem")
sale_query.filter("user_id", auth.user_id)
sale_query.sort("created", true)
var gem_sale_contents = yield(GotmContent.list(sale_query), "completed")
var gem_sale_content = gem_sale_contents[0]
# Remove the sale
yield(GotmContent.delete(gem_sale_content), "completed")
# Get the bids sorted by amount so that the highest bid comes first.
var bid_query = GotmQuery.new()
bid_query.filter("properties/sale_id", gem_sale_content.id)
bid_query.sort("properties/amount")
var bid_contents = yield(GotmContent.list(query), "completed")
# Inform the winners and the losers.
for i in range(0, bid_contents.size()):
    var bid_content = bid_contents[i]
    var result = {}
    result.name = "gem"
    result.bid_user_id = bid_content.user_id
    if i == 0:
        # Highest bid is the first element.
        result.is_winner = true
    else:
        result.is_winner = false
    # Create as a child to the bid so that it is automatically removed when the bidder
    # later on reads the result and removes the bid.
    yield(GotmContent.create(null, "", winner_result, "", false, bid_content.id))
```

Finally, when the buyer wants to know if they won the auction, they can list the auction result created by the seller.

```gdscript
# Define the buyer's inventory
var buyer_inventory = []
# Get the buyer's auction results
var auth = yield(GotmAuth.fetch(), "completed")
var result_query = GotmQuery.new()
result_query.filter("bid_user_id", auth.user_id)
var result_contents = yield(GotmContent.list(query), "completed")
for result_content in result_contents:
    # Is {"name": "gem", "bid_user_id": auth.user_id, "is_winner": true}
    var result = result_content.properties
    if result.is_winner:
        # Add the item to the buyer's inventory
        buyer_inventory.append(result.name)
    # Remove the bid. The result is automatically removed because the seller created the
    # result as a child to the bid.
    yield(GotmContent.delete(result_content.parents[0]), "completed")
# Save the buyer's inventory
var buyer_inventory_key = auth.user_id + "/inventory"
yield(GotmContent.update_by_key(buyer_inventory_key, buyer_inventory), "completed")
```

<include medium="auction house">

[](/src/docs/content/utility/marketplace-auction-house-common.md)

</include>
