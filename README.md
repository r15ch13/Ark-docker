# ARK: Survival Evolved - Docker Cluster

Docker build for managing an __ARK: Survival Evolved__ server cluster.

This image uses [Ark Server Tools](https://github.com/FezVrasta/ark-server-tools) to manage an ark server and is forked from [boerngen-schmidt/Ark-docker](https://hub.docker.com/r/boerngenschmidt/ark-docker/).

*If you use an old volume, get the new arkmanager.cfg in the template directory.*

__Don't forget to use `docker pull r15ch13/arkcluster` to get the latest version of the image__

## Features
 - Easy install (no steamcmd / lib32... to install)
 - Easy access to ark config file
 - Mods handling (via Ark Server Tools)
 - `docker stop` is a clean stop
 - Auto upgrading of arkmanager

## Usage
Fast & Easy cluster setup via docker compose:

```yaml
version: "3.5"

services:
  island:
    image: r15ch13/arkcluster:latest
    deploy:
      mode: global
    environment:
      CRON_AUTO_UPDATE: "0 */3 * * *"
      CRON_AUTO_BACKUP: "0 */1 * * *"
      UPDATEONSTART: 1
      BACKUPONSTART: 1
      BACKUPONSTOP: 1
      WARNONSTOP: 1
      USER_ID: 1000
      GROUP_ID: 1000
      TZ: "UTC"
      MAX_BACKUP_SIZE: 500
      SERVERMAP: "TheIsland"
      SESSION_NAME: "ARK Cluster TheIsland"
      MAX_PLAYERS: 15
      RCON_ENABLE: "True"
      RCON_PORT: 32330
      GAME_PORT: 7777
      QUERY_PORT: 27015
      SERVER_PASSWORD: ""
      ADMIN_PASSWORD: "keepmesecret"
      SPECTATOR_PASSWORD: "keepmesecret"
      MODS: "731604991"
      CLUSTER_ID: "keepmesecret"
      KILL_PROCESS_TIMEOUT: 300
      KILL_ALL_PROCESSES_TIMEOUT: 300
    volumes:
      - server_island:/ark
      - cluster:/cluster
    ports:
      - "32330:32330/tcp"
      - "7777:7777/udp"
      - "7778:7778/udp"
      - "27015:27015/udp"

  island:
    image: r15ch13/arkcluster:latest
    deploy:
      mode: global
    environment:
      CRON_AUTO_UPDATE: "15 */3 * * *"
      CRON_AUTO_BACKUP: "15 */1 * * *"
      UPDATEONSTART: 1
      BACKUPONSTART: 1
      BACKUPONSTOP: 1
      WARNONSTOP: 1
      USER_ID: 1000
      GROUP_ID: 1000
      TZ: "UTC"
      MAX_BACKUP_SIZE: 500
      SERVERMAP: "Valguero_P"
      SESSION_NAME: "ARK Cluster Valguero"
      MAX_PLAYERS: 15
      RCON_ENABLE: "False"
      RCON_PORT: 32331
      GAME_PORT: 7779
      QUERY_PORT: 27016
      SERVER_PASSWORD: ""
      ADMIN_PASSWORD: "keepmesecret"
      SPECTATOR_PASSWORD: "keepmesecret"
      MODS: "731604991"
      CLUSTER_ID: "keepmesecret"
      KILL_PROCESS_TIMEOUT: 300
      KILL_ALL_PROCESSES_TIMEOUT: 300
    volumes:
      - server_valguero:/ark
      - cluster:/cluster
    ports:
      - "32331:32331/tcp"
      - "7779:7779/udp"
      - "7780:7780/udp"
      - "27016:27016/udp"

volumes:
  server_island:
  server_valguero:
  cluster:
```

## Volumes
+ __/ark__ : Working directory :
    + /ark/server : Server files and data.
    + /ark/log : logs
    + /ark/backup : backups
    + /ark/arkmanager.cfg : config file for Ark Server Tools
    + /ark/crontab : crontab config file
    + /ark/server/ShooterGame/Saved/Config/LinuxServer/Game.ini : ark Game.ini config file
    + /ark/server/ShooterGame/Saved/Config/LinuxServer/GameUserSetting.ini : ark GameUserSetting.ini config file
    + /ark/template : Default config files
    + /ark/template/arkmanager.cfg : default config file for Ark Server Tools
    + /ark/template/crontab : default config file for crontab
    + /ark/staging : default directory if you use the --downloadonly option when updating.
+ __/cluster__ : Cluster volume to share with other instances

## Known issues
Currently none
