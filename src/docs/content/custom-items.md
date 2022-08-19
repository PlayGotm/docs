# Custom Godot items

Creative players love to customize whatever they can in a game because it lets them express themselves and influence their gaming experience in their own way. When a player has created something impressive or funny, they can also share it with friends and other players.

Lego Racers and "Gary Gadget: Building Cars" are two games where players can construct cars any way they like using parts they have collected in the game world. Without the car customization mechanic, these games would probably not have become as popular as they were.

<include subject="item">

[](/src/docs/content/utility/editor-intro.md)

</include>

<include subject="car">

[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="bookmarks" scenario="When a player wants to bookmark another player's car for inspiration, so that they can find it later">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

[](/src/docs/content/utility/share-subjects-with-friends-using-memorable-custom-keys.md)

</include>

## Group cars in a garage with directories

A player can group the cars they have created in a garage by putting them in a directory that is shared with their other cars.

<include subject="car" directory="my_garage">

[](/src/docs/content/utility/directory-snippet.md)

</include>

<include subject="car">

[](/src/docs/content/utility/browse-subjects-by-a-player.md)

</include>

## Add car stats with metadata

A player can describe the properties of a car by with the help of lightweight metadata. In this case the player has made a car with a top speed of 50, a weight of 1500 and it takes 5 seconds to accelerate from 0 to 50.

<include subject="car" prop1="speed" value1="50" prop2="weight" value2="1500" prop3="acceleration" value3="5">

[](/src/docs/content/utility/save-metadata-snippet.md)

</include>

## Browse fast cars

A player can browse cars based on car stats. In this case a player is interested in fast cars with a top speed of at least 100.

<include subject="map" min="speed" value="100">

[](/src/docs/content/utility/filter-min-snippet.md)

</include>

<include subject="car">

[](/src/docs/content/utility/upvote-good-subjects.md)
[](/src/docs/content/utility/browse-a-players-upvoted-subjects.md)
[](/src/docs/content/utility/browse-top-voted-subjects.md)
[](/src/docs/content/utility/search-memorable-subjects-by-name.md)

</include>
