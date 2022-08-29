# GDGotm

Game developers can use GDGotm to implement many features for their Godot game using GDScript.

GDGotm is a free and [open-source](https://github.com/PlayGotm/GDGotm) Godot plugin, and it doesn't require a Gotm account when in [local mode](#local-mode-for-offline-or-single-player-games).

GDGotm contains the following features:

###### GDGotm features

<!-- prettier-ignore -->
| Feature | What can it be used for? | Can it be used for games not hosted on Gotm? | Does it support offline or single-player games? | Does it support online or multi-player games? |
| --- | --- | --- | --- | --- |
| [Content](/src/docs/content.md) | [Mods](content/mods.md), [marketplace](content/marketplace.md), [crafting](content/crafting.md), and [more](/src/docs/content.md). | Yes | [Yes](/src/docs/content/mix-local-and-global-contents.md) | Yes | Yes |
| [Leaderboard](/src/docs/leaderboard.md) | High scores, ranks, leagues, seasons, score distribution graphs. |  Yes | [Yes](/src/docs/leaderboard/mix-local-and-global-scores.md) | Yes | 
| [Lobby](/src/docs/lobby.md) | [Matchmaking](lobby/matchmaking.md), discover players and establish peer-to-peer connections. | LAN only | | Yes |

GDGotm supports Godot version 3.1.0 and newer.

# Install

1. [Download the plugin](https://github.com/PlayGotm/GDGotm/archive/refs/heads/master.zip) and extract the contents into your Godot project. You can also download the plugin via [Godot's asset library](https://docs.godotengine.org/en/stable/tutorials/assetlib/using_assetlib.html#in-the-editor) by searching for `GDGotm`.

1. Add the `gotm/Gotm.gd` file to your project's [autoloads](https://docs.godotengine.org/en/stable/tutorials/scripting/singletons_autoload.html#autoload). Make sure the autoload is named `Gotm`. It must be named `Gotm` for it to work.

# Initialize

You must initialize GDGotm before you can use it in your game.

###### Initialize Gotm

```gdscript
Gotm.initialize()
```

GDGotm is in [local mode](#local-mode-for-offline-or-single-player-games) by default. To enable online mode, see [Global mode for online or multi-player games](#global-mode-for-online-or-multi-player-games).

# Local mode for offline or single-player games

GDGotm is in local mode by default and doesn't require an internet connection or a Gotm account.

While in local mode, [all features](#gdgotm-features) that support offline or single-player games will create any new data locally. Local data is stored locally on the player's device and is not accessible to other players and devices.

Features that do not support offline or single-player games do not work in local mode.

# Global mode for online or multi-player games

GDGotm is in [local mode](#local-mode-for-offline-or-single-player-games) by default. To enable global mode you must have a Gotm account.

While in global mode, [all features](#gdgotm-features) that support online or multi-player games will create any new data globally. Global data is stored on Gotm's servers and is accessible to other players and devices.

Features that do not support global mode but supports local mode will continue to create any new data locally.

You can still [create local data](#create-local-data-in-global-mode) in global mode.

If a player is temporarily disconnected from the internet, GDGotm will try to continue working by using cached data.

You can enable global mode by providing a project key when initializing GDGotm. You can create a project key in your [game's dashboard](/dashboard/_/_?page=tools&highlight=project-key).

###### Initialize Gotm with project key

```gdscript
var config = GotmConfig.new()
# Replace the empty string with your project key.
config.project_key = ""
Gotm.initialize(config)
```

# Create local data in global mode

While GDGotm is in [global mode](#global-mode-for-online-or-multi-player-games), any newly created data is global by default.

However, [every feature](#gdgotm-features) that supports offline or single-player games has a function that lets you create local data even when in global mode. Local data is stored locally on the player's device and is not accessible to other players or devices. Global data is stored on Gotm's servers and is accessible to other players and devices.

You can also [disable global mode](#disable-global-mode) completely for a particular feature.

You can learn more by clicking the links in the [feature table's](#gdgotm-features) offline games support column.

# Disable global mode

When you enable [global mode](#global-mode-for-online-or-multi-player-games), all supported features are made global.

If you want a feature to remain in local mode, you can disable global mode for that particular feature. You can learn more by clicking the links in the [feature table's](#gdgotm-features) offline games support column.

# Enable detailed logs

When you need to debug or troubleshoot issues related to GDGotm, you can enable verbose logs to be printed in Godot's console.

###### Initialize Gotm with logs

```gdscript
var config = GotmConfig.new()
config.enable_logs = true
Gotm.initialize(config)
```

# Issues

You can find help via the [contacts page](/contacts). If you encounter any bugs, please create an issue in [GDGotm's repository](https://github.com/PlayGotm/GDGotm).

# Contribute

Everyone is welcome to improve GDGotm by contributing to its [source code](https://github.com/PlayGotm/GDGotm).
