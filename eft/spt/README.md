# Escape From Tarkov - SPT

## From their github [sp-tarkov server](https://github.com/sp-tarkov/server)

SPT is a modding framework for Escape From Tarkov. 

## Known issues

When starting the server without wine, some mods can throw an error about them not finding a module.
You can resolve this by going into `/user/mods/modfolder/src/mod.js` and looking through the file. You need to find the import string, it could look something like that 
`const ConfigTypes_1 = require("C:/snapshot/project/obj/models/enums/ConfigTypes")`, in this case you would need to get rid of `C:` like that 
`const ConfigTypes_1 = require("/snapshot/project/obj/models/enums/ConfigTypes")`.

## Server Ports

You should let the ip in the config file `SPT_Data/Server/configs/http.json` be `0.0.0.0` else you will have an error with ports.

Ports required to run the server in a table format. (You can use any port)

| Port    | default |
|---------|---------|
| Game    | 6969    |
