# Custom Godot characters

Many players enjoy customizing their in-game character for the same reason they enjoy a certain clothing style in real life. For some players customizing their in-game character is a way to express themselves, and for other players it can simply be a fun opportunity to make a goofy-looking character.

A player can decide how a character looks by customizing body features such as hair color, nose size, body length, and so on. A player can also modify the stats of the character, such as strength, intellect, agility, and so on.

How much control a player has in customizing their character can range from just a few predefined options like in the multiplayer game "World of Warcraft", to advanced sliders in the single-player game "The Elder Scrolls: Oblivion".

<include subject="character">

[](/src/docs/content/utility/editor-intro.md)

</include>

<include subject="character">

[](/src/docs/content/utility/share-subject-with-other-players.md)
[](/src/docs/content/utility/save-subject-locally.md)
[](/src/docs/content/utility/load-subject.md)

<include container="friends" scenario="When a player wants to add another player's character to their friends list">

[](/src/docs/content/utility/add-subject-to-container.md)

</include>

[](/src/docs/content/utility/share-subjects-with-friends-using-memorable-custom-keys.md)

</include>

## Group characters by class with directories

A player can group their characters by class by putting them in a directory that is shared with their other characters of the same class.

<include subject="character" directory="wizards">

[](/src/docs/content/utility/directory-snippet.md)

</include>

<include subject="character">

[](/src/docs/content/utility/browse-subjects-by-a-player.md)

</include>

## Add character stats with metadata

A player can describe the properties their character with the help of lightweight metadata. In this case the player has made a blue-haired wizard character with a high intellect of 100 but weak strength of 5.

<include subject="character" prop1="character_class" value1='"wizard"' prop2="hair_color" value2="blue" prop3="intellect" value3="100">

[](/src/docs/content/utility/save-metadata-snippet.md)

</include>

## Browse strong characters

A player can browse characters based on character stats. In this case a player is interested in strong characters with strength of at least 100.

<include subject="character" min="strength" value="100">

[](/src/docs/content/utility/filter-min-snippet.md)

</include>

<include subject="character">

[](/src/docs/content/utility/upvote-good-subjects.md)
[](/src/docs/content/utility/browse-a-players-upvoted-subjects.md)
[](/src/docs/content/utility/browse-top-voted-subjects.md)
[](/src/docs/content/utility/search-memorable-subjects-by-name.md)

</include>
