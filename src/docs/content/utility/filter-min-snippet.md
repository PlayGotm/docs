```gdscript
var query = GotmQuery.new()
query.filter_min("properties/%prop%", %value%)
var first_20_%subject%s = yield(GotmContent.list(query), "completed")
var second_20_%subject%s = yield(GotmContent.list(query, first_20_%subject%s.back()), "completed")
```
