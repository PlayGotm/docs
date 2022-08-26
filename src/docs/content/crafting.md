# Crafting in Godot

Crafting is a popular game mechanic where a player can craft a new item from other ingredient items according to a recipe. A player can unlock new capabilities by crafting items, which adds depth and progression to the game.

Minecraft is a popular game where a player first crafts wooden tools, which lets the player collect ingredients for stone tools, which then lets the player collect ingredients for iron tools, and so on.

This example shows how to do crafting in Godot by storing the recipes and the player's [inventory](./inventory.md) in [Gotm contents](/src/docs/content.md).

<include>

[](/src/utility/gdgotm-notice.md)

</include>

## Craft an iron axe

A player can craft an iron axe from wood and iron.

```gdscript
# Define the recipe and inventory
var recipe = {}
recipe.name = "iron_axe"
recipe.ingredients = {}
recipe.ingredients.wood = true
recipe.ingredients.iron = true
var inventory = ["wood", "iron"]
# Check if we have the required ingredients.
for ingredient in recipe.ingredients:
    if !my_inventory.has(ingredient):
        return
# Add the new sword to our inventory
inventory.append(recipe.name)
# Remove the ingredients from our inventory.
for ingredient in recipe.ingredients:
    inventory.erase(ingredient)
# Save our inventory
yield(GotmContent.update_by_key(inventory_key, "inventory"), "completed")
```

## Create a recipe without updating the game

A game developer can add new recipes without updating the game. This example shows how to add a recipe that lets a player craft an axe from a piece of wood and an iron nugget. When the recipe is created it will be immediately accessible to all players.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Click the Create button.
1. Type `recipes/iron_axe` into the Key text field.
1. Type `Iron axe` into the Name text field to make it searchable by name.
1. Paste the contents below into the Properties text field.

```json
{
  "wood": true,
  "iron": true
}
```

5. Click the Save button.

## Change a recipe without updating the game

A game developer can update existing recipes without updating the game. This example shows how to change the iron axe recipe to only require an iron nugget. When the recipe is updated it will be immediately updated for all players.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Type `recipes/iron_axe` into the Key text field.
1. Click the Get button.
1. Paste the contents below into the Properties text field.

```json
{
  "iron": true
}
```

6. Click the Save button.

## Remove a recipe without updating the game

A game developer can delete existing recipes without updating the game. This example shows how to delete the iron axe recipe. When the recipe is deleted it will immediately no longer be accessible to any player.

1. Go to the [game's dashboard](/dashboard/_/_?page=tools).
1. Navigate to the Content card.
1. Type `recipes/iron_axe` into the Key text field.
1. Click the Get button.
1. Click the Delete button.

## Get recipe by key

A player can get a specific recipe by using the key the game developer used when creating the recipe. This example shows how to get the iron axe recipe.

```gdscript
# Is {"wood": true, "iron": true}
var ingredients = yield(GotmContent.get_properties_by_key("recipes/iron_axe"), "completed")
```

## List all recipes

A player can explore all possible recipes by listing all contents in the "recipes" directory.

```gdscript
# Get all contents whose keys start with "recipes/", sorted by name.
var query = Query.new()
query.filter("directory", "recipes")
query.sort("name", true)
var first_20_recipes = yield(GotmContent.list(query), "completed")
# Is {"wood": true, "iron": true}
var ingredients = first_20_recipes[0].properties
# (Get the next 20 recipes if there are more)
var second_20_recipes = yield(GotmContent.list(query, first_20_recipes.back()), "completed")
```

## List all recipes with a certain ingredient

When a player wants to know what they can do with a certain ingredient they have found, they can list all recipes that require that ingredient. This example shows how to list all recipes that require a piece of wood, which in this case is the iron axe recipe.

```gdscript
var query = Query.new()
query.filter("properties/wood", true)
var first_20_recipes = yield(GotmContent.list(query), "completed")
# Is {"wood": true, "iron": true}
var ingredients = first_20_recipes[0].properties
# (Get the next 20 recipes if there are more)
var second_20_recipes = yield(GotmContent.list(query, first_20_recipes.back()), "completed")
```

## Search recipe by name

When a player wants to look up a recipe, they can easily find it by searching for its name. In this example the player searches for "iro", which matches "Iron axe".

```gdscript
var query = Query.new()
query.filter("name_part", "iro")
var recipes = yield(GotmContent.list(query), "completed")
# Is {"wood": true, "iron": true}
var ingredients = recipes[0].properties
```
