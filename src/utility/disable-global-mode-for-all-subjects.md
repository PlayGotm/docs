## Disable global mode for all %subject%s

When you enable [global mode](/src/docs/gdgotm.md#global-mode-for-online-or-multi-player-games), all newly created %subject%s are made global.

<gdscript>

If you want all %subject%s to be stored locally instead, you can disable global mode for %subject%s when [initializing the plugin](/src/docs/gdgotm.md#initialize).

```gdscript
var config = GotmConfig.new()
config.force_local_%subject%s = true
Gotm.initialize(config)
```

</gdscript>
