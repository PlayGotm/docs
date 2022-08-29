# Inventory in Godot

An inventory contains everything a player is carrying in a game and is used for many things, such as [crafting](./crafting.md) and [trading](./trading.md). When the player exits the game they don't want to lose their items, which is why the inventory must be saved.

This example shows how to make an inventory in Godot by using an array. The inventory is saved to a [Gotm content](/src/docs/content.md), which makes it accessible to other players and devices if needed.

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Save inventory

A player can save their inventory as any Godot object. They can then load the inventory later when restarting the game.

###### Create inventory

```gdscript
var inventory = ["sword"]
var content = yield(GotmContent.create(inventory), "completed")
```

<gdscript>

# Save inventory locally

If a player's inventory doesn't need to be accessible to other players or devices, they can save it locally to their own device. They can then load the inventory later when restarting the game on the same device.

###### Create local inventory

```gdscript
var inventory = ["sword"]
var content = yield(GotmContent.create_local(inventory), "completed")
```

</gdscript>

# Hide inventory from other players

If a player's inventory shouldn't be accessible to other players, but needs to be accessible to other devices, the player can save it as a private content.

###### Create private inventory

```gdscript
var inventory = ["sword"]
var is_private = true
var content = yield(GotmContent.create(inventory, "", {}, "", is_private), "completed")
```

# Load inventory

A player can load their inventory from the content.

###### Get inventory as Godot variant

```gdscript
var inventory = yield(GotmContent.get_variant(content), "completed")
```

# Save inventory using custom key

A player can use a custom unique key when saving their inventory instead of using the content's automatically generated identifier.

###### Create inventory with key

```gdscript
# Save our inventory with a key
var key = "my_inventory"
var inventory = ["sword"]
var content = yield(GotmContent.create(inventory, key), "completed")
# Load our inventory later with the same key
var shared_inventory = yield(GotmContent.get_node_by_key(key), "completed")
```

<include subject="item" container="inventory" scenario="When a player wants to reference item contents in their inventory">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

</include>

# Add sword

A player can add a sword to their inventory by appending it to the array.

###### Append sword to inventory

```gdscript
var key = "my_inventory"
var inventory = yield(GotmContent.get_variant_by_key(key), "completed")
inventory.append("sword")
yield(GotmContent.update_by_key(key, inventory), "completed")
```

# Remove sword

A player can remove a sword to their inventory by erasing it from the array.

###### Remove sword from inventory

```gdscript
var key = "my_inventory"
var inventory = yield(GotmContent.get_variant_by_key(key), "completed")
inventory.erase("sword")
yield(GotmContent.update_by_key(key, inventory), "completed")
```

# Add coin stacks

A player can add coin stacks to their inventory. A coin stack is an item that can hold more than one coin. If a player already has a coin stack in their inventory, the new coin stack is merged with the existing one. If a player doesn't have a coin stack already, the coin stack is added without the need of merging.

###### Add coin stack to inventory

```gdscript
var coin_stack = {}
coin_stack.name = "coins"
coin_stack.amount = 20
var key = "my_inventory"
var inventory = yield(GotmContent.get_variant_by_key(key), "completed")
# Find any existing coin stack that we should merge our stack with
var existing_coin_stack
for item in inventory:
    if item.name == "coins":
        existing_coin_stack = item
        break
if existing_coin_stack:
    # Merge the stacks
    existing_coin_stack.amount += coin_stack.amount
else:
    # Add the new stack
    inventory.append(coin_stack)
yield(GotmContent.update_by_key(key, inventory), "completed")
```

# Inspect player inventory

A player can inspect a particular player's inventory by combining the key with the player's user id.

###### Get player's inventory

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

# Limit inventory size

How many items a player's inventory can contain can be limited.

###### Block new items if inventory is full

```gdscript
var size_limit = 20
var item = "sword"
var key = "my_inventory"
var inventory = yield(GotmContent.get_variant_by_key(key), "completed")
# Don't add the item if the inventory is full
if inventory.size() >= size_limit:
    return
# Add the item
inventory.append(item)
yield(GotmContent.update_by_key(key, inventory), "completed")
```

# Limit inventory weight

How much weight a player's inventory can hold can be limited.

###### Block new items if inventory weighs too much

```gdscript
var weight_limit = 100
var item = {}
item.name = "sword"
item.weight = 10
var key = "my_inventory"
var inventory = yield(GotmContent.get_variant_by_key(key), "completed")
# Get the weight of the inventory
var inventory_weight = 0
for item in inventory:
    inventory_weight += item.weight
# Don't add the item if the inventory will weigh too much
if inventory_weight + item.weight > weight_limit:
    return
# Add the item
inventory.append(item)
yield(GotmContent.update_by_key(key, inventory), "completed")
```
