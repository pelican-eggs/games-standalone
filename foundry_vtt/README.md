# Foundry VTT
[Foundry VTT](https://foundryvtt.com/) is a standalone application built for experiencing multiplayer tabletop RPGs using a feature-rich and modern self-hosted application where your players connect directly through the browser.

# Version 12 and below
This egg is only compatible with Foundry VTT version 13 and above. If you want to run an older version, please use the older egg script [here](https://github.com/pelican-eggs/games-standalone/tree/2b66489d43b50db60fbabf347da0fcfe030cca23/foundry_vtt).

# Installation
Foundry requires a license. In order to use this egg, you will need to purchase a foundry license, select the Node.js platform from your profile on the website, and then paste the "Timed URL" into the variable when seting up the server.

<img width="1281" height="794" alt="Screenshot of the FoundryVTT.com licenses page" src="https://github.com/user-attachments/assets/8aff7de6-2234-4a93-945b-2312f4e2b7ba" />

Note that this egg only runs the node application. You will need to manage TLS, reverse proxying, etc. on your own.

# Server Ports
This is a node application and only needs a single port that you will connect to over http(s)
