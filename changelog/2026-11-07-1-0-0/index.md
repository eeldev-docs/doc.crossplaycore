---
title: CrossplayCore 1.0.0
slug: 1-0-0
---

First release of CrossplayCore.

## ADDED

- Added the CrossplayCore module with Steam and Epic Online Services (EOS) backends and Blueprint nodes for login, lobbies, friends, invites, presence, and matches.
- Added login with Steam through EOS Connect, Epic account login (Never, Launcher Only, or Always), optional Steam-to-Epic account linking with a Steam-only fallback, and Dev Auth Tool login for testing several players in PIE.
- Added typed lobby and member attributes with search filters for EOS and Steam lobbies.
- Added Steam lobbies, used when the lobby service is Platform Only or EOS is not available.
- Added match start over EOS P2P and Steam P2P, with lobby members traveling to the owner's match address.
- Added lobby invites, offline invites, Steam game invites, and joining from the Steam overlay, accepted EOS invites, or the `+crossplay_lobby` and `+connect_lobby` launch arguments. Pending joins are kept until login completes.
- Added the Epic friends overlay for lobby invites, with the Steam invite dialog as a fallback.
- Added Epic and Steam friends with Product User Id lookup, presence and rich presence text, and friend update and removal events.
- Added EOS lobby voice chat with Blueprint controls for muting, volume, and audio devices, and Online Subsystem voice chat for matches when lobby voice is unavailable.
- Added EOS achievement unlocking.
- Added the Crossplay Online Subsystem (`CROSSPLAY`) with Identity, Sessions, Friends, Presence, and Achievements, so the built-in session Blueprint nodes work on top of CrossplayCore.
- Added a host migration adapter for HostMigrationCore.
- Added support for the EOSCore and SteamCore PRO platforms and networking, including SteamCoreSockets.
- Added an option to use the EOS sandbox and deployment IDs supplied by the Epic Games launcher.
- Added configurable EOS friends and presence scopes.
- Added a setting that allows players whose net IDs come from another Online Subsystem to join crossplay matches.

## FIXED

- Fixed `OnLobbyMemberJoined` firing before the member's display name had arrived.
- Fixed Steam lobby joining by dispatching Steam callbacks to the game thread and keeping the platform user ID when the crossplay login fails.
- Steam lobby joins are now accepted when the Steam user is available, even if the EOS login fails.
- Lobbies and matches now recover after a process was killed and left its EOS lobby membership behind. Create and Join are refused while another lobby request is running.
- Host migration now detects a host that left the lobby, ignores host announcements from an earlier process, and no longer joins a match address that points at the local player.
- The current lobby is left when the subsystem shuts down.
- The game net driver is restored after a match, and a surviving EOS socket subsystem is registered again after shutdown.
- EOS APIs that need newer engine or EOS SDK versions are now guarded so the plugin builds on older versions.
