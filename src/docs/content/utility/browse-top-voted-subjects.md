# Browse top voted %subject%s

Some players are only interested in the most popular %subject%s and would like to browse %subject%s with many upvotes and few downvotes.

```gdscript
var query = GotmQuery.new()
query.sort("score")
var first_top_20_%subject%s = yield(GotmContent.list(query), "completed")
var second_top_20_%subject%s = yield(GotmContent.list(query, first_top_20_%subject%s.back()), "completed")
```
