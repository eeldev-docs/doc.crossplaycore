---
sidebar_position: 9
title: Steam lobbies
---

# Steam lobbies

With **Project Settings → Plugins → Crossplay → Lobbies → Lobby Service** set to **Platform Only**, or when EOS is not
available, lobbies are Steam lobbies. The same nodes and events work:

- Create, Find, Join and Leave Crossplay Lobby
- typed lobby and member attributes, and search filters
- invites: **Send Crossplay Invite** with a Steam user, **On Invite Received**, **Show Invite Overlay**
- member joined and left events, and join requests from the Steam overlay
- **Start Crossplay Match** over Steam P2P

**Join Crossplay Lobby** recognizes Steam lobby ids, so a Steam lobby invite can be joined even when new lobbies are
created in EOS.

Steam lobbies do not need EOS. They keep working when the EOS login fails, see
[When the EOS login fails](login#when-the-eos-login-fails).

## Differences from EOS lobbies

- Steam only searches on the bucket and **Equal** filters. The other filters (Greater, Less, Not Equal, floats) are
  applied to the results, so put an Equal filter in the search when there are many lobbies.
- Only the lobby owner can set lobby attributes.
- **Crossplay** is false on the lobby, and members have no Product User Id unless they are also logged in to EOS.
- No EOS voice room. Voice works in the match with the engine's VoIP, see [Voice chat](voice-chat).
- Steam stores only strings. The plugin stores the attribute types next to the values, so they come back with the
  type they were set with.

## Matches over Steam P2P

In Steam lobbies, **Start Crossplay Match** runs over Steam P2P instead of EOS P2P.

1. Use [SteamCore PRO](../getting_started/steam-setup#with-steamcore-pro-recommended), or enable the engine's
   **SteamSockets** plugin with OnlineSubsystemSteam.
2. The plugin switches the game net driver to `SteamCoreSocketsNetDriver` (SteamCore PRO) or `SteamSocketsNetDriver`
   when the match starts, and writes `steam.<SteamID>:<port>` to the lobby as the match address.

SteamSockets needs **OnlineSubsystemSteam** to be the active Steam subsystem. SteamCoreSockets needs SteamCore PRO
and these two settings, which are off by default:

```ini title="Config/DefaultEngine.ini"
[OnlineSubsystemSteamCore]
bUseSteamNetworking=True
bAllowP2PPacketRelay=True
```

`bUseSteamNetworking` starts SteamCoreSockets, and `bAllowP2PPacketRelay` makes it use `steam.` addresses instead of
IP addresses. Without them the match can't start.

Neither runs in the editor, so test Steam matches in standalone or packaged builds.
**Start Crossplay Match** gives **Not Supported** when neither is available.

Matches over Steam P2P also work on Linux, where [matches over EOS P2P](match#linux) don't.
