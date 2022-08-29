###### Get %subject%s by directory

```gdscript
# Save our %subject% with a file-path-like key
var key = "%directory%/my_%subject%"
var %subject% = get_node("%subject%")
var content = yield(GotmContent.create(%subject%, key), "completed")
# Load a %subject% from the directory
var query = GotmQuery.new()
query.filter("directory", "%directory%")
var listed_%subject%s = yield(GotmContent.list(query), "completed")
var listed_%subject% = yield(GotmContent.get_node(related_contents[0]), "completed")
```
