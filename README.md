# GPTWOL a simple Wake On Lan gui

---
[![Docker Pulls](https://img.shields.io/docker/pulls/misterbabou/gptwol.svg?logo=docker)](https://hub.docker.com/r/misterbabou/gptwol)
[![GitHub Release](https://img.shields.io/github/release/Misterbabou/gptwol.svg?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/releases/latest)
[![GitHub last commit](https://img.shields.io/github/last-commit/Misterbabou/gptwol?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/commits/main)
[![MIT Licensed](https://img.shields.io/github/license/Misterbabou/gptwol.svg?logo=github&logoColor=959DA5)](https://github.com/Misterbabou/gptwol/blob/main/LICENSE.md)
---

GPTWOL is a simple and lightweight Wake on Lan gui made with python to wake up your computers on your LAN.
It was made mostly by chatGPT.

## Screenshot 

![gptwol-gui.png](/assets/gptwol-gui.png)

## Features 

- Docker Image to deploy
- Send Wake On Lan packets
- Add or Delete Computer
- Computers status check with ping or tcp request (timeout settings available)
- Very low power usage (20 mb RAM)
- Check if IP and MAC provided are valid
- cron job to wake up device
- Check if Cron provided is valid
- Search on cumputer Name, MAC or IP

## Special configuration you can change

- Ping Refresh to check Status availibility 
- Disable Delete or Add Computers
- Change the port of the Web UI

## Docker Configuration
> [!NOTE]
>
>It's recommanded to use docker compose to run this application. [Install documentation](https://docs.docker.com/compose/install/)

> [!CAUTION]
>
>- The app container needs to run in host network mode to send the wakeonlan command on your local network.
>- Make sure that the PORT you are using is free on your host computer
>- Make sure that BIOS settings and remote OS is configure to allow Wake On Lan
>- Don't expose gptwol directly on internet without proper authentication

### With docker compose

Create `docker-compose.yml` file:
```
services:
  gptwol:
    container_name: gptwol
    image: misterbabou/gptwol:latest
    network_mode: host
    restart: unless-stopped
    environment:
      - PORT=8080 #Free Port on Your host; default is 5000
      - TZ=Europe/Paris #Set your timezone for Cron; default is UTC
      #- DISABLE_ADD_DEL=1 #Uncomment this line to disable Add or delete Computers; default is to allow
      #- DISABLE_REFRESH=1 #Uncomment this line to prevent your browser to refresh Computer status; default is to allow
      #- REFRESH_PING=15 # Uncomment this line to change ping status check, can be 15 or 60 (seconds); default value is 30 seconds
      #- PING_TIMEOUT=200 #Uncomment this line to change the time to wait for a ping answer in (in ms); default value is 300 milliseconds
    volumes:
      - ./computers.txt:/app/computers.txt
      - ./appdata/cron:/etc/cron.d
```

Create the file for storing computers (the mounted file on docker-compose)
```
touch computers.txt
```

Run the application
```
docker compose up -d
```

### With docker

Create the file for storing computers (the mounted file on docker command)
```
touch computers.txt
```

Run the application
```
docker run -d \
  --name=gptwol \
  --network="host" \
  --restart unless-stopped \
  -e PORT=8080 \
  -e TZ=Europe/Paris \
  -v ./computers.txt:/app/computers.txt \
  -v ./appdata/cron:/etc/cron.d \
  misterbabou/gptwol:latest
```

## Roadmap 

:heavy_check_mark: Add ARM version (Added in 1.0.1)

:heavy_check_mark: Add feature to plan automatic Wake on Lan (Cron) (Added in 1.0.3)

:heavy_check_mark: Add Search feature (Added in 1.0.4)

:heavy_check_mark: Remove Cron on Computer deletion (Added in 1.0.4)

:heavy_check_mark: Improve load page performance due to ping timeout. (added in 1.0.5)

- Add filter buttons to filter computer by Name or IP

:heavy_check_mark: Add a TCP port option to check availibility without using ICMP (added in 2.0.1)

- Add OIDC Authentication (will require time)

- moove computers.txt in an other directory not to mount a file but a directory to the docker container
