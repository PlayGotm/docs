## Browse a player's upvoted %subject%s

A player can upvote %subject%s as a way to easily find them again later. Some players are also interested in what %subject%s their friends have upvoted.

```gdscript
var auth = yield(GotmAuth.fetch(), "completed")
var my_user_id = auth.user_id
var query = GotmQuery.new()
query.filter("upvote_user_id", my_user_id)
var my_upvoted_%subject%s = yield(GotmContent.list(query), "completed")
```
