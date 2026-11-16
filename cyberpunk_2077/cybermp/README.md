# CyberMP

[CyberMP](https://cyber.mp/) is a multiplayer mod for Cyberpunk 2077.

The egg downloads the Linux server build from [CyberMP-Server-Releases](https://github.com/Cyber-MP/CyberMP-Server-Releases/releases) and runs it on the stock `ghcr.io/pelican-eggs/yolks:debian` image. Required shared libraries (libssl1.1, nng, c-ares, libcurl-gnutls) are bundled into `./libs` during install.

Documentation: https://docs.cyber.mp

## Server Ports

TCP and UDP use the same port.

| Port | default |
|------|---------|
| Game | 2077    |

## Variables

| Variable | Default | Description |
| --- | --- | --- |
| `SERVER_VERSION` | `latest` | Release tag such as `v4.0.4-beta.1`, or `latest`. Reinstall to apply. |
| `PUBLIC_ADDRESS` | empty | Public IP/hostname clients connect to (`connection_host_addr`). Required for the server list. |
| `CYBERMP_MAX_PLAYERS` | `20` | Maximum player slots. |
| `CYBERMP_PRIVATE_TOKEN` | empty | Masterlist token. Leave empty for private servers. |
| `CYBERMP_DISABLE_VOICE` | `false` | Disable the built-in VoIP server. |
| `CYBERMP_EP2_DLC_ENABLED` | `false` | Enable Phantom Liberty content. |

Other settings, including the `resources` list, are configured in `server.toml`. It is kept across reinstalls.
