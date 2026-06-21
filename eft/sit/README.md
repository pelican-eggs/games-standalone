# Singleplayer Tarkov Server (Isn't maintained anymore at this time (use [Fika](https://github.com/project-fika/Fika-Server) instead))

## After server installation

Run the server once to generate the necessary config files for the coop mod, then edit `coopConfig.json` in `/home/container/user/mods/SITCoop/config` with your external IPv4.

## Client for connecting to the server

[Stay in Tarkov](https://github.com/stayintarkov/StayInTarkov.Client)

An Escape From Tarkov BepInEx module designed to be used with the SIT.Aki-Server-Mod with the ultimate goal of "Offline" Coop

To install the SIT client:
- Install the live Escape from Tarkov game from the official launcher.
- Install the [SIT Manager](https://github.com/stayintarkov/SIT.Manager.avalonia) from the repo.
- Afterwards, follow the instructions here [`SIT Manager Method`](https://docs.stayintarkov.com/en/install.html#).


## Server Ports

Ports required to run the server in a table format.

| Port                 | default |
|----------------------|---------|
| Game                 | 6969    |
| SIT Mod Websocket    | 6970    |
| Nat Helper WebSocket | 6971    |

## Server components

Installation script based on [SIT.Docker](https://github.com/stayintarkov/SIT.Docker).

[SPT-AKI Server](https://github.com/sp-tarkov/server) 

SPT is a modding framework for Escape From Tarkov. 

[SIT.Aki-Server-Mod](https://github.com/stayintarkov/SIT.Aki-Server-Mod)

A SPT-Aki mod to be used with SPT-Aki Server to allow the Coop Module to communicate with the SPT-Aki Server.
