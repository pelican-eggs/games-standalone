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
container console. The Web Interface can be ready while the game server is
still loading. `FS25 image ready.` indicates container initialization, not a
loaded map. Large mod maps can take several minutes on any start, not only
the first one. Wait until the game server is available before trying to join.

## Terminal access

Open **Terminal** on the noVNC desktop or in the applications menu. The image
uses XTerm with an interactive Bash shell, without reusing a D-Bus terminal
process. **Alt+F2** opens the application finder; enter `xterm` to open another
terminal. Installation and DLC shortcuts use the same terminal and retain
their output after finishing.

The updated image repairs XFCE's terminal preference and desktop/menu launchers
before the desktop starts, preserving other preferences and keeping a one-time
`.bak` copy of changed files. Pull the rebuilt image and restart the container.
No egg reimport or game reinstallation is required. Existing mods, savegames,
GIANTS settings and the Wine prefix remain unchanged.

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

Measure from clicking **Start** in GIANTS until the game server is joinable.
Keep the same FS25 version, DLCs, complete mod set, selected map and a copy of
the same savegame for the Windows/Linux comparison. Compare at least three
runs, separating the first load from subsequent loads. CPU and memory limits
are ceilings, not targets that every loading stage must reach.

After pulling an image containing the updated runtime, open a terminal in
noVNC while the game server is loading and run:

```text
/opt/fs25/fs25ctl.py diagnose --seconds 10
```

The existing image controller reports Wine version, CPU affinity, CPU cgroup
limits/throttling counters, process/thread CPU and disk I/O, process open-file
limits/counts and the largest i3d timings from the game-log tail. It runs only when
invoked and does not rewrite settings, restart Wine or change game data.
Log timings can be from earlier loading stages. Repeat the command if the
game process starts after the first PID lookup. Save the output together with
`/home/container/config/FarmingSimulator2025/log.txt` for diagnosis.

The runtime uses the game directory for the GIANTS server process and applies
headless Wine options before Wine starts, even with an existing prefix. It
raises low open-file soft limits to at most 65,536 without exceeding the hard
limit or lowering an existing higher limit. These changes address runtime
inconsistencies; a speed improvement must still be measured with your mod set.
Existing servers need the updated image, not an egg reimport, for these changes.

### Wine runtime and synchronization

The updated image builds Wine-Proton 11 from pinned source against the Pelican
base image's 64-bit libraries, with Wine 11 WoW64 support for both 32-bit and
64-bit Windows programs and the existing `win64` prefix format. It retains
the existing WineHQ 11 runtime as an explicit compatibility option. The image
build tests prefix initialization and both Windows command interpreters.

The default `proton`/`auto` selection checks `futex_waitv` and usable shared
memory in a disposable child before Wine starts. FSYNC does not need a new
host kernel, privileged mode or `/dev/ntsync` on supporting containers. If
the host/container does not support it, blocks the syscall or fails the
shared-memory check, FSYNC is disabled and startup continues. Wine may prefer
NTSync if its device is already accessible. The image does not change the
kernel, Docker device mappings or Wings configuration. ESYNC is not included
in this Wine branch. Acceleration and Windows-equivalent loading times are
not guaranteed; compare measurements with the same mod set and savegame.

`diagnose` reports the inherited request separately from observed process file
descriptors. Look for `FSYNC shared-memory descriptor observed` or `NTSync
device descriptor observed` while the map loads. A requested flag or a message
in an old log alone does not establish the current backend.

No egg reimport is required. To change the selection on an existing server,
stop the container, create `/home/container/config/wine-runtime.json` using
the Pelican file manager, and restart the container:

```json
{"runtime": "proton", "sync": "auto"}
```

`runtime` accepts `proton` (default) or `stable` (WineHQ 11). `sync` accepts
`auto` (default), `fsync` (try FSYNC only, otherwise fall back), or `server`
(disable accelerated backends in Wine-Proton). Non-empty `FS25_WINE_RUNTIME`
and `FS25_WINE_SYNC` environment variables override the file's respective
values. The choice is inherited by the desktop, installers and GIANTS; changes
require a full container restart, not only a GIANTS game restart.

For an A/B test, compare `proton`/`auto` with `proton`/`server`, restarting the
container between tests. Use `stable`/`server` with the matching pre-upgrade
prefix/configuration backup if the new runtime causes a compatibility regression.
Keep the old image digest and data backup until join/rejoin, save/load, settings
retention, installation/activation, DLCs and noVNC terminal access are verified.

Wine is LGPL-2.1-or-later. The pinned source archive, license notices, authors
and the sole source overlay (`VERSION.fs25`) are included in
`/opt/fs25/wine-source`. The version `11.0-fs25-proton-dc26e61` identifies the
runtime separately from WineHQ 11. Wine's normal prefix-update mechanism remains
in use; runtime selection does not reset activation or game settings.
The Dockerfile verifies the source
archive checksum and uses generic CPU targets rather than `-march=native`.
See the [pinned Wine-Proton source](https://github.com/ValveSoftware/wine/tree/dc26e61847081a1b5cb0733dc30feba6ee575482)
and its [FSYNC implementation](https://github.com/ValveSoftware/wine/blob/dc26e61847081a1b5cb0733dc30feba6ee575482/server/fsync.c).

Review the complete game log for `Error:` entries. Isolate optional mods only
on a separate test copy after checking savegame dependencies. Use a new savegame
for a built-in-map reference; do not switch the map of an existing mod-map save.
Do not extract mod ZIPs or delete game caches. The image preserves the Wine
prefix and FS25 config directory between restarts.

Before an image/Wine upgrade, stop the container and back up the prefix,
configuration and savegame, and record the old image digest. Test join/rejoin,
save/load, stop/restart and retained GIANTS settings after the update. Restore
the previous image and its matching backup if a regression occurs.

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
