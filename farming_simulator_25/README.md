# Farming Simulator 25 – Pelican/Pterodactyl Egg

An egg for an FS25 dedicated server using the Docker image
[`toetje585/arch-fs25server:latest`](https://github.com/wine-gameservers/arch-fs25server).

## Features

- FS25 installation and license activation through noVNC
- Wine 11 and headless audio configuration
- automatic sequential DLC installation with manual product-key entry
- freely configurable game, web, and noVNC ports
- persistent mods, savegames, DLCs, and license data
- map selection through the GIANTS web interface

## Installation

1. Import [`egg-farming-simulator-25.json`](./egg-farming-simulator-25.json) in
   the Pelican or Pterodactyl admin panel.
2. Assign the ports:
   - primary: `10823` TCP/UDP – game port
   - additional: `7999` TCP – GIANTS web interface
   - additional: `6080` TCP – noVNC
3. Upload the FS25 IMG, ZIP, or EXE file to `/home/container/installer`.
4. Start the server and open noVNC:

   ```text
   http://SERVER-IP:6080/vnc.html?resize=remote&autoconnect=1
   ```

5. Open **Install / activate FS25** and enter the server key.

A separate GIANTS server license is required. The Steam version does not
include a compatible server key.

## DLCs

Upload DLC installers named `FarmingSimulator25_*.exe`, `.img`, `.iso`, or
`.zip` to `/home/container/dlc`, then set the following in the panel:

```text
AUTO_INSTALL_DLC=true
```

The installers open one after another in noVNC. DLCs that are already installed
are skipped; product keys must still be entered manually.

## Mods and Savegames

```text
Mods:      /home/container/config/FarmingSimulator2025/mods
Savegames: /home/container/config/FarmingSimulator2025/savegame1
DLCs:      /home/container/config/FarmingSimulator2025/pdlc
Logs:      /home/container/logs
```

Upload mods as ZIP files and do not extract them.

## Map Selection

Leave `SERVER_MAP` empty if the map should be selected in the GIANTS web
interface. The saved `mapID` and `mapFilename` remain in place across container
restarts. Setting `SERVER_MAP` forces the server to use that map.

## Disclaimer

This project is not affiliated with GIANTS Software. The game and DLCs must be
obtained legitimately from GIANTS and activated for the server.
