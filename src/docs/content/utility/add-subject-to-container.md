## Add %subject% to %container%

%scenario%, they can save a lightweight content identifier instead of the original content data. The content identifier can be used to retrieve the original content data.

```gdscript
# Save %subject% to our %container%
var %container% = []
%container%.append(content.id)
var %container%_content = yield(GotmContent.create(%container%), "completed")
# Load our %container%
var loaded_%container% = yield(GotmContent.get_variant(%container%_content.id), "completed")
var %subject% = yield(GotmContent.get_node(loaded_%container%[0]), "completed")
```
