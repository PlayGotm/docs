# Crafting

More or less copy-paste from custom-items

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
