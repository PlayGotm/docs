# Welcome to the official documentation for Gotm!

This repository contains the source material for the [official documentation for Gotm](https://gotm.io/docs). Anyone may improve or add to the documentation by creating a [pull request](https://github.com/PlayGotm/docs/pulls) or by reporting an [issue](https://github.com/PlayGotm/docs/issues).

## Report

If you find something that could be improved but don't have the means to do it yourself, please create an [issue](https://github.com/PlayGotm/docs/issues).

## Contribute

You can contribute to this repository by creating a [pull request](https://github.com/PlayGotm/docs/pulls) containing your changes.

The documentation is structured as a flat collection of articles. An article is a web page that explains what a specific concept is on Gotm. An article also contains chapters that explain how the concept is used in specific scenarios.

For example, the "game" article explains what a game on Gotm is. The article's chapters then explain how to create a game, how to embed it on other websites, how to configure a game page, and so on. Since the "game" article is about the game concept, its source file is located at [`/src/docs/game.md`](https://github.com/PlayGotm/docs/blob/main/src/docs/game.md), and the resulting article on Gotm is located at [`https://gotm.io/docs/game`](https://gotm.io/docs/game). Since the "create a game" chapter belongs to the "game" article, its source file is located at [`/src/docs/game/create-a-game.md`](https://github.com/PlayGotm/docs/blob/main/src/docs/game/create-a-game.md), and the resulting chapter on Gotm is located at [`https://gotm.io/docs/game#create-a-game`](https://gotm.io/docs/game#create-a-game).

The source files are written in [Markdown](https://gotm.io/docs/markdown). You can preview Markdown while editing a source file on GitHub, in a [Markdown online editor](https://marked.js.org/demo/?version=master), or via your preferred text editor. Please keep in mind that [Gotm renders the Markdown differently](https://gotm.io/docs/markdown) from the previewers.

## Writing guidelines

Depending on the particular topic, the documentation on Gotm is targeted towards multiple types of readers, for example game developers, players, asset creators, streamers, social media influencers, and so on.

To make sure that the documentation is as helpful as possible to the reader, all documentation should try to answer the "five Ws":

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
