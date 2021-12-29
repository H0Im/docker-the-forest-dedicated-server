## Docker - TheForest Dedicated Server
This includes a TheForest Dedicated Server based on Docker with Wine and an example config.

## What you need to run this
* Basic understanding of Linux and Docker

## Getting started
WARNING: If you dont do Step 1 and 2 your server can/will not save!
1. Create a new game server account over at https://steamcommunity.com/dev/managegameservers (Use AppID: `242760`)
2. Insert the Login Token in the config.cfg file (At `serverSteamAccount`)
3. Create 2 directories on your Dockernode (`/srv/tfds/steamcmd` and `/srv/tfds/game`)
4. Start the container with the following examples:

Bash:
```console
docker run --rm -i -t -p 8766:8766/tcp -p 8766:8766/udp -p 27015:27015/tcp -p 27015:27015/udp -p 27016:27016/tcp -p 27016:27016/udp -v /srv/tfds/steamcmd:/steamcmd -v /srv/tfds/game:/theforest --name the-forest-dedicated-server jammsen/the-forest-dedicated-server:latest
or
docker run --rm -i -t -p 8766:8766/tcp -p 8766:8766/udp -p 27015:27015/tcp -p 27015:27015/udp -p 27016:27016/tcp -p 27016:27016/udp -v $(pwd)/theforest/steamcmd:/steamcmd -v $(pwd)/theforest/game:/theforest --name the-forest-dedicated-server jammsen/the-forest-dedicated-server:latest
```
Docker-Compose:
```yaml
version: "3.7"
services:
  the-forest-dedicated-server:
    container_name: the-forest-dedicated-server
    image: jammsen/the-forest-dedicated-server:latest
    restart: unless-stopped
    ports:
        - 8766:8766/tcp
        - 8766:8766/udp
        - 27015:27015/tcp
        - 27015:27015/udp
        - 27016:27016/tcp
        - 27016:27016/udp
    volumes:
        - /srv/tfds/steamcmd:/steamcmd
        - /srv/tfds/game:/theforest
        
#Recommend to add a static IP to the docker container
#Below is an example for the 172 range    
    networks:
        forest_ip:
            ipv4_address: 172.26.0.2
networks:
  forest_ip:
    ipam:
      driver: default
      config:
          - subnet: 172.26.0.0/16
```

## Planned features in the future
* Automated checks for updates based on SteamCMD in a given interval 
* Automated updates based on checks
* ~~Wrapper around the render server error~~

## Software used
* Debian Slim Stable
* Supervisor
* Xvfb
* Winbind
* Wine
* SteamCMD
* TheForest Dedicated Server
