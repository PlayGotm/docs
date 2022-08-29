<gdscript>

# Save %subject% locally

If a player is not finished with their %subject%, or does not want to or can't share it with other players, they can save it locally to their own device.

###### Create local %subject%

```gdscript
var %subject% = get_node("%subject%")
var content = yield(GotmContent.create_local(%subject%), "completed")
```

</gdscript>
