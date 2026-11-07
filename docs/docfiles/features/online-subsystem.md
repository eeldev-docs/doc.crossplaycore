---
sidebar_position: 8
title: Online Subsystem
---

# Online Subsystem (built-in session nodes)

CrossplayCore also registers an Online Subsystem named `Crossplay`, so the engine's own nodes and code that uses
`IOnlineSubsystem::Get()` work on top of it: **Create Session**, **Find Sessions**, **Join Session**,
**Destroy Session** and the player's unique net id.

You don't need it to use CrossplayCore. It is for projects and plugins that already use the engine's session nodes.

## Turning it on

```ini title="Config/DefaultEngine.ini"
[OnlineSubsystem]
DefaultPlatformService=Crossplay
NativePlatformService=Steam
```

`Crossplay` must be the **default** platform service: the engine only accepts player ids of the same type as the
default Online Subsystem. The native service (Steam) is loaded next to it.

## Hosting and joining

- **Host:** **Create Session**, then **Open Level** with `?listen`. The match address is written to the lobby when
  the map opens, so **StartCrossplayMatch** is not needed.
- **Client:** **Find Sessions**, then **Join Session**. The node travels to the host over EOS P2P.

## How sessions map to lobbies

- One session at a time: the session is the current crossplay lobby.
- Session settings become [lobby attributes](attributes). Integers are stored as int32 and floats as float, and
  settings marked **Dont Advertise** stay on the host.
- Set the search bucket with the `CROSSPLAYBUCKET` setting, on the session and on the search.
- The build id is stored in the lobby, and search results from another build are skipped.
- **Login** and **Auto Login** run **Crossplay Login**.
- Player ids are Crossplay ids (the Product User Id, or `Steam:<id>` without one), and they replicate on the
  PlayerState. When the EOS login failed but Steam is running, the player gets the `Steam:<id>` id, so they can still
  join matches.
- The voice interface is the engine's VoIP, see [Voice chat](voice-chat#matches-engine-voip).

## Not included yet

Friends, presence, achievements and more than one session are not part of the Online Subsystem yet. Use the
Crossplay nodes for them.
