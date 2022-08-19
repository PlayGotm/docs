## Enchant a sword

A player can enchant their sword with anti-fire essences to increase the sword's fire resistance.

```gdscript
# Find our sword in our inventory
var auth = yield(GotmAuth.fetch(), "completed")
var my_inventory_key = auth.user_id + "/inventory"
var my_inventory = yield(GotmContent.get_variant_by_key(my_inventory_key), "completed")
var my_sword
for item in my_inventory:
    if item.name === "sword":
        my_sword = item:
        break
# Enchant sword
for item in my_inventory.duplicate():
    if item == "anti_fire_essence":
        my_sword.fire_resistance += 100
        my_inventory.erase(item)
# Save inventory
yield(GotmContent.update_by_key(my_inventory_key, my_inventory), "completed")
```
