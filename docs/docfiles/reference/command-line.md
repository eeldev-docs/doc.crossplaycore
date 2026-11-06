---
sidebar_position: 3
title: Command line
---

# Command line

| Argument | Description |
|---|---|
| `-CrossplayDevAuth=<name>` | Log in with this Dev Auth Tool credential. Wins over Steam. Needs **Use Dev Auth Tool**, not in Shipping. See [Testing](../getting_started/testing#the-dev-auth-tool). |
| `-AUTH_TYPE=exchangecode -AUTH_PASSWORD=<code>` | Passed by the Epic Games Launcher. Logs in with the player's Epic account unless **Epic Account Login** is **Never**. |
| `+crossplay_lobby <id>` | Passed by Steam when the game starts from an invite to an EOS lobby. Joins the lobby after login. |
| `+connect_lobby <id>` | Passed by Steam when the game starts from an invite to a Steam lobby. Joins the lobby after login. |

## Config overrides

Any setting can be changed for one run with the engine's `-ini:` argument, for example:

```bash
-ini:Game:[/Script/CrossplayCore.CrossplaySettings]:LobbyService=PlatformOnly
-ini:Engine:[OnlineSubsystem]:DefaultPlatformService=Crossplay
```
