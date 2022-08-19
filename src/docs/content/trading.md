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
