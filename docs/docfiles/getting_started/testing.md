---
sidebar_position: 6
title: Testing
---

# Testing

## Without Steam: Device ID

When Steam is not running, for example in PIE, **Allow Device Id Login** (on by default) logs in to EOS anonymously
with the computer's device id. The player gets a Product User Id and can create and join lobbies.

A device id belongs to the computer, so every PIE window and every game on one computer gets the **same** Product
User Id. That is enough for one player, but not for testing a lobby with several players on one computer. Use the
Dev Auth Tool for that.

## The Dev Auth Tool

The EOS Developer Authentication Tool logs in real Epic accounts, so each PIE window gets its own Product User Id.

1. Start the tool from the EOS SDK (`SDK/Tools/EOS_DevAuthTool`) on port `6547`.
2. Log in one Epic account per player and name the credentials `Player1`, `Player2`, `Player3` and `Player4`.
3. Turn on **Project Settings → Plugins → Crossplay → Login → Dev Auth → Use Dev Auth Tool**.
4. Play with several clients. PIE instance 0 logs in as `Player1`, instance 1 as `Player2`, and so on.

| Setting | Default | Description |
|---|---|---|
| Use Dev Auth Tool | Off | Log in with the Dev Auth Tool. |
| Dev Auth Host | `localhost:6547` | The address of the tool. |
| Dev Auth Credentials | `Player1` … `Player4` | The credential names, one per PIE instance. |

Standalone games pick a credential on the command line:

```bash
UnrealEditor.exe MyGame.uproject -game -CrossplayDevAuth=Player2
```

The **Epic Games** identity provider must be enabled for EOS Connect in the Developer Portal, and the client needs an
application linked to it. See [EOS setup](eos-setup).

:::note
- Dev Auth is never used in Shipping builds.
- With only **Use Dev Auth Tool** on, Steam still wins when it is running. A `-CrossplayDevAuth=<name>` on the command
  line wins over Steam.
:::

## Steam

Steam features need Steam running and one Steam account per game. A second game on the same account can't join the
first one's lobby. Test with two computers (or a computer and a virtual machine) with Steam accounts that are friends.

- Use your own Steam App ID, set with `SteamDevAppId` under `[OnlineSubsystemSteam]` in `DefaultEngine.ini`. With
  Spacewar (480), invites that start the game start Spacewar instead, and EOS rejects the Steam login.
- Steam starts the build that is installed for the App ID when a player accepts an invite while the game is not
  running. Upload the build to your App ID to test that.
- [Matches in Steam lobbies](../features/steam-lobbies#matches-over-steam-p2p) use SteamCoreSockets or SteamSockets,
  which do not run in the editor. Test them in standalone or packaged builds.
- To test how the game behaves without EOS, run it with an App ID that is not in the EOS Steam identity provider. The
  login fails with `EOS_Connect_ExternalTokenValidationFailed`, and
  [Steam lobbies, matches and voice keep working](../features/login#when-the-eos-login-fails).

## Logging

All log lines use the `LogCrossplay` category. A successful login writes `Logged in as` with the player's ids. For
more detail:

```ini
[Core.Log]
LogCrossplay=Verbose
```
