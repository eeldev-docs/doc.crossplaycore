---
sidebar_position: 5
title: How it works
---

# How it works

## Two backends

CrossplayCore runs two backends next to each other:

- The **platform backend** (Steam) knows who the player is. It provides the Steam session ticket for EOS, the friends
  list, rich presence, the overlay and achievements.
- The **service backend** (EOS) is the crossplay layer. It gives every player a **Product User Id** and holds the
  lobbies all platforms share.

Both are set in **Project Settings → Plugins → Crossplay → Backends**. The defaults are `CrossplayBackend_Steam` and
`CrossplayBackend_EOS`.

## The Crossplay Subsystem

The **Crossplay Subsystem** is a Game Instance subsystem. It is created with the GameInstance, so it lives across map
changes and needs no setup. It logs in to both backends and sends each call to the backend that serves it:

| Call | Goes to |
|---|---|
| Login | Steam for the ticket, then EOS Connect. See [Login](../features/login). |
| Lobbies | EOS when crossplay is active, otherwise Steam lobbies. Set with **Lobby Service**. |
| Friends and presence | Steam. EOS when the player is logged in with an Epic account. |
| Achievements | Steam. |
| Invites | The backend of the current lobby. |
| Invite overlay | Steam, for both EOS and Steam lobbies. |

**Crossplay is active** when the player is logged in and has a Product User Id. Check it with
**Is Crossplay Active**.

## Lobby service

**Project Settings → Plugins → Crossplay → Lobbies → Lobby Service** decides where lobbies are created and searched:

| Value | Lobbies |
|---|---|
| **Auto** (default) | EOS when crossplay is active, otherwise Steam lobbies. |
| **Crossplay** | Always EOS. Lobby calls fail when crossplay is not active. |
| **Platform Only** | Always Steam lobbies. See [Steam lobbies](../features/steam-lobbies). |

**JoinCrossplayLobby** recognizes Steam lobby ids whatever the setting, so a Steam invite can always be joined.

## Crossplay User Id

Every player is a **Crossplay User Id** with three fields:

| Field | Description |
|---|---|
| Product User Id | The EOS Product User Id. Empty when the player is not logged in to EOS. |
| Platform | How the player logged in: `Steam`, `Epic`, `DeviceId` or `DevAuth`. |
| Platform User Id | The SteamID or Epic account id. |

Two ids are equal when their Product User Ids are equal. Ids without a Product User Id are compared by platform and
platform user id.

## What each member writes to the lobby

Every member writes its display name, platform and ids to the lobby as attributes. That way every player sees the
others' names and platforms without extra lookups. These attributes start with `cpc_` and are hidden from the
attributes the plugin returns, so don't use that prefix for your own keys.

## One lobby at a time

A player is in at most one lobby. **Get Current Lobby** returns it, and **Is In Lobby** tells whether there is one.
Joining a lobby from a [join request](../features/invites#join-requests) leaves the current lobby first.
