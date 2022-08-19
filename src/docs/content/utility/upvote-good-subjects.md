## Upvote good %subject%s

A player can share their opinion of the %subject% with other players and the %subject%-creator by upvoting or downvoting it. This allows players to easier find popular %subject%s with many upvotes and avoid unpopular %subject%s with many downvotes.

```gdscript
# Create votes
var upvote_mark = yield(GotmMark.create(content, "upvote"), "completed")
var downvote_mark = yield(GotmMark.create(content, "downvote"), "completed")
# Count votes by all users
var total_upvote_count = yield(GotmMark.get_count(content, "upvote"), "completed")
var total_downvote_count = yield(GotmMark.get_count(content, "downvote"), "completed")
# Get my votes
var my_upvotes = yield(GotmMark.list_by_target(content, "upvote"), "completed")
var my_downvotes = yield(GotmMark.list_by_target(content, "downvote"), "completed")
```
