# cpoppema/docker-flexget

Read all about FlexGet [here](http://www.flexget.com/#Description).

## Usage

```
# Build the container from Dockerfile
docker build -t diegor/flexget .

# Create the container
docker create \
    --name=flexget \
    -e PGID=<gid> -e PUID=<uid> \
    -v </path/to/flexget/appdata>:/config \
    -v <path/to/downloads>:/downloads \
    tagname

# Example:
docker create \
    --name=flexget \
    -e PGID=20 -e PUID=501 \
    -v ~/.config/flexget/:/config \
    -v /Volumes/pool/:/downloads \
    diegor/flexget

# Start the container
docker start flexget

# Login into the container
docker exec -it flexget /bin/bash

# Once you are in, you can manager flexget as usual
# Authenticate flexget with trakt
flexget -c /config/flexget/config.yml trakt auth diego_russo

# Start flexget daemon
flexget -c /config/flexget/config.yml daemon start -d
```

This container is based on phusion-baseimage with ssh removed. For shell access whilst the container is running do `docker exec -it flexget /bin/bash`.

**Parameters**

* `-p 9091` - Transmission WebUI Port
* `-v /config` - Transmission app data
* `-v /downloads` - location of downloads on disk
* `-e PGID` for for GroupID - see below for explanation
* `-e PUID` for for UserID - see below for explanation

**Transmission**

FlexGet is able to connect with transmission using `transmissionrpc`, which is pre-installed in this container. For more details, see http://flexget.com/wiki/Plugins/transmission.

### User / Group Identifiers

**TL;DR** - The `PGID` and `PUID` values set the user / group you'd like your container to 'run as' to the host OS. This can be a user you've created or even root (not recommended).

Part of what makes this container work so well is by allowing you to specify your own `PUID` and `PGID`. This avoids nasty permissions errors with relation to data volumes (`-v` flags). When an application is installed on the host OS it is normally added to the common group called users, Docker apps due to the nature of the technology can't be added to this group. So this feature was added to let you easily choose when running your containers.

## Updates / Monitoring

* Upgrade to the latest version of FlexGet simply `docker restart flexget`.
* Monitor the logs of the container in realtime `docker logs -f flexget`.

**Credits**
* [linuxserver.io](https://github.com/linuxserver) for providing awesome docker containers.
