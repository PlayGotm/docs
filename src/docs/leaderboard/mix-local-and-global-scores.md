<include subject="score">

[](/src/utility/mix-local-and-global-subjects-intro.md)
[](/src/utility/gdgotm-notice.md)

</include>

# Create local scores

When a player creates a score, they can choose whether they want to store it globally or locally.

```gdscript
# Create global score
var global_score = yield(GotmScore.create("score_name", 1), "completed")
print(global_score.is_local) # Prints false
```

```gdscript
# Create local score
var local_score = yield(GotmScore.create_local("score_name", 1), "completed")
print(local_score.is_local) # Prints true
```

# List local scores

When a player lists scores or interacts with a leaderboard in general, they can choose whether they want to use global or local scores and leaderboards.

```gdscript
# Use global scores and leaderboards
var global_leaderboard = GotmLeaderboard.new()
global_leaderboard.name = "score_name"
var global_scores = yield(global_leaderboard.get_scores(), "completed")
```

```gdscript
# Use local scores and leaderboards
var local_leaderboard = GotmLeaderboard.new()
local_leaderboard.name = "score_name"
local_leaderboard.is_local = true
var local_scores = yield(local_leaderboard.get_scores(), "completed")
```

<include subject="score">

[](/src/utility/disable-global-mode-for-all-subjects.md)

</include>
