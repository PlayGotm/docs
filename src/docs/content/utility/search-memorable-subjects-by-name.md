# Search memorable %subject%s by name

When a player searches for a particular %subject%, they may only remember a part of the %subject%'s name.

```gdscript
# Save our %subject% with a searchable name
var %subject% = get_node("%subject%")
var name = "The best %subject% ever!"
var content = yield(GotmContent.create(%subject%, "", {}, name), "completed")
# Search %subject%s using a part of the name
var query = GotmQuery.new()
query.filter("name_part", "best %subject%")
var first_20_%subject%s = yield(GotmContent.list(query), "completed")
var second_20_%subject%s = yield(GotmContent.list(query, first_20_%subject%s.back()), "completed")
```
