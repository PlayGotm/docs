# Browse %subject%s by a player

When a player wants to browse %subject%s made by a particular player, they can use that player's unique identifier.

###### Get %subject%s by a player

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("user_id", my_user_id)
var my_%subject%s = yield(GotmContent.list(query), "completed")
var my_%subject% = yield(GotmContent.get_node(my_%subject%s[0]), "completed")
```
