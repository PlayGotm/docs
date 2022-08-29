###### Add custom metadata to %subject%

```gdscript
# Save our %subject% with custom metadata
var %subject% = get_node("%subject%")
var meta = {}
meta.%prop1% = %value1%
meta.%prop2% = %value2%
meta.%prop3% = %value3%
var content = yield(GotmContent.create(%subject%, "", meta), "completed")
# Read metadata
var loaded_meta = content.properties
var %prop1% = loaded_meta.%prop1%
var %prop2% = loaded_meta.%prop2%
var %prop3% = loaded_meta.%prop3%
```
