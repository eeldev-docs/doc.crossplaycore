---
sidebar_position: 1
title: Plugin settings
---

# Plugin settings

The settings are in **Edit → Project Settings → Plugins**, and are saved to `Config/DefaultGame.ini`.

## Crossplay

Saved under `[/Script/CrossplayCore.CrossplaySettings]`.

### Backends

| Setting | Default | Description |
|---|---|---|
| Platform Backend Class | `CrossplayBackend_Steam` | The platform backend: who the player is, friends, overlay, achievements. |
| Service Backend Class | `CrossplayBackend_EOS` | The crossplay service: Product User Id and shared lobbies. |

See [How it works](../getting_started/how-it-works).

### Lobbies

| Setting | Default | Description |
|---|---|---|
| Lobby Service | `Auto` | Which lobbies CreateCrossplayLobby and FindCrossplayLobbies use. `Auto`: EOS lobbies when logged in to EOS, otherwise Steam lobbies. `Crossplay`: only EOS lobbies, so players on every platform meet. `Platform Only`: only Steam lobbies, so only Steam players meet, and there is no lobby voice chat. It does not change the login: the backends decide that. |
| Auto Accept Join Requests | On | Join the lobby by itself on a [join request](../features/invites#join-requests). Turn off to call **JoinCrossplayLobby** yourself. |

### Match

| Setting | Default | Description |
|---|---|---|
| Auto Join Match | On | Members travel to the owner by themselves when the [match](../features/match) starts. Turn off to call **Join Match** yourself. |
| Accept Other Platforms In Match | On | Let players with ids from another Online Subsystem into a match hosted from a crossplay lobby. The engine otherwise rejects them with `incompatible_unique_net_id`. |

### Voice

| Setting | Default | Description |
|---|---|---|
| Voice Input Starts Muted | Off | Join [voice chat](../features/voice-chat) with the microphone muted. |

### Moderation

| Setting | Default | Description |
|---|---|---|
| Query Sanctions On Login | On | Fetch the player's active [sanctions](../features/moderation#sanctions) after login. |
| Lobby Blocking Sanctions | Empty | Sanction actions that keep a player from creating, joining or quick matching, for example `RESTRICT_MATCHMAKING`. |

### Login

| Setting | Default | Description |
|---|---|---|
| Epic Account Login | `Launcher Only` | When players log in with their Epic account: `Never`, `Launcher Only` or `Always`. See [Epic accounts](../features/login#epic-accounts). |
| Allow Device Id Login | On | Log in to EOS anonymously when Steam is not running, or when EOS rejects the Steam login (for example Spacewar, 480). See [Device ID](../features/login#device-id). |
| Device Id Display Name | Empty | The display name for Device ID logins. Empty uses the computer's name. |

### Login | Dev Auth

| Setting | Default | Description |
|---|---|---|
| Use Dev Auth Tool | Off | Log in with the EOS Developer Authentication Tool. Never in Shipping. See [Testing](../getting_started/testing#the-dev-auth-tool). |
| Dev Auth Host | `localhost:6547` | The address of the tool. |
| Dev Auth Credentials | `Player1`, `Player2`, `Player3`, `Player4` | The credential name for each PIE instance. |

## Crossplay - Epic Online Services

Saved under `[/Script/CrossplayCoreEOS.CrossplayEOSSettings]`.

:::info With EOSCore
When [EOSCore](../getting_started/eos-setup#with-eoscore-recommended) is in the project, CrossplayCore uses EOSCore's
EOS platform and its settings in **Project Settings → Plugins → EOSCore Plugin**. The platform settings below
(**Create Platform If Missing**, the ids, **Client Secret** and **Enable Voice Chat**) are then ignored, even when they
are filled in. The scope and lobby settings still apply.
:::

| Setting | Default | Description |
|---|---|---|
| Create Platform If Missing | On | Create the EOS platform when no other plugin has created one. |
| Product Id | Empty | From the Epic Developer Portal. |
| Sandbox Id | Empty | From the Epic Developer Portal. |
| Deployment Id | Empty | From the Epic Developer Portal. |
| Client Id | Empty | From the Epic Developer Portal. |
| Client Secret | Empty | From the Epic Developer Portal. |
| Request Friends Scope | On | Ask for the player's Epic friends at Epic login. Needs the Friends permission in the client policy. See [Scopes](../features/login#scopes). |
| Request Presence Scope | On | Ask for the player's Epic presence at Epic login. Needs the Online Presence permission in the client policy. |
| Lobby Presence | On | Tie EOS lobbies to the player's Epic presence, so Epic friends can join from the social overlay. Only for players logged in with an Epic account. |
| Enable Voice Chat | On | Create the EOS platform with voice for [lobby voice chat](../features/voice-chat#eos-lobbies). |

When **Create Platform If Missing** is on and the ids are set, the plugin only reuses an existing EOS platform with the
same Product Id and Client Id. Platforms created by other plugins for other products (for example the editor's Fab
plugin) are skipped.

See [EOS setup](../getting_started/eos-setup).

## Example

```ini title="Config/DefaultGame.ini"
[/Script/CrossplayCore.CrossplaySettings]
LobbyService=Auto
bAutoAcceptJoinRequests=True
bAutoJoinMatch=True
bAcceptOtherPlatformsInMatch=True
bVoiceInputStartsMuted=False
EpicAccountLogin=LauncherOnly
bAllowDeviceIdLogin=True
bUseDevAuthTool=False

[/Script/CrossplayCoreEOS.CrossplayEOSSettings]
bCreatePlatformIfMissing=True
ProductId=...
SandboxId=...
DeploymentId=...
ClientId=...
ClientSecret=...
bRequestFriendsScope=True
bRequestPresenceScope=True
bLobbyPresence=True
bEnableVoiceChat=True
```
