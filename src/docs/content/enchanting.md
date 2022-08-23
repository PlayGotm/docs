# Enchanting in Godot

Enchanting lets a player improve an existing item by applying other ingredient items to it according to a recipe. For example, a player can make their ordinary sword inflict extra fire damage on enemies by enchanting it with a fire enchantment. Enchanting is an effective way to give a player more choices and control, while also creating an opportunity for interesting or unexpected gameplay.

Minecraft is a popular game where a player can enchant their tools, armor and weapons through a complicated enchanting system.

This example shows how to do crafting in Godot by storing the recipes and the player's [inventory](./inventory.md) in [Gotm contents](/src/docs/content.md).

## Enchant a sword with fire

A player can enchant their sword with fire to make the sword inflict extra fire damage on enemies.

```gdscript
# Define the recipe and inventory
var recipe = {}
recipe.name = "fire_damage"
recipe.ingredients = {}
recipe.ingredients.sword = true
recipe.ingredients.fire = true
recipe.effects = {}
recipe.effects.fire_damage = 10
var sword = {}
sword.name = "sword"
sword.fire_damage = 0
var fire = {}
fire.name = "fire"
var inventory = [sword, fire]
# Check if we have the required ingredients.
for ingredient in recipe.ingredients:
    var has_ingredient = false
    for item in inventory:
        if item.name == ingredient:
            has_ingredient = true
            break
    if !has_ingredient:
        return
# Enchant our sword
sword.fire_damage += recipe.effects.fire_damage
# Remove the ingredients from our inventory
for ingredient in recipe.ingredients:
    for item in inventory.duplicate():
        if item.name == ingredient:
            inventory.erase(item)
            break
# Save our inventory
yield(GotmContent.update_by_key(inventory_key, "inventory"), "completed")
```

## Create a recipe without updating the game

A game developer can add new recipes without updating the game. This example shows how to add a recipe that lets a player enchant a sword with fire. When the recipe is created it will be immediately accessible to all players.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Click the Create button.
1. Type `recipes/fire_damage` into the Key text field.
1. Type `Fire damage` into the Name text field to make it searchable by name.
1. Paste the contents below into the Properties text field.

```json
{
  "ingredients": {
    "sword": true,
    "fire": true
  },
  "effects": {
    "fire_damage": 10
  }
}
```

5. Click the Save button.

## Change a recipe without updating the game

A game developer can update existing recipes without updating the game. This example shows how to change the fire damage recipe to give 20 fire damage instead of 10. When the recipe is updated it will be immediately updated for all players.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Type `recipes/fire_damage` into the Key text field.
1. Click the Get button.
1. Paste the contents below into the Properties text field.

```json
{
  "ingredients": {
    "sword": true,
    "fire": true
  },
  "effects": {
    "fire_damage": 20
  }
}
```

6. Click the Save button.

## Remove a recipe without updating the game

A game developer can delete existing recipes without updating the game. This example shows how to delete the fire damage recipe. When the recipe is deleted it will immediately no longer be accessible to any player.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Type `recipes/fire_damage` into the Key text field.
1. Click the Get button.
1. Click the Delete button.

## Get recipe by key

A player can get a specific recipe by using the key the game developer used when creating the recipe. This example shows how to get the iron axe recipe.

```gdscript
var recipe = yield(GotmContent.get_properties_by_key("recipes/iron_axe"), "completed")
var ingredients = recipe.ingredients # Is {"sword": true, "fire": true}
var effects = recipe.effects # Is {"fire_damage": 10}
```

## List all recipes

A player can explore all possible recipes by listing all contents in the "recipes" directory.

```gdscript
# Get all contents whose keys start with "recipes/", sorted by name.
var query = Query.new()
query.filter("directory", "recipes")
query.sort("name", true)
var first_20_recipes = yield(GotmContent.list(query), "completed")
var recipe = first_20_recipes[0].properties
var ingredients = recipe.ingredients # Is {"sword": true, "fire": true}
var effects = recipe.effects # Is {"fire_damage": 10}
# (Get the next 20 recipes if there are more)
var second_20_recipes = yield(GotmContent.list(query, first_20_recipes.back()), "completed")
```

## List all recipes with a certain ingredient

When a player wants to know what they can do with a certain ingredient they have found, they can list all recipes that require that ingredient. This example shows how to list all recipes that require fire, which in this case is the fire damage recipe.

```gdscript
var query = Query.new()
query.filter("properties/ingredients/fire", true)
var first_20_recipes = yield(GotmContent.list(query), "completed")
var recipe = first_20_recipes[0].properties
var ingredients = recipe.ingredients # Is {"sword": true, "fire": true}
var effects = recipe.effects # Is {"fire_damage": 10}
# (Get the next 20 recipes if there are more)
var second_20_recipes = yield(GotmContent.list(query, first_20_recipes.back()), "completed")
```

## Search recipe by name

When a player wants to look up a recipe, they can easily find it by searching for its name. In this example the player searches for "fir", which matches "Fire damage".

```gdscript
var query = Query.new()
query.filter("name_part", "fir")
var recipes = yield(GotmContent.list(query), "completed")
var recipe = recipes[0].properties
var ingredients = recipe.ingredients # Is {"sword": true, "fire": true}
var effects = recipe.effects # Is {"fire_damage": 10}
```
