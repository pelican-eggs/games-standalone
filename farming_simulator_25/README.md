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

A separate GIANTS license is required. The Steam edition does
not work at the moment.
Steam Support will maybe added in a Future update.

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

## Panel variables and GIANTS settings

`SERVER_PORT` and `WEB_PORT` are always applied because they must match the
Pelican allocations. Optional game and web settings follow one consistent
rule:

- leave a variable empty to keep the value saved in the GIANTS Web Interface;
- enter a value to make the panel override that setting on every container
  start.

This applies to the server name, web login, game and admin passwords, player
limit, language, map, savegame slot, pause behavior, save interval, Web API
interval and crossplay. A new installation uses `admin` / `webpassword` for the
initial web login. Change those credentials in the GIANTS Web Interface.

When upgrading an existing server, clear the optional variables once in the
panel. Values already stored for an old egg remain explicit overrides until
they are emptied. The previous image's one-year Web API interval is migrated
once to 60 seconds when `SERVER_STATS_INTERVAL` is empty.

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
Game log:   /home/container/config/FarmingSimulator2025/log.txt
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
interface. Setting `SERVER_MAP` forces a built-in `mapID` whenever the
container starts. Select mod maps in the GIANTS Web Interface so GIANTS keeps
the matching map filename.

## Slow mod-map startup

FS25 checks every ZIP in the active mod directory before loading the selected
map and savegame. For a controlled test, keep only the mod map and its required
dependencies in `/home/container/config/FarmingSimulator2025/mods`, then
compare the first and second start with the same files. Do not extract mod ZIP
files.

Review `/home/container/config/FarmingSimulator2025/log.txt` for `Error:`
entries and compare the same save with a built-in map. Large maps can still
take substantially longer on the first load because map data, textures and
shaders must be prepared. The image preserves the Wine prefix and FS25 config
directory between restarts and does not clear game caches.

## Player disconnect and pause checks

For **Pause Game If Empty**, leave `SERVER_PAUSE` empty and enable the option in
the GIANTS Web Interface. The game allocation must be available on the same
port over TCP and UDP. `SERVER_STATS_INTERVAL` controls the Link XML/Web API
refresh interval, so a shorter value makes external player-status displays
update sooner; the game engine still owns the actual disconnect timeout.

When testing, verify both a normal quit and a dropped client connection. Check
the game log for the disconnect time and confirm that the Web Interface reports
zero players before evaluating pause behavior.

## Disclaimer

This project is not affiliated with GIANTS Software. The game and DLCs must be
obtained legitimately from GIANTS and activated for the server.
