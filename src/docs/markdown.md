# Markdown

When a game developer or player wants to add a description to their game or their profile page, they can make the description look nice by writing it in [Markdown](https://commonmark.org/help/).

In addition to the [standard Markdown syntax](https://commonmark.org/help/), Gotm also supports some custom behavior.

# Include another Markdown file

When your Markdown file starts to become big or when you want to reuse Markdown snippets in multiple places, it's useful to split the Markdown into multiple files. The separate files can then be included into a Markdown file.

###### Markdown file to be included

```markdown
# separate.md

Include me!
```

###### Main markdown file

```markdown
# main.md

<include>

[](/separate.md)

</include>
```

###### What the result looks like

```markdown
# main.md

# separate.md

Include me!
```

## Replace text in included Markdown file

When reusing the same Markdown files in several places, the topic tends to change and it can be useful to replace certain keywords in the included Markdown file.

###### Markdown file to be included with replacements

```markdown
# separate.md

Include %subject%!
```

###### Main markdown file with replacements

```markdown
# main.md

<include subject="garlic bread">[](/separate.md)</include>
```

###### What the result looks like with replacements

```markdown
# main.md

# separate.md

Include garlic bread!
```

# Embed a game

When explaining a something game-related it can be useful to show a demo as an example. Games hosted on Gotm can be embedded into Markdown.

In this example, we embed the game hosted at https://gotm.io/game-examples/lobbies.

###### Embed a game in Markdown

```markdown
<game>[](/game-examples/lobbies)</game>
```

The result looks like this:

<game>[](/game-examples/lobbies)</game>

# Button style

When buttons are created in Markdown, they get a special style on Gotm.

###### Make a button

```markdown
<button>My Button!</button>
```

The result looks like this:

<button>My Button!</button>

## Button attributes

The button supports some special attributes:

- `outlined`: Add an outline to the button.
- `icon`: Add an icon to the button. See [Supported button icons](#supported-button-icons).

The example below shows how to make an outlined button with a "home" icon.

###### Make an outlined home button

```markdown
<button outlined icon="home">Home</button>
```

The result looks like this:

<button outlined icon="home">Home</button>

###### Supported button icons

| Name        | How it looks                         | Code                                   |
| ----------- | ------------------------------------ | -------------------------------------- |
| home        | <button icon="home"></button>        | `<button icon="home"></button>`        |
| content     | <button icon="content"></button>     | `<button icon="content"></button>`     |
| leaderboard | <button icon="leaderboard"></button> | `<button icon="leaderboard"></button>` |
| lobby       | <button icon="lobby"></button>       | `<button icon="lobby"></button>`       |
| gdgotm      | <button icon="gdgotm"></button>      | `<button icon="gdgotm"></button>`      |
| file        | <button icon="file"></button>        | `<button icon="file"></button>`        |
| multiplayer | <button icon="multiplayer"></button> | `<button icon="multiplayer"></button>` |
| sparkles    | <button icon="sparkles"></button>    | `<button icon="sparkles"></button>`    |
| auction     | <button icon="auction"></button>     | `<button icon="auction"></button>`     |
| hammer      | <button icon="hammer"></button>      | `<button icon="hammer"></button>`      |
| trees       | <button icon="trees"></button>       | `<button icon="trees"></button>`       |
| user        | <button icon="user"></button>        | `<button icon="user"></button>`        |
| package     | <button icon="package"></button>     | `<button icon="package"></button>`     |
| shop        | <button icon="shop"></button>        | `<button icon="shop"></button>`        |
| scale       | <button icon="scale"></button>       | `<button icon="scale"></button>`       |
| handshake   | <button icon="handshake"></button>   | `<button icon="handshake"></button>`   |
| star        | <button icon="star"></button>        | `<button icon="star"></button>`        |
| reference   | <button icon="reference"></button>   | `<button icon="reference"></button>`   |
| offline     | <button icon="offline"></button>     | `<button icon="offline"></button>`     |
