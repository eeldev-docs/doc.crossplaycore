---
title: CrossplayCore 1.2.0
slug: 1-2-0
date: '2026-11-26T22:09:30'
---

## ADDED

- Added the **Block Lobby Requests When Sanctions Unknown** setting, which rejects lobby requests when the login sanctions check cannot determine the player's sanctions.

## CHANGED

- Enabled Online Subsystem Steam and Steam Sockets as plugin dependencies on Windows, Linux, and macOS.
- Lobby update operations now report failures returned by Steam or Epic Online Services (EOS) to the caller.
- Added a Crossplay Blueprint example to the sample project.

## FIXED

- Fixed a race condition when destroying a session while a create, join, or update operation was still in progress. Session destruction now waits for the active operation to finish.
- Fixed pending login retries continuing after logout.
- Improved how Steam and EOS callbacks handle login, lobby membership, and session state changes.
- Fixed the same player's crossplay user IDs comparing as different when only one of them had a Product User Id.
- Fixed **Is Lobby Owner** returning true for a lobby without a valid owner.
- Fixed Crossplay Online Subsystem user IDs not converting correctly to and from bytes.
- Presence queries for known friends now return the cached presence immediately.
