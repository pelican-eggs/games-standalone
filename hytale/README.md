# Hytale Server - Pelican Panel Egg

This egg allows you to run a dedicated Hytale server on Pelican Panel.

## Requirements

- **Pelican Panel** installed and configured
- **Java 25** (included in the Docker image)
- **RAM**: Minimum 4GB, recommended 6-8GB or more depending on player count
- **Port**: UDP 5520 (default, configurable)

## Features

- ✅ Automatic installation via Hytale Downloader
- ✅ Java 25 support with AOT cache
- ✅ Dynamic memory configuration
- ✅ Customizable additional arguments
- ✅ Support for release and pre-release channels
- ✅ QUIC protocol over UDP

## Egg Installation

### 1. Import the Egg in Pelican Panel

1. Access your Pelican admin panel
2. Go to **Admin Panel** → **Nests**
3. Select or create a nest (e.g., "Minecraft" or create "Hytale")
4. Click **Import Egg**
5. Upload the `egg-hytale.json` file
6. Confirm the import

### 2. Configure the Node

Make sure your node has the required Docker image:

```bash
docker pull ghcr.io/pterodactyl/yolks:java_25
```

### 3. Create a Server

1. Go to **Servers** → **Create New**
2. Select the nest where you imported the egg
3. Select the "Hytale" egg
4. Configure resources:
   - **Memory**: Minimum 4096MB (4GB)
   - **Disk Space**: Minimum 10GB
   - **CPU**: 100% or more based on capacity
5. Configure the port (Default Allocation):
   - **Important**: Assign a UDP port (default 5520)
6. Complete the rest of the information and create the server

## Configuration

### Environment Variables

| Variable | Description | Default Value |
|----------|-------------|---------------|
| `SERVER_MEMORY` | RAM in MB | `4096` |
| `ASSETS_PATH` | Path to Assets.zip file | `Assets.zip` |
| `ADDITIONAL_ARGS` | Additional arguments | `` |
| `AUTO_DOWNLOAD` | Auto download (1=yes, 0=no) | `1` |
| `PATCHLINE` | Version channel (release/pre-release) | `release` |

### Useful Additional Arguments

You can add these to the `ADDITIONAL_ARGS` variable:

```bash
--disable-sentry                    # Disable crash reports
--backup --backup-frequency 60      # Backups every 60 minutes
--auth-mode offline                 # Offline mode (testing only)
```

## First Start - Authentication

⚠️ **IMPORTANT**: The server requires authentication on first start.

### Authentication Process

1. Start the server
2. In the console you'll see a message like:
   ```
   ===================================================================
   DEVICE AUTHORIZATION
   ===================================================================
   Visit: https://accounts.hytale.com/device
   Enter code: ABCD-1234
   ===================================================================
   ```
3. Open the URL in your browser
4. Enter the displayed code
5. Authorize the server with your Hytale account
6. The server will authenticate automatically

### Console Authentication

You can also execute from the panel console:

```bash
/auth login device
```

## Network Configuration

### Firewall and Ports

Hytale uses **QUIC over UDP** (not TCP):

```bash
# Make sure to open the UDP port in your firewall
sudo ufw allow 5520/udp
```

### Port Forwarding

If the server is behind a router, configure **UDP Port Forwarding**:
- External port: 5520 (or your chosen port)
- Internal port: 5520
- Protocol: **UDP** (not TCP)

## File Structure

Once installed, the server will have this structure:

```
/home/container/
├── HytaleServer.jar          # Server executable
├── HytaleServer.aot          # AOT cache for fast startup
├── Assets.zip                # Game asset files
├── config.json               # Main configuration
├── permissions.json          # Player permissions
├── whitelist.json            # Whitelist
├── bans.json                 # Banned players
├── .cache/                   # Server cache
├── logs/                     # Server logs
├── mods/                     # Installed mods
└── universe/                 # Worlds and player data
    └── worlds/               # Individual worlds
```

## Installing Mods

1. Download mods from [CurseForge](https://www.curseforge.com/hytale)
2. Upload `.jar` or `.zip` files to the `mods/` folder
3. Restart the server

## Optimization

### RAM Usage

RAM usage depends primarily on:
- **View Distance**
- Number of players
- Loaded world area

**Recommendations**:
- 4-6 players: 4-6GB
- 10-20 players: 8-12GB
- 20+ players: 16GB+

### View Distance

Limit view distance to reduce RAM usage:
- Recommended value: **12 chunks (384 blocks)**
- Hytale default: 24 chunks (768 blocks)

You can install the [Nitrado:PerformanceSaver](https://github.com/nitrado/hytale-plugin-performance-saver) plugin for dynamic management.

## Recommended Plugins

| Plugin | Description |
|--------|-------------|
| [Nitrado:WebServer](https://github.com/nitrado/hytale-plugin-webserver) | Base web server for APIs |
| [Nitrado:Query](https://github.com/nitrado/hytale-plugin-query) | Server status via HTTP |
| [Nitrado:PerformanceSaver](https://github.com/nitrado/hytale-plugin-performance-saver) | Dynamic optimization |
| [ApexHosting:PrometheusExporter](https://github.com/apexhosting/hytale-plugin-prometheus) | Detailed metrics |

## Troubleshooting

### Server won't start

1. Verify you have at least 4GB of RAM assigned
2. Check that the `Assets.zip` file exists
3. Review logs in the `logs/` folder

### Players can't connect

1. Verify the UDP port is open (not TCP)
2. Check port forwarding if behind a router
3. Make sure the server is authenticated
4. Verify client and server have the same version

### Authentication error

If you lose authentication:

```bash
/auth login device
```

And follow the process again.

### Version mismatch

If there's a game update:

1. Stop the server
2. Execute from the panel console:
   ```bash
   ./hytale-downloader -check-update
   ./hytale-downloader
   ```
3. Restart the server

## Limits and Considerations

- **Server limit**: 100 servers per Hytale license
- **Authentication required**: Cannot run without authentication (except offline mode for testing)
- **Protocol**: Must match exactly between client and server

## Support and Resources

- [Official Hytale Server Manual](https://support.hytale.com/hc/en-us/articles/45326769420827)
- [Pelican Panel Documentation](https://pelican.dev/docs/)
- [Hytale Modding Community](https://www.curseforge.com/hytale)
- [GitHub - Pelican Eggs](https://github.com/pelican-eggs/yolks)

## Additional Notes

### AOT Cache

The server includes a pre-trained AOT cache (`HytaleServer.aot`) that significantly improves startup times. It's automatically activated with the `-XX:AOTCache=HytaleServer.aot` argument.

### Automatic Backups

Enable backups by adding to `ADDITIONAL_ARGS`:
```
--backup --backup-frequency 30 --backup-dir backups
```

### Plugin Development

During development, disable Sentry:
```
--disable-sentry
```

---

**Egg Version**: 1.0.1
**Hytale Version**: Early Access
**Last Updated**: 2026-01-13
