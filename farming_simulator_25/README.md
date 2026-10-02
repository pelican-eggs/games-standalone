# Farming Simulator 25

Farming Simulator 25 dedicated server egg for Pelican and Pterodactyl. It uses
the FS25-specific image from the Pelican yolks repository. The image contains
Wine 11, an XFCE desktop, TigerVNC and noVNC.

```text
ghcr.io/pelican-eggs/games:fs25
```

## Features

- FS25 installation and license activation through noVNC
- direct Wine startup command without a server-writable start script
- persistent game files, Wine prefix, configuration, mods and savegames
- automatic or interactive installation from EXE, IMG, ISO or ZIP
- sequential DLC installation with manual product-key entry
- configurable game, web-interface and noVNC ports
- optional automatic game-server start through the GIANTS web interface
- map selection through the GIANTS web interface

## Allocations

Assign three ports to the server:

| Purpose | Protocol | Default |
|---|---|---:|
| Game | TCP and UDP | `10823` |
| GIANTS web interface | TCP | `7999` |
| noVNC | TCP | `6080` |

The values of `SERVER_PORT`, `WEB_PORT` and `NOVNC_PORT` must match the
allocations assigned in the panel.

## Installation

1. Import `egg-farming-simulator-25.json` in the panel.
2. Assign the three ports listed above.
3. Upload the official FS25 installer (`.exe`, `.img`, `.iso` or `.zip`) to
   `/home/container/installer`.
4. Start the server.
5. Open noVNC:

   ```text
   http://SERVER-IP:6080/vnc.html?resize=remote&autoconnect=1
   ```

6. Open **Install / activate FS25** on the desktop. Archives and disk images
   are extracted automatically before the installer starts.
7. Enter the dedicated-server product key when the activation window opens.
8. During installation, Farming Simulator 25 is launched once to complete
   activation. This is expected and is the only time the full game is started
   by the installation process. Quit the game after activation, or restart the
   container if it remains open.
9. Restart the server after installation and activation have completed.
10. Delete the contents of `/home/container/installer` after a successful
    installation to reclaim the disk space used by the installer and extracted
    files.

A separate GIANTS dedicated-server license is required. The Steam edition does
not provide the required server key.

Set `AUTO_INSTALL=true` if an uploaded installer should launch automatically.
The default `INSTALL_MODE=silent` installs the game unattended and still opens
the activation program through noVNC afterwards.

## Server startup

`AUTOSTART_SERVER` supports three modes:

- `false`: keep the graphical desktop online and wait for manual actions.
- `web_only`: start the GIANTS web interface but not the game session.
- `true`: start the web interface and submit its game-server start form.

After installation, open the GIANTS Web Interface address printed in the
container console. The first game-server start from that interface can take
several minutes before the server becomes available. This longer delay occurs
only on the first start; later starts are faster.

## DLCs

Upload DLC installers named `FarmingSimulator25_*.exe`, `.img`, `.iso` or
`.zip` to `/home/container/dlc`. Either open **Install FS25 DLCs** on the
desktop or set:

```text
AUTO_INSTALL_DLC=true
```

DLC installers run one after another. Product keys are entered manually in
noVNC. DLCs already present in the persistent `pdlc` directory are skipped.

## Persistent paths

```text
Game:       /home/container/game/Farming Simulator 2025
Wine:       /home/container/.fs25server
Config:     /home/container/config/FarmingSimulator2025
Mods:       /home/container/config/FarmingSimulator2025/mods
Savegames:  /home/container/config/FarmingSimulator2025/savegame1
DLC data:   /home/container/config/FarmingSimulator2025/pdlc
Installers: /home/container/installer
DLC setup:  /home/container/dlc
Logs:       /home/container/logs
```

Upload mods as ZIP files without extracting them.

## Mods and savegames

Upload mod ZIP files without extracting them to:

```text
/home/container/config/FarmingSimulator2025/mods
```

Savegames are stored in numbered directories below the same persistent config
directory. For example, savegame slot `1` is:

```text
/home/container/config/FarmingSimulator2025/savegame1
```

To migrate a savegame, stop the server and upload the complete contents of the
existing `savegameN` directory into the matching destination directory. Set
`SAVEGAME_INDEX` to the same slot number, then start the server again.

## Map selection

Leave `SERVER_MAP` empty to select and retain the map in the GIANTS web
interface. Setting `SERVER_MAP` forces that `mapID` whenever the container
starts.

## Migration from the previous image

Back up the complete `/home/container` directory before changing images. The
new image keeps the existing `game`, `config`, `installer`, `dlc`, `logs` and
`.fs25server` paths. Old `.fs25-egg` and `.fs25-runtime` directories are no
longer executed and should only be removed after installation, activation and a
server restart have been verified.

## Disclaimer

This project is not affiliated with GIANTS Software. The game and DLCs must be
obtained legitimately from GIANTS and activated for the server.
