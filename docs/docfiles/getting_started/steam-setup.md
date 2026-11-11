---
sidebar_position: 3
title: Steam setup
---

# Steam setup

:::tip Recommended: SteamCore PRO
CrossplayCore works best together with **[SteamCore PRO](https://steamcore-pro.eeldev.com)**. SteamCore PRO starts
Steam for CrossplayCore, and its SteamCoreSockets carry matches in [Steam lobbies](../features/steam-lobbies) without
the engine's Steam plugins. Your project also gets SteamCore PRO's full Blueprint API for the Steamworks SDK, for
example stats, leaderboards, Workshop, Steam Inventory, microtransactions and dedicated servers.
[Get SteamCore PRO on Fab](https://www.fab.com/listings/d101542d-2534-4b2c-be16-e9b3e5cd4d04) or
[on Gumroad](https://eeldev.gumroad.com/l/steamcorepro).
:::

Steam is the platform: it tells CrossplayCore who the player is, and provides friends, rich presence, overlay
invites and achievements.

## Initialize Steam

CrossplayCore does not start Steam itself. Steam must be initialized by something that also runs the Steam
callbacks:

- **SteamCore PRO** (recommended, see below)
- **OnlineSubsystemSteam**, the engine's Steam plugin

When Steam is not running, for example in PIE or on a test machine, the Steam backend is not available and the plugin
falls back to the other [login methods](../features/login).

## With SteamCore PRO (recommended)

### 1. Enable the plugins

Enable **SteamCore PRO** and **CrossplayCore** in **Edit → Plugins**, and restart the editor. See
[Installing SteamCore PRO](https://steamcore-pro.eeldev.com/docfiles/getting_started/installing-plugin).

SteamCore PRO replaces the engine's Steam plugins. Turn off **Online Subsystem Steam** and **Steam Sockets** if they
are on. SteamCore PRO warns at startup when the engine's Steam plugins are enabled.

### 2. Configure SteamCore PRO

Add this to `Config/DefaultEngine.ini`, with your own Steam App ID:

```ini title="Config/DefaultEngine.ini"
[OnlineSubsystemSteamCore]
bEnabled=True
SteamDevAppId=480
SteamAppId=480
bUseSteamNetworking=True
bAllowP2PPacketRelay=True
bRelaunchInSteam=False

[OnlineSubsystem]
DefaultPlatformService=SteamCore
```

`bUseSteamNetworking` and `bAllowP2PPacketRelay` are off by default. Keep both on: CrossplayCore needs them for
matches in [Steam lobbies](../features/steam-lobbies#matches-over-steam-p2p), which run over SteamCoreSockets with
`steam.` addresses.

SteamCore PRO initializes Steam at startup, and CrossplayCore uses it from there. See
[Configuring SteamCore PRO](https://steamcore-pro.eeldev.com/docfiles/getting_started/configuring-plugin) for all
settings.

:::caution
Use your own App ID when you test crossplay. Spacewar (480) works for Steam itself, but EOS rejects its tickets, see
[Steam as an EOS identity provider](#steam-as-an-eos-identity-provider).
:::

### 3. Leave the net driver to CrossplayCore

Matches don't need the `SteamCoreSocketsNetDriver` lines from SteamCore PRO's own setup guide. When a match starts in a
[Steam lobby](../features/steam-lobbies), CrossplayCore switches the game net driver to `SteamCoreSocketsNetDriver`
by itself, and switches back afterwards. Matches in crossplay lobbies run over EOS P2P.

### 4. With the `Crossplay` Online Subsystem

If you use the [`Crossplay` Online Subsystem](../features/online-subsystem), make SteamCore PRO its native platform:

```ini title="Config/DefaultEngine.ini"
[OnlineSubsystem]
DefaultPlatformService=Crossplay
NativePlatformService=SteamCore
```

Together with [EOSCore](eos-setup#with-eoscore-recommended), this gives you one Steam plugin and one EOS plugin, both
shared with CrossplayCore.

## Steam as an EOS identity provider

For Steam players to get a Product User Id, add **Steam** as an identity provider in the Epic Developer Portal and
link it to your EOS Connect application. Steam needs your app's **Web API publisher key** for this.

The Steam backend asks Steam for a Web API ticket with the identity `epiconlineservices` and logs in to EOS Connect
with it. The first time a Steam account logs in, the plugin creates a Product User for it.

:::caution
The Steam identity provider must be set up for the same Steam App ID the game runs with. EOS rejects tickets from
another app, such as Spacewar (480), and the log shows
`EOS_Connect_ExternalTokenValidationFailed`. The player then logs in with a Device ID instead, or stays on Steam
only when **Allow Device Id Login** is off. See [When the EOS login fails](../features/login#when-the-eos-login-fails).
:::

## Steam P2P matches

Matches in [Steam lobbies](../features/steam-lobbies) run over Steam P2P: with SteamCore PRO over SteamCoreSockets,
with OnlineSubsystemSteam over the engine's **SteamSockets** plugin, which you then enable. See
[Matches over Steam P2P](../features/steam-lobbies#matches-over-steam-p2p).

## Next step

[Quick Start](quick-start).
