# Share %subject%s with friends using memorable custom keys

A player can easily send their %subject% to their friends by choosing a memorable key for their %subject% instead of using the content's automatically generated identifier.

```gdscript
# Save our %subject% with a key
var key = "awesome_%subject%_by_sam"
var %subject% = get_node("_%subject%")
var content = yield(GotmContent.create(%subject%, key), "completed")
# Load our %subject% with the same key
var shared_%subject% = yield(GotmContent.get_node_by_key(key), "completed")
```
