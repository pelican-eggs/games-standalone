# Tiny Block Server

Open-source headless dedicated server for [Tiny Block](https://tinyblock.nosuchgames.com/), a living 2D sandbox game built with Godot 4.

The egg downloads a verified Linux x86_64 release from [GitHub Releases](https://github.com/hardtab/tinyblock-server/releases), publishes the running world in the official Tiny Block server browser, and stores world saves in the server's persistent home directory.

## Network

Tiny Block Server initiates outbound HTTPS and WebSocket connections, and attempts STUN/WebRTC for a lower-latency direct path. It does not require a fixed inbound game port. When ephemeral UDP is unavailable behind Docker or NAT, dedicated servers automatically fall back to the authenticated WebSocket relay.

## Variables

| Variable | Default | Description |
| --- | --- | --- |
| `VERSION` | `latest` | GitHub release tag, such as `v0.1.0`, or `latest`. |
| `WORLD_ID` | `world_community_1` | Stable save identifier. |
| `WORLD_NAME` | `Tiny Block Community` | Public name displayed in the server browser. |
| `WORLD_MODE` | `skyblock` | `skyblock`, `floating_islands`, `procedural`, `one_block`, or `challenge_run`. |
| `MAX_PLAYERS` | `16` | Server-declared capacity; any positive integer is accepted. |

World saves persist under `/home/container/.local/share/godot/app_userdata/Tiny Block Server/`.

