# Witcher Online

Witcher Online is a multiplayer mod for the Witcher 3.

## Important Info

The server files can be downloaded from Nexus Mods only and thus have to be downloaded manually. Go to https://www.nexusmods.com/witcher3/mods/11590?tab=files and download the "Witcher Online Server" zip file. This file should be uploaded to the root of the server and then the reinstall button should be pressed to set up the files correctly.

## Links

- Mod page: https://www.nexusmods.com/witcher3/mods/11590
- GitHub: https://github.com/rejuvenate7/WitcherOnline
- Server documentation: https://rejuvenate.gitbook.io/witcheronline/guides/server-setup

## Server Ports

The Witcher Online server requires a single port for access.

| Port  | default |
|-------|---------|
| Game  | 40000   |

## Architecture support

This egg works natively on amd64 and through box64 on arm64. If you need to use the box64 version, change the selected docker image for a server to "Emulated with box64".
