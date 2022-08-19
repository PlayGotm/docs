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
