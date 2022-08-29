<include subject="content">

[](/src/utility/mix-local-and-global-subjects-intro.md)
[](/src/utility/gdgotm-notice.md)

</include>

# Create local contents

When a player creates a content, they can choose whether they want to store it globally or locally.

###### Create global content

```gdscript
# Create global content
var global_content = yield(GotmContent.create(), "completed")
print(global_content.is_local) # Prints false
```

###### Create local content

```gdscript
# Create local content
var local_content = yield(GotmContent.create_local(), "completed")
print(local_content.is_local) # Prints true
```

# List local contents

When a player lists contents, they can choose whether they want to use global or local contents.

###### List global content

```gdscript
# Use global content
var global_query = GotmQuery.new()
var global_contents = yield(GotmContent.list(global_query), "completed")
```

###### List local content

```gdscript
# Use local content
var local_query = GotmQuery.new()
local_query.filter("is_local", true)
var local_contents = yield(GotmContent.list(local_query), "completed")
```

<include subject="content">

[](/src/utility/disable-global-mode-for-all-subjects.md)

</include>
