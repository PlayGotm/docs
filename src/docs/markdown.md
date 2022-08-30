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

In this example, we embed the game hosted at https://gotm.io/studio-handle/game-handle.

###### Embed a game in Markdown

```markdown
<game>[](/studio-handle/game-handle)</game>
```
