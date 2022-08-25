# Welcome to the official documentation for Gotm!

This repository contains the source material for the [official documentation for Gotm](https://gotm.io/docs). Anyone may improve or add to the documentation by creating a [pull request](https://github.com/PlayGotm/docs/pulls) or by reporting an [issue](https://github.com/PlayGotm/docs/issues).

## Report

If you find something that could be improved but don't have the means to do it yourself, please create an [issue](https://github.com/PlayGotm/docs/issues).

## Contribute

You can contribute to this repository by creating a [pull request](https://github.com/PlayGotm/docs/pulls) containing your changes.

The documentation is structured as a flat collection of articles. An article is a web page that explains what a specific concept is on Gotm. An article also contains chapters that explain how the concept is used in specific scenarios.

For example, the "game" article explains what a game on Gotm is. The article's chapters then explain how to create a game, how to embed it on other websites, how to configure a game page, and so on. Since the "game" article is about the game concept, its source file is located at [`/src/docs/game.md`](https://github.com/PlayGotm/docs/blob/main/src/docs/game.md), and the resulting article on Gotm is located at [`https://gotm.io/docs/game`](https://gotm.io/docs/game). Since the "create a game" chapter belongs to the "game" article, its source file is located at [`/src/docs/game/create-a-game.md`](https://github.com/PlayGotm/docs/blob/main/src/docs/game/create-a-game.md), and the resulting chapter on Gotm is located at [`https://gotm.io/docs/game/create-a-game`](https://gotm.io/docs/game/create-a-game).

The source files are written in [Markdown](https://gotm.io/docs/markdown). You can preview Markdown while editing a source file on GitHub, in a [Markdown online editor](https://marked.js.org/demo/?version=master), or via your preferred text editor. Please keep in mind that [Gotm renders the Markdown differently](https://gotm.io/docs/markdown) from the previewers.

## Help wanted

The table below shows unwritten articles and chapters that are needed to make the documentation more complete.

The example scenarios in the table are suggestions for how the topic can be explained through an underlying theme. For example, it's easier to explain how leaderboards can be used for high scores by giving concrete examples that make sense to a game developer.

Everyone is welcome to come up with their own ideas for how a game developer can implement various game mechanics with the help of Gotm.

**Documentation that everyone is welcome to contribute (your own ideas are welcome too!)**

| Article | Chapter                                 | Example scenario                                                                                                      |
| ------- | --------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Content | Character progression                   | Use content to save a role-playing character's stats, level or skill tree.                                            |
|         | Characters and enemies                  | Use content to add/edit/delete enemies and their stats or abilities.                                                  |
|         | Destructible terrain like Minecraft     | Use content to store terrain chunks.                                                                                  |
|         | Dialogues                               | Use content to add/edit/delete dialogues.                                                                             |
|         | Downloadable content or expansion packs | Use content to release downloadable content.                                                                          |
|         | Drop tables                             | Use content to configure which items an enemy drops when defeated, and their drop rates.                              |
|         | Events and seasons                      | Use content to configure special events or seasons that are triggered by a time or manually by the developer.         |
|         | Game configuration and balancing        | Use content to configure a game and balance it by tweaking attributes of enemies, items or abilities.                 |
|         | Custom abilities                        | Use content to let players create dynamic abilities. Oblivion could be used as an example game                        |
|         | Skill trees                             | Use content to define skill trees and to save a player's progression in the tree.                                     |
|         | Game saves                              | Use content to store a player's game saves.                                                                           |
|         | Items                                   | Use content to add/edit/delete items of a game.                                                                       |
|         | Maps                                    | Use content to add/edit/delete maps of a game.                                                                        |
|         | Mods                                    | Use content to let players create mods and share them with other players.                                             |
|         | Player messages like Dark Souls         | Use content to let players leave messages for other players like they can in the game "Dark Souls".                   |
|         | Player profiles                         | Use content to let players create a profile with, for example, a picture and a description.                           |
|         | Quests                                  | Use content to add/edit/delete quests of a game.                                                                      |
|         | Synchronize data between devices        | Use content to synchronize some player data between devices.                                                          |
|         | Turn-based multiplayer games            | Use content to store the state of an ongoing match in a turn-based multiplayer game without needing peer connections. |

The table below lists documentation that is intended to be created by Gotm staff in the near future.

**Documentation that the Gotm staff intends to write**

| Article            | Chapter                  | Description                                                                                     |
| ------------------ | ------------------------ | ----------------------------------------------------------------------------------------------- |
| Content            | Limits                   | Explain the limitations of the content API.                                                     |
|                    | Performance optimization | Document performance caveats and ways to optimize the usage of the content API.                 |
|                    | API reference            | API details extracted from the plugin. Maybe include examples per function and property.        |
| Creator dashboard  |                          | Explain what it is and what you can do in it.                                                   |
| Game dashboard     |                          | Explain what it is and what you can do in it.                                                   |
| Game pack          |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Game promo         |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Game               |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Jam dashboard      |                          | Explain what it is and what you can do in it.                                                   |
| Jam                |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Playlist dashboard |                          | Explain what it is and what you can do in it.                                                   |
| Playlist           |                          | Explain what it is on Gotm is and when you encounter it.                                        |
| Product dashboard  |                          | Explain what it is and what you can do in it.                                                   |
| Product            |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Pulse              |                          | Explain what the Pulse premium plan is and how it works.                                        |
| Settings dashboard |                          | Explain what it is and what you can do in it.                                                   |
| Studio dashboard   |                          | Explain what it is and what you can do in it.                                                   |
| Studio             |                          | Explain what it is on Gotm and when you encounter it.                                           |
| User               |                          | Explain what it is on Gotm and when you encounter it.                                           |
| Revenue share      |                          | Explain how Gotm's revenue share model works in detail, and how it compares to other platforms. |
| Web player         |                          | Explain what the web player is and what you can do in it.                                       |

## Tips for writing good documentation

Writing good documentation that the reader can easily understand, search and navigate is hard. Here are some tips that may help you write good documentation.

### Guidelines

The documentation in this repository follows a set of guidelines that should be followed.

- Avoid acronyms unless absolutely necessary or if the term is only referred to by its acronym. For example, "role-playing game" should be used instead of "RPG" because not everyone knows what "RPG" means. On the other hand, using "HTML" is ok, because no one knows it as "HyperText Markup Language".
- Use "and" instead of "&".
- Capitalize words in a title as you would in a normal sentence. For example, capitalize your title like "How to make a Godot game" instead of "How to Make a Godot Game".

### The five Ws

Depending on the particular topic, the documentation on Gotm is targeted towards multiple types of readers, for example game developers, players, asset creators, streamers, social media influencers, and so on.

A helpful guideline when writing understandable documentation is to try to answer some or all of the "five Ws":

- Who?
- What?
- When?
- Where?
- Why?

The order of the five Ws varies and depends on what makes most sense for a particular topic. [Geoff Hart's article](https://techwhirl.com/the-five-ws-of-online-help/) explains how the five Ws can be used to help readers understand the documentation better. Below are some examples from the article:

- Who?

  - Who will use the product or function?
  - Who can help them perform tasks they can’t perform themselves?
  - Who else will be affected when they use a function or perform a task?

- What?

  - What features are available?
  - What can we do with a given feature?
  - What can we not do with a given feature?
  - What are the prerequisites for a feature, both in terms of knowledge and in terms of the steps that must be taken before using the feature?
  - What are the consequences of using a given feature?

- When?

  - When does this step take place? (Where does the step fall in a larger sequence of steps?)
  - When will the user be ready to do the step? (What are the prerequisites?)
  - When will users have the necessary knowledge to complete a step? (What must we teach them before they can proceed?)
  - When users have completed a task, is there any cleanup or followup required?

- Where?

  - Where will the product be used, and what are the consequences of that location?
  - Where do users find the menus, keystrokes, or toolbars to activate a feature?
  - Where is the product not applicable, and what can users do in that situation?
  - Where does the context limit the use of a feature?

- Why?

  - Why would someone use this feature?
  - Why would they use a different feature?
  - Why might this feature fail to work?
