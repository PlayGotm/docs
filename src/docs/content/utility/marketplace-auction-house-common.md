# Find items for sale by a player

When a player wants to find items that are being sold by a particular player in the %medium%, they can use that player's unique identifier.

###### Get items sold by player

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("user_id", my_user_id)
var gem_sale_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_contents[0].properties
```

# Search items for sale by name

When a player wants to look up an item in the %medium%, they can easily find it by searching for its name. In this example the player searches for "ge", which matches "Gem".

###### Search items for sale

```gdscript
var query = Query.new()
query.filter("name_part", "ge")
var gem_sale_contents = yield(GotmContent.list(query), "completed")
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_contents[0].properties
```

# Exclude expensive gems

When a player is searching for gems in the %medium%, they can exclude expensive gems from their search. In this case the player is only interested in gems that cost less than 100 coins.

###### Filter items for sale by price range

```gdscript
var query = GotmQuery.new()
query.filter("properties/name", "gem")
query.filter_max("properties/price", 100)
var gem_sale_contents = yield(GotmContent.list(query), "completed")
var gem_sale_content = gem_sale_contents[0]
# Is {"name": "gem", "price": 50}
var gem_sale = gem_sale_content.properties
```
