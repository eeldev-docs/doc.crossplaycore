---
sidebar_position: 1
title: Installing the plugin
---

# Installing the plugin

## Install from Fab

1. Open the **Epic Games Launcher** and go to **Unreal Engine → Library → Fab Library**.
2. Search for **CrossplayCore**.
3. Click **Install to Engine** and select your engine version (**5.5** to **5.8**).

## Enable the plugin

Open your project, go to **Edit → Plugins**, search for **CrossplayCore** and make sure it is enabled. Restart the
editor when asked. The plugin enables the engine plugins it needs: **EOSShared**, **OnlineSubsystem**,
**OnlineSubsystemUtils** and, on Windows and Mac, **SocketSubsystemEOS**. The engine has no SocketSubsystemEOS for
Linux, see [Linux](../features/match#linux).

For host migration, also enable **CrossplayCore - Host Migration** and **HostMigrationCore**. See
[Host migration](../features/host-migration).

## Installing manually (project plugin)

You can also place the plugin in your project instead of the engine:

```text
YourProject/
└── Plugins/
    ├── CrossplayCore/
    │   ├── CrossplayCore.uplugin
    │   ├── Config/
    │   ├── Resources/
    │   └── Source/
    └── CrossplayHostMigration/      (optional)
```

Right click your `.uproject` → **Generate Visual Studio project files**, then build your project.

## Modules

| Module | Type | Contents |
|---|---|---|
| `CrossplayCore` | Runtime | Types, the Crossplay Subsystem, settings, Blueprint nodes and the backend base class. |
| `CrossplayCoreSteam` | Runtime | The Steam backend (platform). Uses the engine's Steamworks SDK. |
| `CrossplayCoreEOS` | Runtime | The EOS backend (crossplay service) and the EOS settings. Uses the engine's EOS SDK. |
| `OnlineSubsystemCrossplay` | Runtime | The `Crossplay` Online Subsystem. See [Online Subsystem](../features/online-subsystem). |
| `CrossplayHostMigration` | Runtime | The **Crossplay Lobby** host migration adapter (separate **CrossplayCore - Host Migration** plugin). |

C++ projects that call the API add `CrossplayCore` to their module's dependencies:

```csharp
PublicDependencyModuleNames.AddRange(new string[] { "CrossplayCore" });
```

## Next step

[Set up Epic Online Services](eos-setup).
