---
title: CrossplayCore 1.2.0
slug: 1-2-0
date: '2026-11-26T22:09:30'
---

## CHANGED

- Enabled Online Subsystem Steam and Steam Sockets as plugin dependencies on Windows, Linux, and macOS.
- Lobby update operations now report failures returned by Steam or Epic Online Services (EOS) to the caller.
- Added a Crossplay Blueprint example to the sample project.

## FIXED

- Fixed a race condition when destroying a session while a create, join, or update operation was still in progress. Session destruction now waits for the active operation to finish.
- Fixed pending login retries continuing after logout.
- Improved how Steam and EOS callbacks handle login, lobby membership, and session state changes.
