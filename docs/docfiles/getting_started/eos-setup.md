---
sidebar_position: 2
title: EOS setup
---

# Epic Online Services setup

:::tip Recommended: EOSCore
CrossplayCore works best together with **[EOSCore](https://eoscore.eeldev.com)**. CrossplayCore uses EOSCore's EOS
platform for login, lobbies, voice chat and P2P matches, so you configure EOS in one place. Your project also gets
EOSCore's full Blueprint API on the same platform, for example stats, leaderboards, Player Data Storage, sessions and
the Epic account services.
[Get EOSCore on Fab](https://www.fab.com/listings/8e78818d-4662-4c6d-8ab2-6e22b9d3595e) or
[on Gumroad](https://eeldev.gumroad.com/l/eoscore).
:::

EOS is the crossplay layer: it gives every player a **Product User Id** and holds the lobbies all platforms share.

## Developer Portal

In the [Epic Developer Portal](https://dev.epicgames.com/portal), for your product:

1. Create a **Client** with a client policy, and note the **Product Id**, **Sandbox Id**, **Deployment Id**,
   **Client Id** and **Client Secret**.
2. Under **Identity Providers**, add the providers your players log in with, and link them to your **EOS Connect**
   application:
   - **Steam**, for Steam players. See [Steam setup](steam-setup).
   - **Epic Games**, for [Epic accounts](../features/login#epic-accounts) and the
     [Dev Auth Tool](testing#the-dev-auth-tool).
3. For Epic accounts, allow the Epic Account Services scopes **Basic Profile**, **Friends List** and **Presence**.
4. For [reports and sanctions](../features/moderation), allow **Reports** and **Sanctions** in the client policy.

## With EOSCore (recommended)

With EOSCore enabled, CrossplayCore does not create an EOS platform of its own. At login it creates EOSCore's Online
Subsystem if nothing else has, and uses EOSCore's platform and configuration.

:::info EOSCore's settings are used, not CrossplayCore's
With EOSCore, the EOS platform is configured in **Project Settings → Plugins → EOSCore Plugin** only. CrossplayCore
ignores these settings in **Project Settings → Plugins → Crossplay - Epic Online Services**, even when they are
filled in:

- **Create Platform If Missing**, **Product Id**, **Sandbox Id**, **Deployment Id**, **Client Id**, **Client Secret**
  and **Use Launcher Deployment**. The keys come from EOSCore's EOS configuration.
- **Enable Voice Chat**. Voice depends on **Enable RTC** in EOSCore's EOS configuration.

Leave them empty, so the keys are only in one place. The other Crossplay settings, such as the Epic account scopes
and **Lobby Presence**, still apply.
:::

### 1. Enable the plugins

Enable **EOSCore** and **CrossplayCore** in **Edit → Plugins**, and restart the editor. See
[Installing EOSCore](https://eoscore.eeldev.com/docfiles/getting_started/installing_plugin).

### 2. Add your EOS keys to EOSCore

Open **Project Settings → Plugins → EOSCore Plugin**, add an entry under **EOS Configurations** with the keys from the
Developer Portal, and set **Default Configuration Name** to its name. Keep **Enable RTC** on if you use
[voice chat](../features/voice-chat).

The settings are saved to `Config/DefaultEngine.ini`:

```ini title="Config/DefaultEngine.ini"
[/Script/OnlineSubsystemEOSCore.EOSCoreSettings]
DefaultConfigurationName=MyGame
+EOSConfigurations=(ConfigName="MyGame",ProductId="...",SandboxId="...",DeploymentId="...",ClientId="...",ClientSecret="...",bEnableRTC=True)

[EOSSDK]
bHasProjectBinary=true
```

`bHasProjectBinary=true` makes the engine's EOS modules use the EOS SDK that ships with EOSCore, so CrossplayCore and
EOSCore never load two different SDK versions. See the
[EOSCore settings reference](https://eoscore.eeldev.com/docfiles/settings_reference) for all settings.

### 3. Leave the net driver to CrossplayCore

Crossplay matches don't need the `NetDriverEOSCore` lines from EOSCore's own setup guide. When a
[match](../features/match) starts, CrossplayCore switches the game net driver to EOS P2P on EOSCore's platform, and
switches back afterwards.

### 4. Choose the default Online Subsystem

Keep the platform as the default Online Subsystem, for example `DefaultPlatformService=SteamCore` with
[SteamCore PRO](steam-setup#with-steamcore-pro-recommended). If you use the engine's session nodes, use the
[`Crossplay` Online Subsystem](../features/online-subsystem) instead:

```ini title="Config/DefaultEngine.ini"
[OnlineSubsystem]
DefaultPlatformService=Crossplay
NativePlatformService=SteamCore
```

At login the log shows `Creating the EOSCore online subsystem to use its EOS platform`, and when the first match
starts, `using it for P2P as a borrowed platform`.

## Without EOSCore

Open **Project Settings → Plugins → Crossplay - Epic Online Services** and fill in:

| Setting | Description |
|---|---|
| Create Platform If Missing | Create the EOS platform when no other plugin has created one. On by default. |
| Product Id | From the Developer Portal. |
| Sandbox Id | From the Developer Portal. |
| Deployment Id | From the Developer Portal. |
| Client Id | From the Developer Portal. |
| Client Secret | From the Developer Portal. |

The settings are saved to `Config/DefaultGame.ini`.

## Where the EOS platform comes from

CrossplayCore looks for an EOS platform in this order:

1. A platform from `IEOSPlatformHandleProvider`, the same contract HostMigrationCore and EOSCore use.
2. A platform the engine's EOS SDK manager already has, for example from OnlineSubsystemEOS.
3. EOSCore's platform, after creating EOSCore's Online Subsystem, when EOSCore is enabled.
4. Its own platform, created from the settings above, when **Create Platform If Missing** is on.

[Matches over EOS P2P](../features/match) work on all of them.

## Next step

[Set up Steam](steam-setup).
