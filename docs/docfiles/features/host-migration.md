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
3. Start matches with **StartCrossplayMatch** (or [Create Session](online-subsystem)) as usual.

:::caution
Don't use HostMigrationCore's Steam or EOS adapter with CrossplayCore. They expect Steam or EOS player ids, while the
players here have Crossplay ids, so no new host is found and the log shows `Candidate ... unavailable, skipping`.
:::

See the [HostMigrationCore documentation](https://hostmigrationcore.eeldev.com) for what gets saved and restored.
