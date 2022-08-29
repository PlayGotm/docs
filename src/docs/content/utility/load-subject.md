# Load %subject%

When a player wants to play an existing %subject% that is stored in a content, they can load it from the content.

###### Load %subject% as Godot node

```gdscript
var instanced%subject% = yield(GotmContent.get_node(content), "completed")
add_child(instanced_%subject%)
```
