# Leaderboard

<include subject="score">

[](/src/utility/gdgotm-beta-notice.md)

</include>

Let your players compete against eachother or themselves by adding high scores and leaderboards to your game!

<include>

[](/src/utility/gdgotm-notice.md)

</include>

# Create scores

When creating scores, you need to provide two things:

1. A score `name` which describes what the score represents, for example "bananas_collected" or "time_elapsed".
1. A score `value` which describes how much the score is worth compared to other scores with the same name.

Create scores with [GotmScore](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmScore.gd)`.create`. In the example below we create three scores with the name "bananas_collected", and their values are 1, 2 and 3.

###### Create GotmScores

```gdscript
var score_name = "bananas_collected"
var score1: GotmScore = yield(GotmScore.create(score_name, 1), "completed")
var score2: GotmScore = yield(GotmScore.create(score_name, 2), "completed")
var score3: GotmScore = yield(GotmScore.create(score_name, 3), "completed")
```

BETA NOTICE: Scores and leaderboards are currently beta features. See the beta notice at the top of this page for more information.

## User data

When you create a score, the current user's id is automatically assigned to the score's `user_id` field. If you create a score while the current user is signed into Gotm, the user id of that score will point to a registered user which you can fetch with [GotmUser](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmUser.gd)`.fetch`.

###### Fetch user of a score

```gdscript
# Fetch the Gotm-registered user that created score1.
var user: Gotmuser = yield(GotmUser.fetch(score1.user_id), "completed")
if user:
    # User is a registered Gotm user and has a display name.
    # Access it with the user.display_name field.
    pass
else:
    # User is not registered and has no display name.
    pass
```

# Fetch scores

Scores are fetched with a [GotmLeaderboard](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmLeaderboard.gd) instance, which acts as a filter when fetching scores. You do not need to create a leaderboard before creating scores.

###### Create a leaderboard

```gdscript
var top_leaderboard = GotmLeaderboard.new()
top_leaderboard.name = score_name
```

Use `get_scores` to fetch up to 20 top scores in descending order (highest value first).

###### Get scores in leaderboard

```gdscript
# Returns [score3, score2, score1]
var top_scores = yield(top_leaderboard.get_scores(), "completed")
```

## Fetch surrounding scores

Use `get_surrounding_scores` to fetch up to 20 scores above and and 20 below a certain score.

###### Get surrounding scores in leaderboard

```gdscript
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores = yield(top_leaderboard.get_surrounding_scores(score2), "completed")
```

You can also get scores above and below a certain value...

###### Get surrounding scores in leaderboard by value

```gdscript
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores_by_value = yield(top_leaderboard.get_surrounding_scores(2.5), "completed")
```

... or above and below a certain rank with `get_surrounding_scores_by_rank`.

###### Get surrounding scores in leaderboard by rank

```gdscript
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores_by_rank = yield(top_leaderboard.get_surrounding_scores_by_rank(2), "completed")
```

## Fetch scores after

Pass a score to `get_scores` to fetch up to 20 scores after that score.

###### Get scores that comes after another score

```gdscript
# Returns [score1]
var scores_after_score_id = yield(top_leaderboard.get_scores(score2), "completed")
```

You can also get scores after certain value...

###### Get scores that come after a value

```gdscript
# Returns [score1]
var scores_after_value = yield(top_leaderboard.get_scores(2), "completed")
```

... or after a certain rank with `get_scores_by_rank`.

###### Get scores that come after a rank

```gdscript
# Returns [score2, score1]
var scores_after_rank = yield(top_leaderboard.get_scores_by_rank(1), "completed")
```

## Fetch score counts

Use `get_count` to get the total number of scores that matches the leaderboard.

###### Get the number of scores in a leaderboard

```gdscript
# Returns 3
var score_count = yield(top_leaderboard.get_count(), "completed")
```

You can also use `get_counts` to get counts across a range of values, which is useful if you want to display distribution graphs.

###### Get segmented score counts in a leaderboard

```gdscript
# Fetches the number of scores between 0 and 4, in 4 ranges.
# Returns [0, 1, 1, 1], because there are 0 scores in the range [0, 1),
# 1 score in [1, 2), 1 score in [2, 3) and 1 score in [3, 4], where ")"
# is exclusive.
var score_counts = yield(top_leaderboard.get_counts(0, 4, 4), "completed")
```

## Fetch rank

Use `get_rank` to get the rank of a score. Ranks start at 1, which means that the score with the highest value in a leaderboard has rank 1.

###### Get a score's rank

```gdscript
# Returns 1
var rank_from_score_id = yield(top_leaderboard.get_rank(score3), "completed")
```

You can also pass a value to get the rank a score would have if it would have that value.

###### Get a value's rank

```gdscript
# Returns 2
var rank_from_value = yield(top_leaderboard.get_rank(2.5), "completed")
```

## Invert score order

If you want rank 1 to be the score with lowest value instead of the highest value, set the `is_inverted` field to `true` on your `GotmLeaderboard` instance.

###### Get inverted rank and scores

```gdscript
top_leaderboard.is_inverted = true
# Returns 3
var inverted_rank = yield(top_leaderboard.get_rank(score3), "completed")
# Returns [score1, score2, score3]
var inverted_scores = yield(top_leaderboard.get_scores(), "completed")
```

## Rank older scores higher

If you want older scores to rank higher than newer scores with the same value, set the `is_oldest_first` field to `true` on your `GotmLeaderboard` instance.

###### Get ranks using a "oldest wins" policy

```gdscript
# score1 was rank 3, but is now 4 because score1_copy is a newer score with the same value
# and therefore took score1's place.
var score1_copy: GotmScore = yield(GotmScore.create(score1.name, score1.value), "completed")
# Returns 3
var score1_copy_rank_with_newest_first = yield(top_leaderboard.get_rank(score1_copy), "completed")
top_leaderboard.is_oldest_first = true
# Returns 4
var score1_copy_rank_with_oldest_first = yield(top_leaderboard.get_rank(score1_copy), "completed")
```

# Modify scores

Use `update` to update a score.

###### Update a score

```gdscript
yield(score2.update(5), "completed")
# Returns [score2, score3, score1]
yield(top_leaderboard.get_scores(), "completed")
```

Use `delete` to delete a score.

###### Delete a score

```gdscript
yield(score2.delete(), "completed")
# Returns [score3, score1]
yield(top_leaderboard.get_scores(), "completed")
```

# Filter scores

Gotm provides a powerful way to filter scores in multiple ways with `GotmLeaderboard` by setting any of the `properties`, `is_unique`, `period` or `user_id` fields.

## Filter by properties

Scores have an optional `properties` field where you can store your own metadata. You can also filter scores by these properties.

###### Get scores by properties

```gdscript
# Update properties on score
yield(score1.update(null, {"difficulty": "hard", "level": 25}), "completed")
# Set properties filter on leaderboard
top_leaderboard.properties = {"difficulty": "hard"}
# Returns [score1]
yield(top_leaderboard.get_scores(), "completed")
# Returns 1
yield(top_leaderboard.get_rank(score1), "completed")
```

## Filter by unique per user

If the same user creates multiple scores they will all show up in your fetches by default. Use `is_unique` to only fetch the last created score per user.

###### Get unique scores

```gdscript
top_leaderboard.is_unique = true
# Returns [score3]
yield(top_leaderboard.get_scores(), "completed")
# Returns 1
yield(top_leaderboard.get_rank(score3), "completed")
```

## Filter by time period

Scores created at any time are included in your fetches by default. Use `period` to only fetch scores that were created within a certain time period.

###### Get scores created within a time period

```gdscript
# Only include scores created in the last 24 hours
top_leaderboard.period = GotmPeriod.sliding(GotmPeriod.TimeGranularity.DAY)

# Only include scores created today
top_leaderboard.period = GotmPeriod.offset(GotmPeriod.TimeGranularity.DAY, 0)

# Only include scores created two days ago.
top_leaderboard.period = GotmPeriod.offset(GotmPeriod.TimeGranularity.DAY, -2)

# Only include scores created in February 2019
top_leaderboard.period = GotmPeriod.at(GotmPeriod.TimeGranularity.MONTH, 2019, 2)
```

## Filter by user

Scores created from any users are included in your fetches by default. Use `user_id` to only fetch scores that were created by a particular user.

###### Specify a user's leaderboard

```gdscript
top_leaderboard.user_id = "<A_USER_ID>"
```

# Manage scores in the dashboard

You can easily browse, delete and edit scores in the tools section of [your game's dashboard](/dashboard/_/_?p=tools&highlight=leaderboards).

# Reference

- [Gotm](https://github.com/PlayGotm/GDGotm/blob/master/gotm/Gotm.gd)
- [GotmConfig](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmConfig.gd)
- [GotmLeaderboard](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmLeaderboard.gd)
- [GotmScore](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmScore.gd)
- [GotmUser](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmUser.gd)
