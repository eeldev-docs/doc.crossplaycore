---
sidebar_position: 3
sidebar_label: FAQ & Troubleshooting
description: Answers to common questions and problems with the CrossplayCore plugin.
---

# FAQ & Troubleshooting

## Do I need SteamCore PRO or EOSCore?
No, but they are the recommended setup. CrossplayCore uses the Steamworks and EOS SDKs that ship with the engine,
so it also runs with the engine's Online Subsystems. With [SteamCore PRO](https://steamcore-pro.eeldev.com) and
[EOSCore](https://eoscore.eeldev.com), CrossplayCore uses their Steam and EOS platforms and networking, and you get
their full Blueprint APIs on the same login. See [Steam setup](docfiles/getting_started/steam-setup#with-steamcore-pro-recommended)
and [EOS setup](docfiles/getting_started/eos-setup#with-eoscore-recommended).

## Login fails with "Invalid session ticket"
EOS did not accept the Steam ticket. Check that **Steam** is an identity provider for EOS Connect in the Developer
Portal, with the Web API publisher key of the same Steam App ID the game runs with. Spacewar (480) does not work.
