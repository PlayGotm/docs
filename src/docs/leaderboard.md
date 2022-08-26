#### Leaderboards

Let your players compete against eachother or themselves.

<a href="/docs/leaderboards">
  <OutlinedButton padding="10px 15px" margin="10px">
    Get started
  </OutlinedButton>
</a>
<br />

# Leaderboard

BETA NOTICE: Leaderboards are currently beta features and are stored locally unless the game is running on Gotm, even if you have provided a project key. Using beta features is safe when using the official [GDScript plugin](/about/plugin).

With the [GDScript plugin](/about/plugin) you can easily add high scores and leaderboards to your game.

[<button outlined icon="download">
Download Project
</button>](https://github.com/PlayGotm/Game-Examples/releases/latest/download/Leaderboards.zip)
[<button outlined icon="code">
Browse source
</button>](https://github.com/PlayGotm/Game-Examples/tree/master/Leaderboards)

The above game example uses the GDScript plugin to create scores and list them with leaderboads. See the [source code](https://github.com/PlayGotm/Game-Examples/tree/master/Leaderboards) for more details.

#### Get started

[Download](https://github.com/PlayGotm/GDGotm/archive/master.zip) the plugin and extract its contents anywhere in your game's project directory. You can also get the plugin by searching for `GDGotm` in [Godot's Asset Library](https://docs.godotengine.org/en/stable/tutorials/assetlib/using_assetlib.html#in-the-editor).

Install the plugin by following the installation instructions in the `README.md` file.

The [example project](https://github.com/PlayGotm/Game-Examples/releases/latest/download/Leaderboards.zip) already comes with the plugin installed.

The code snippets in this guide are taken from [this code](https://github.com/PlayGotm/Game-Examples/blob/master/Leaderboards/Root.gd) in the example project.

<Anchor id="1-initialize"></Anchor>

##### 1. Initialize

Before you can use the plugin you must initialize it by calling [Gotm](https://github.com/PlayGotm/GDGotm/blob/master/gotm/Gotm.gd)`.initialize`.

```
Gotm.initialize()
```

By default the plugin stores scores locally on the player's device. This means a player can see their own scores, but not scores from other players.

If you provide a project key, the data will be stored on Gotm's cloud. This means players can see each other's scores. You can create a project key in the tools section of [your game's dashboard](/dashboard).

To provide a project key, initialize the plugin with a [GotmConfig](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmConfig.gd) instance.

```
var config = GotmConfig.new()
config.project_key = "" # YOUR PROJECT KEY HERE
Gotm.initialize(config)
```

<Anchor id="2-create-scores"></Anchor>

##### 2. Create scores

When creating scores, you need to provide two things:

1. A score `name` which describes what the score represents, for example "bananas_collected" or "time_elapsed".
1. A score `value` which describes how much the score is worth compared to other scores with the same name.

Create scores with [GotmScore](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmScore.gd)`.create`. In the example below we create three scores with the name "bananas_collected", and their values are 1, 2 and 3.

```
var score_name = "bananas_collected"
var score1: GotmScore = yield(GotmScore.create(score_name, 1), "completed")
var score2: GotmScore = yield(GotmScore.create(score_name, 2), "completed")
var score3: GotmScore = yield(GotmScore.create(score_name, 3), "completed")
```

Note that scores are only stored locally if you haven't initialized the plugin with a project key.

BETA NOTICE: Scores and leaderboards are currently beta features. See the beta notice at the top of this page for more information.

###### User data

When you create a score, the current user's id is automatically assigned to the score's `user_id` field. If you create a score while the current user is signed into Gotm, the user id of that score will point to a registered user which you can fetch with [GotmUser](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmUser.gd)`.fetch`.

```
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

<Anchor id="3-fetch-scores"></Anchor>

##### 3. Fetch scores

Scores are fetched with a [GotmLeaderboard](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmLeaderboard.gd) instance, which acts as a filter when fetching scores. You do not need to create a leaderboard before creating scores.

```
var top_leaderboard = GotmLeaderboard.new()
top_leaderboard.name = score_name
```

Use `get_scores` to fetch up to 20 top scores in descending order (highest value first).

```
# Returns [score3, score2, score1]
var top_scores = yield(top_leaderboard.get_scores(), "completed")
```

###### Fetch surrounding scores

Use `get_surrounding_scores` to fetch up to 20 scores above and and 20 below a certain score.

```
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores = yield(top_leaderboard.get_surrounding_scores(score2), "completed")
```

You can also get scores above and below a certain value...

```
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores_by_value = yield(top_leaderboard.get_surrounding_scores(2.5), "completed")
```

... or above and below a certain rank with `get_surrounding_scores_by_rank`.

```
# Returns {"before": [score3], "score": score2, "after": [score1]}
var surrounding_scores_by_rank = yield(top_leaderboard.get_surrounding_scores_by_rank(2), "completed")
```

###### Fetch scores after

Pass a score to `get_scores` to fetch up to 20 scores after that score.

```
# Returns [score1]
var scores_after_score_id = yield(top_leaderboard.get_scores(score2), "completed")
```

You can also get scores after certain value...

```
# Returns [score1]
var scores_after_value = yield(top_leaderboard.get_scores(2), "completed")
```

... or after a certain rank with `get_scores_by_rank`.

```
# Returns [score2, score1]
var scores_after_rank = yield(top_leaderboard.get_scores_by_rank(1), "completed")
```

###### Fetch score counts

Use `get_count` to get the total number of scores that matches the leaderboard.

```
# Returns 3
var score_count = yield(top_leaderboard.get_count(), "completed")
```

You can also use `get_counts` to get counts across a range of values, which is useful if you want to display distribution graphs.

```
# Fetches the number of scores between 0 and 4, in 4 ranges.
# Returns [0, 1, 1, 1], because there are 0 scores in the range [0, 1),
# 1 score in [1, 2), 1 score in [2, 3) and 1 score in [3, 4], where ")"
# is exclusive.
var score_counts = yield(top_leaderboard.get_counts(0, 4, 4), "completed")
```

###### Fetch rank

Use `get_rank` to get the rank of a score. Ranks start at 1, which means that the score with the highest value in a leaderboard has rank 1.

```
# Returns 1
var rank_from_score_id = yield(top_leaderboard.get_rank(score3), "completed")
```

You can also pass a value to get the rank a score would have if it would have that value.

```
# Returns 2
var rank_from_value = yield(top_leaderboard.get_rank(2.5), "completed")
```

###### Invert score order

If you want rank 1 to be the score with lowest value instead of the highest value, set the `is_inverted` field to `true` on your `GotmLeaderboard` instance.

```
top_leaderboard.is_inverted = true
# Returns 3
var inverted_rank = yield(top_leaderboard.get_rank(score3), "completed")
# Returns [score1, score2, score3]
var inverted_scores = yield(top_leaderboard.get_scores(), "completed")
```

###### Rank older scores higher

If you want older scores to rank higher than newer scores with the same value, set the `is_oldest_first` field to `true` on your `GotmLeaderboard` instance.

```
# score1 was rank 3, but is now 4 because score1_copy is a newer score with the same value
# and therefore took score1's place.
var score1_copy: GotmScore = yield(GotmScore.create(score1.name, score1.value), "completed")
# Returns 3
var score1_copy_rank_with_newest_first = yield(top_leaderboard.get_rank(score1_copy), "completed")
top_leaderboard.is_oldest_first = true
# Returns 4
var score1_copy_rank_with_oldest_first = yield(top_leaderboard.get_rank(score1_copy), "completed")
```

<Anchor id="4-modify-scores"></Anchor>

##### 4. Modify scores

Use `update` to update a score.

```
yield(score2.update(5), "completed")
# Returns [score2, score3, score1]
yield(top_leaderboard.get_scores(), "completed")
```

Use `delete` to delete a score.

```
yield(score2.delete(), "completed")
# Returns [score3, score1]
yield(top_leaderboard.get_scores(), "completed")
```

<Anchor id="5-filter-scores"></Anchor>

##### 5. Filter scores

Gotm provides a powerful way to filter scores in multiple ways with `GotmLeaderboard` by setting any of the `properties`, `is_unique`, `period` or `user_id` fields.

###### Filter by properties

Scores have an optional `properties` field where you can store your own metadata. You can also filter scores by these properties.

```
# Update properties on score
yield(score1.update(null, {"difficulty": "hard", "level": 25}), "completed")
# Set properties filter on leaderboard
top_leaderboard.properties = {"difficulty": "hard"}
# Returns [score1]
yield(top_leaderboard.get_scores(), "completed")
# Returns 1
yield(top_leaderboard.get_rank(score1), "completed")
```

###### Filter by unique per user

If the same user creates multiple scores they will all show up in your fetches by default. Use `is_unique` to only fetch the last created score per user.

```
top_leaderboard.is_unique = true
# Returns [score3]
yield(top_leaderboard.get_scores(), "completed")
# Returns 1
yield(top_leaderboard.get_rank(score3), "completed")
```

###### Filter by time period

Scores created at any time are included in your fetches by default. Use `period` to only fetch scores that were created within a certain time period.

```
# Only include scores created in the last 24 hours
top_leaderboard.period = GotmPeriod.sliding(GotmPeriod.TimeGranularity.DAY)

# Only include scores created today
top_leaderboard.period = GotmPeriod.offset(GotmPeriod.TimeGranularity.DAY, 0)

# Only include scores created two days ago.
top_leaderboard.period = GotmPeriod.offset(GotmPeriod.TimeGranularity.DAY, -2)

# Only include scores created in February 2019
top_leaderboard.period = GotmPeriod.at(GotmPeriod.TimeGranularity.MONTH, 2019, 2)
```

###### Filter by user

Scores created from any users are included in your fetches by default. Use `user_id` to only fetch scores that were created by a particular user.

```
top_leaderboard.user_id = "<A_USER_ID>"
```

<Anchor id="manage-scores-in-the-dashboard"></Anchor>

#### Manage scores in the dashboard

You can easily browse, delete and edit scores in the tools section of [your game's dashboard](/dashboard).

<Anchor id="reference"></Anchor>

#### Reference

- [Gotm](https://github.com/PlayGotm/GDGotm/blob/master/gotm/Gotm.gd)
- [GotmConfig](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmConfig.gd)
- [GotmLeaderboard](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmLeaderboard.gd)
- [GotmScore](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmScore.gd)
- [GotmUser](https://github.com/PlayGotm/GDGotm/blob/master/gotm/GotmUser.gd)
