## Share %subject% with other players

A player has finished creating their %subject% and is ready to make it accessible to other players by storing it in a content.

```gdscript
var %subject% = get_node("%subject%")
var content = yield(GotmContent.create(%subject%), "completed")
```
