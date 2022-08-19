<gdscript>

## Save %subject% locally

If a player is not finished with their custom %subject%, or does not want to or can't share it with other players, they can save it locally to their own device.

```gdscript
var custom_%subject% = get_node("custom_%subject%")
var content = yield(GotmContent.create_local(custom_%subject%))
```

</gdscript>
