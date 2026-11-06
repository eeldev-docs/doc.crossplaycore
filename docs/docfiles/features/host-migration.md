---
sidebar_position: 3
title: Host migration
---

# Host migration

The **CrossplayCore - Host Migration** plugin connects CrossplayCore to
[HostMigrationCore](https://hostmigrationcore.eeldev.com), so a match started from a crossplay lobby keeps going when
the host leaves or crashes.

It is a separate plugin that needs both CrossplayCore and HostMigrationCore, so neither of them depends on the other.

## Setup

1. Enable **CrossplayCore - Host Migration** and **HostMigrationCore**.
2. Set **Project Settings → Plugins → Host Migration → Adapter Class** to **Crossplay Lobby**. With
   `DefaultPlatformService=Crossplay`, an empty Adapter Class picks it too.
3. Start matches with **Start Crossplay Match** (or [Create Session](online-subsystem)) as usual.

:::caution
Don't use HostMigrationCore's Steam or EOS adapter with CrossplayCore. They expect Steam or EOS player ids, while the
players here have Crossplay ids, so no new host is found and the log shows `Candidate ... unavailable, skipping`.
:::

See the [HostMigrationCore documentation](https://hostmigrationcore.eeldev.com) for what gets saved and restored.

## How it works

When the host is gone, HostMigrationCore picks the new host, and the adapter uses the current crossplay lobby
(EOS or Steam) to find it:

- Every member writes its player id to the lobby, so candidates are found whatever Online Subsystem the player ids
  come from.
- The new host announces its address in its member attributes, and the others travel there over the same P2P network.
- The lobby is handed to the new host, and the lobby's match address moves to it, so players who join later go to the
  new host. Players who already followed it do not travel again.

The adapter's lobby keys start with `hm_`.

It works in EOS and Steam lobbies, and also when the EOS login failed and the players only have Steam ids.

:::note
The migration starts when the host's connection is lost, or when the host is no longer in the lobby, whichever comes
first. A host that quits leaves the lobby at once. When a host crashes, EOS removes it from the lobby after about 30
seconds, so in an EOS lobby the migration starts about 20–35 seconds after the crash.
:::

A player who crashed can start the game again and rejoin the lobby. It then travels to the new host and gets its saved
state back, as long as it returns within the state's keep time.
