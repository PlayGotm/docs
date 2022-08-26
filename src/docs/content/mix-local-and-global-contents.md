# Mix local and global contents

## Create local contents

```gdscript
var local_content = yield(GotmContent.create_local(), "completed")
print(local_content.is_local) # Prints true
```

## List local contents

```gdscript
var local_query = GotmQuery.new()
local_query.filter("is_local", true)
var local_contents = yield(GotmContent.list(local_query), "completed")
```

<include subject="content">

[](/src/utility/disable-global-mode-for-all-subjects.md)

</include>
