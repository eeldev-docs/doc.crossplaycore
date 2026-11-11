---
sidebar_position: 1
slug: /
title: Introduction
---

# CrossplayCore

**Cross-platform multiplayer for Unreal Engine: one API for Steam and Epic Online Services.**

CrossplayCore gives you one Blueprint and C++ API for login, lobbies, friends, invites, presence and matches across
Steam and Epic Online Services (EOS). Steam players log in to EOS with their Steam account (EOS Connect), so Steam
players, Epic players and anyone else with a Product User Id meet in the same EOS lobbies.

No custom GameInstance, GameMode or Online Subsystem is required. The plugin uses the Steamworks and EOS SDKs that
ship with the engine, so it runs on its own.

:::tip Even better with SteamCore PRO and EOSCore
CrossplayCore uses the Steam and EOS platforms of [SteamCore PRO](https://steamcore-pro.eeldev.com) and
[EOSCore](https://eoscore.eeldev.com) when they are in the project. You configure Steam and EOS once, CrossplayCore's
matches run over their networking, and you get their full Blueprint APIs for stats, leaderboards, Workshop,
Player Data Storage and more on the same login. See [Steam setup](docfiles/getting_started/steam-setup) and
[EOS setup](docfiles/getting_started/eos-setup).
:::

:::tip
New to the plugin? Start with [Installing the plugin](docfiles/getting_started/installing-plugin), set up
[EOS](docfiles/getting_started/eos-setup) and [Steam](docfiles/getting_started/steam-setup), then follow the
[Quick Start](docfiles/getting_started/quick-start).
:::

## Features

| Feature | Description |
|---|---|
| **[Login](docfiles/features/login)** | Steam → EOS Connect, Epic accounts (launcher, saved login, account portal), Device ID and the Dev Auth Tool for PIE. |
| **[Crossplay lobbies](docfiles/features/lobbies)** | Create, find, join and leave EOS lobbies that every platform shares, with member events, kick, owner handover, settings changes and lobby chat. |
| **[Typed attributes](docfiles/features/attributes)** | String, Integer, Float and Boolean lobby and member attributes, and search filters that compare numbers. |
| **[Invites and join requests](docfiles/features/invites)** | In-game invites, offline invites, a list of pending invites, Steam overlay invites and starting the game from an invite. |
| **[Friends and presence](docfiles/features/friends-and-presence)** | Steam or Epic friends with their Product User Id, so they can be invited straight into a crossplay lobby. Friend requests, blocking, avatars and recent players. |
| **[Matches over P2P](docfiles/features/match)** | Start a listen server match over EOS P2P (or Steam P2P) from the lobby, and end it to return to the lobby. No ports, no `DefaultEngine.ini` changes. |
| **[Steam lobbies](docfiles/features/steam-lobbies)** | The same nodes on Steam lobbies, when EOS is not available or not wanted. |
| **[Host migration](docfiles/features/host-migration)** | Keep the match going when the host leaves, with [HostMigrationCore](https://hostmigrationcore.eeldev.com). |
| **[Online Subsystem](docfiles/features/online-subsystem)** | The engine's own Create / Find / Join Session nodes work on top of CrossplayCore. |

## How it fits together

A game runs two backends at the same time:

| Backend | Role | Provides |
|---|---|---|
| **Steam** (platform) | Who the player is | Steam session ticket for EOS, friends, rich presence, overlay invites, achievements |
| **Epic Online Services** (service) | The crossplay layer | Product User Id, lobbies every platform shares, in-game invites |

The **Crossplay Subsystem** (a Game Instance subsystem) logs in to both and sends each call to the backend that
serves it: lobbies go to EOS when crossplay is active, the friends list and achievements go to Steam. See
[How it works](docfiles/getting_started/how-it-works).

## Supported engine versions

| Engine | Status |
|---|---|
| Unreal Engine 5.5 | ✅ Supported |
| Unreal Engine 5.6 | ✅ Supported |
| Unreal Engine 5.7 | ✅ Supported |
| Unreal Engine 5.8 | ✅ Supported |

## Supported platforms

| Platform | Login, lobbies, friends, invites, voice chat | Matches over EOS P2P | Matches over Steam P2P |
|---|---|---|---|
| Windows (Win64) | ✅ | ✅ | ✅ |
| Mac | ✅ | ✅ | ✅ |
| Linux | ✅ | ❌ | ✅ |

On Linux, the engine has no EOS P2P socket subsystem (`SocketSubsystemEOS`), so Linux players can't host or join
matches over EOS P2P. Everything else works on Linux, including EOS lobbies and matches in
[Steam lobbies](docfiles/features/steam-lobbies#matches-over-steam-p2p). See
[Linux](docfiles/features/match#linux).
