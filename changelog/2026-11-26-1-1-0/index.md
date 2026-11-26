---
title: CrossplayCore 1.1.0
slug: 1-1-0
---

## ADDED

- Added stats, leaderboards, and achievement progress APIs for Steam and EOS, available through Blueprint and the Crossplay Online Subsystem.
- Added Quick Match, which looks for a lobby to join and creates one if no suitable lobby is available.
- Added platform restrictions for lobbies and an option to prevent players from joining a match in progress.
- Added lobby management and social APIs for updating lobby settings, kicking players, sending lobby chat messages, ending matches, and accessing avatars and recent players.
- Added EOS player reports, sanctions checks, and friend requests. Configured sanctions can block lobby creation, joining, and Quick Match.

## FIXED

- Fixed an issue where two players using Quick Match at the same time could create separate lobbies and remain split.
- Lobby creation, joining, and Quick Match now wait for the initial EOS sanctions check when lobby-blocking sanctions are configured.
- Successful lobby settings updates are now reflected immediately in the current lobby.
- Logging out now cancels pending login and lobby requests while preserving EOS logins managed by other plugins.
