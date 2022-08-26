# Mix local and global scores

## Create local scores

```gdscript
var local_score = yield(GotmScore.create_local("score_name", 1), "completed")
print(local_score.is_local) # Prints true
```

## List local scores

```gdscript
var local_leaderboard = GotmLeaderboard.new()
local_leaderboard.name = "score_name"
local_leaderboard.is_local = true
var local_scores = yield(local_leaderboard.get_scores(), "completed")
```

<include subject="score">

[](/src/utility/disable-global-mode-for-all-subjects.md)

</include>
