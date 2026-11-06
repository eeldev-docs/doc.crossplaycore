---
sidebar_position: 5
title: Lobbies
---

# Lobbies

When crossplay is active, lobbies are EOS lobbies that Steam players, Epic players and Device ID players all share.
Without EOS they are [Steam lobbies](steam-lobbies). The nodes are the same.

## Create

**Create Crossplay Lobby** creates a lobby and makes the player its owner.

| Setting | Default | Description |
|---|---|---|
| Max Members | `4` | 1 to 64. |
| Visibility | `Public` | See below. |
| Bucket | `default` | Searches only find lobbies in the same bucket. Use it for game mode, region or build version. |
| Attributes | Empty | Typed lobby attributes. See [Attributes](attributes). |

| Visibility | EOS | Steam |
|---|---|---|
| **Public** | Found by searches | Public lobby |
| **Friends Only** | Joinable through presence, not found by searches | Friends only lobby |
| **Invite Only** | Only with an invite | Private lobby |

## Find

**Find Crossplay Lobbies** searches with a **Crossplay Lobby Search**:

| Field | Default | Description |
|---|---|---|
| Bucket | `default` | The bucket to search in. |
| Filters | Empty | Typed filters, for example `skill >= 1000`. See [Search filters](attributes#search-filters). |
| Max Results | `20` | 1 to 200. |

## Join and leave

- **Join Crossplay Lobby** joins by **Lobby Id**, from a search result, an invite or a join request. It recognizes
  Steam lobby ids, so a Steam invite can be joined even when new lobbies are created in EOS.
- **Leave Crossplay Lobby** leaves the current lobby. When the owner leaves, EOS gives the lobby to another member.

A player is in one lobby at a time. **Create** and **Join** fail with **Invalid Parameters** ("Already in a lobby,
leave it first") while the player is in a lobby. A full lobby gives **Lobby Full**.

## The current lobby

| Node | Description |
|---|---|
| **Get Current Lobby** | The lobby the player is in, with its members and attributes. |
| **Is In Lobby** | Whether the player is in a lobby. |
| **Is Lobby Owner** | Whether the local player owns the lobby. |
| **Get Lobby Attribute** / **Get Member Attribute** | Read one attribute. |

A **Crossplay Lobby** has a **Lobby Id**, **Owner**, **Max Members**, **Num Members**, **Visibility**, **Crossplay**
(true for EOS lobbies), **Attributes**, **Members** and **Match Address** (set once a [match](match) has started).
Each member has a **User Id**, **Display Name** and **Attributes**.

## Events

Bind these on the **Crossplay Subsystem**:

| Event | When |
|---|---|
| **On Lobby Updated** | The lobby changed: attributes, members, owner or match address. Gives the whole lobby. |
| **On Lobby Member Joined** | A member joined. Fires once the member's display name has arrived, at most 5 seconds after the join. |
| **On Lobby Member Left** | A member left or was kicked. |
| **On Lobby Left** | The player is no longer in the lobby: they left, were kicked or the lobby was closed. |

A member who leaves before **On Lobby Member Joined** fired gives neither event.
