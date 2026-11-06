---
sidebar_position: 2
title: Friends and presence
---

# Friends and presence

## Query Crossplay Friends

**Query Crossplay Friends** returns the player's friends. Each **Crossplay Friend** has:

| Field | Description |
|---|---|
| User Id | The friend's **Crossplay User Id**. **Product User Id** is filled in when the friend has logged in to the game with crossplay at least once. |
| Display Name | The friend's name. |
| Presence | **Offline**, **Online**, **Away** or **In Game**. |
| In This Game | True when the friend is playing this game. |

Friends with a Product User Id can be passed straight to **Send Crossplay Invite**. Friends without one have never
played the game with crossplay; invite them with **Show Invite Overlay**. See [Invites](invites).

If looking up the Product User Ids fails, the friends list is still returned, without them.

### Steam or Epic friends

| Player logged in with | Friends |
|---|---|
| Steam | The Steam friends list. **In This Game** is set for friends playing the same Steam app. |
| Epic account (launcher, account portal or Dev Auth) | The Epic friends list, with name and presence from EOS. **In This Game** is set for friends playing the same EOS product. Used even when Steam is running. |
| Device ID | No friends list. The node fails with **Not Supported**. |

## Set Crossplay Presence

**Set Crossplay Presence** sets the player's status text, for example `In the lobby` or `Playing Deathmatch`.

- **Steam:** the `status` rich presence key.
- **Epic account:** EOS presence with status Online and the text as rich text (at most 255 bytes).

## Resolve Crossplay Users

**Resolve Crossplay Users** looks up the Product User Id for your own lists of players, for example recent players
or a clan list. Pass users made with **Make Platform User Id** (a Steam or Epic account id), and it returns them with
**Product User Id** filled in for those who have logged in to the game with crossplay. Requires crossplay to be
active.
