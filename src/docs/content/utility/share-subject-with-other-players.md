## Share %subject% with other players

A player has finished creating their custom %subject% and is ready to make it accessible to other players by storing it in a content.

```gdscript
var custom_%subject% = get_node("custom_%subject%")
var content = yield(GotmContent.create(custom_%subject%))
```
