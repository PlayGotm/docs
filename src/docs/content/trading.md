# Trading in Godot

When a player wants a particular item that another player or non-player character has, they can acquire that item by giving away something valuable in return. For example, a sword can be traded for coins or a helmet.

Trading is a way to acquire items in a game, just like enemy [drop tables](./drop-tables.md) or [quests](./quests.md). Another form of trading include [shops](./shop.md), where a player can browse a collection of items and buy them for a set price.

Normally, trading requires both parties to be present and nearby. This makes trading different from [marketplaces](./marketplace.md) or [auction houses](./auction-house.md), where a player can put an item on sale and let other players buy or bid on the item without both parties having to be near each other at the same time.

This example shows how to do trading Godot by storing the [inventories](./inventory.md) of the two trading parties in [Gotm contents](/src/docs/content.md).

## Trade a sword for coins

A player can trade their sword for new items or coins with other players or non-player characters. In this case a player trades their sword for a coin with the town merchant.

```gdscript
# Check if we have a sword and they have a coin
var auth = yield(GotmAuth.fetch(), "completed")
var my_inventory_key = auth.user_id + "/inventory"
var merchant_inventory_key = "merchant/inventory"
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
