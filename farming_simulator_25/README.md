# [Farming Simulator 25](https://www.farming-simulator.com/)

Dedicated server for the GIANTS edition of Farming Simulator 25 on Linux amd64. This egg uses the Pelican-built `ghcr.io/pelican-eggs/games:fs25` image with GE-Proton. The runtime source is reviewed and built in [Pelican Yolks](https://github.com/pelican-eggs/yolks/tree/master/games/fs25); the original development project is [DasEric/fs25-docker](https://github.com/DasEric/fs25-docker). The egg does not pull the personal image.

## Requirements

- A GIANTS product key for the server. This egg does not use SteamCMD.
- About 70 GiB of free disk space for the initial download, extraction and installation.
- The Pelican FS25 image must be available to Wings.

## Server ports

| Purpose | Protocol | Default | Allocation |
| --- | --- | ---: | --- |
| Game | UDP | 10823 | Primary (`SERVER_PORT`) |
| GIANTS web interface | TCP | 8080 | Additional; match `FS25_WEB_PORT` |
| Optional GIANTS TLS | TCP | 8443 | Additional; match `FS25_TLS_PORT` |

## Installation

1. Import the egg and assign the game and web allocations.
2. Set **GIANTS Product Key** under the server's admin Startup settings. Optionally set **GIANTS IMG Download Link** to a direct official `.img` link; otherwise the image looks up the download in the GIANTS portal.
3. Start the server. The first start installs and activates FS25 before opening the GIANTS web interface. Later starts reuse the existing installation. **Autostart Game** controls whether a game session starts automatically or waits for you to start it in the GIANTS web interface.
4. For TLS, place `cert.pem` and `pk.pem` in `game/`, add the TLS allocation and enable TLS in the egg variables.

Set a web admin password in `.fs25-secrets/web-admin-password`, or read the generated password from `game/dedicatedServer.xml` on the node. Set secrets per server, never as egg defaults. Wings may log environment variables during installation; to keep keys out of those logs, leave the panel fields blank and put the settings in the server volume under `.fs25-secrets/`. Use files `activation-key`, `download-url`, `dlc-product-keys` and `dlc-install-sources` as needed, mode `0600`, owned by the server UID.

## DLCs and updates

Set **DLC Product Keys** as `name=KEY;other=KEY`. If needed, set **DLC Installation Sources** with the same names: `name=direct-GIANTS-URL;other=installer.img`. Sources can be official `.exe`/`.img` links or files uploaded under `dlc-installers/`. A missing source is looked up in the GIANTS portal. Already verified DLC installations are reused; removing an entry does not uninstall it.

Back up the volume before an update. **Reinstall** refreshes the base-game and DLC download checks without intentionally replacing the persistent `profile/`, which holds savegames, mods and DLC data.
