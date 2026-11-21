# Valheim Dedicated Server - Docker-compose Setup

## Table of Contents

- [About](#about)
- [Assumptions](#assumptions)
- [What this README will not cover](#what-this-guide-will-not-cover)
- [Setup](#setup)
- [Deployment](#deployment)
- [Further Documentation](#further-documentation)

## About

This repository contains a simple docker-compose setup for a Valheim server. The docker image being used is [lloesche/valheim-server-docker](https://github.com/lloesche/valheim-server-docker).

## Assumptions

When setting up and deploying this Valheim server, this README will assume you set up, downloaded or have the following:
 - A local computer/machine or server that can run Docker 
 - Docker installed with docker-compose support (we recommend installing Docker Desktop)
 - Ports forwarded on you router if needed
 - Firewall setup to open relevant ports
 - A copy of this GitHub Repo downloaded or cloned on your machine that'll run the server
 - A way you can edit `.env` files (While a text editor will suffice, we recommend using something like VSCode)
 - Around 5~7 GiB of available storage 

## What this README will not cover

This README will not cover the process of changing your IP address, since connection to the server requires the exposure of your IP Address when running on a local machine like a desktop or laptop. 

Depending on how much you value exposing your IP address, you might want to explore ways to do so such as hosting the container on a hosting provider's server. 

## Setup

Before running your Valheim server, you will most likely want to define things like the name of your server and/or world, set a password or make it public or private. You can set these up by openning the [`valheim.env`]() file. If untouched, its contents will look like this:

```
# Left empty. If left empty, the server will instead use default values
SERVER_NAME=                                       
WORLD_NAME=                                         
SERVER_PASS=                                       

# default SERVER_NAME=My Server                                       
# default WORLD_NAME=Dedicated                                         
# default SERVER_PASS=secret - 5 characters minimum

# This is crucial. Crossplay is a must for the server to run successfully
CROSSPLAY=true

# Whether the server should be listed in the server browser or not
# Set this to either true or false.
SERVER_PUBLIC=false
```

While you may leave these as they are, we recommend changing the `SERVER_NAME`, `WORLD_NAME` and at the very least `SERVER_PASS`. 

Note: `WORLD_NAME` will dictate the name of the world save file. If you decide to change this field **after** having already launched your server a differently named world, the server will create a new world save file. This does mean you can use multiple world for the same server installation, though it might be an unwieldy way to do so.

## Deployment

1. On your hosting computer/server, using the Command Line Interface of your choosing, change directory to your local installation of this repository:
    ```
    cd '/your/local/install/path/'
    ```

2. Make sure Docker is available on your machine using the command below:
    ```
    docker info
    ```

3. Pull the docker image for the Valheim server (if it is not currently downloaded) using the following command:
    ```
    docker pull lloesche/valheim-server
    ```

4. After the image is successfully pulled, run the server container using the following `docker-compose` command:
    ```
    docker compose up -d
    ```

---

While it will not take long for the container to be created, it will take a few minutes before it is setup and even longer if it is being run for the first time.

If you want to peek at the server logs while it is running, use the following command to get your containers id: 
```
docker ps
```

Then, with the id of your server container, use the following command:
```
docker logs -f [your container id]
```
This will give you a realtime view of your server logs. You can use Ctrl+C to quit this view, it will not affect the server.

The Valheim server will be very verbose, but the following log message will show when your containerized game server is ready to join:
```
Session "# default =My Server" with join code XXXXXX and IP XXX.XXX.XXX.XXX:XXXX is active with 0 player(s)
```
As the server gets deployed for the first time, you will notice the creation of a `valheim-server` directory/folder. This will house the data to get the server software downloaded on your machine using SteamCmd (under `valheim-server/data`) as well as the server configuration and the game world save files (under `valheim-server/config`)

## Updating the Valheim Server Manually

The base docker image includes an auto-updater that checks every 15 mins (Or a different time you can set up with the `` CRON environment variable) whether the Valheim server is out of date compared to the version available on Steam. However, if you wish to forcefully update the server version: 

1. Stop your server with 
    ```
    docker compose down
    ```

2. In your repository, remove the `valheim-server/data` directory

> [!WARNING]  
> Do not remove the `valheim-server/config` directory! This is where your world save data and backups live!

2. (Optional) While it is most likely the Valheim server software being out of date will cause you an issue, you may also encounter issues with an out of date docker image too. Remove the docker image and re-pull it like so:
    ```
    docker image rmi lloesche/valheim-server

    docker pull lloesche/valheim-server
    ```
3. Restart the server with docker compose (as you would normally deploy it), the container will automatically download and install the server software just as if it were deployed for the first time.


## Further Documentation

This project depends on the work of the team behind the `lloesche/valheim-server-docker` image.

If you would like to learn how to customize your server even more, they provide comprehensive documentation on their github page at the link below. 


https://github.com/lloesche/valheim-server-docker