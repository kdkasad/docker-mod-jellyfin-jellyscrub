# Jellyscrub Docker Mod
This is a [Docker mod](https://github.com/linuxserver/docker-mods) for
[LinuxServer.io's Jellyfin
container](https://docs.linuxserver.io/images/docker-jellyfin).
It modifies the `index.html` file in Jellyfin's web directory in order to load
a script required by the Jellyscrub plugin.

## How to use it
Simply add `ghcr.io/kdkasad/docker-mods:jellyfin-jellyscrub` to the
`DOCKER_MODS` environment variable for your Jellyfin container.

An Docker Compose service definition might look like this...
```yaml
version: "3"

services:
  jellyfin:
    image: lscr.io/linuxserver/jellyfin:latest
    container_name: jellyfin
    ports:
      - 7359:7359/udp
      - 1900:1900/udp
    volumes:
      - /media:/media
      - jellyfin_config:/config
    tmpfs:
      - /config/cache
      - /config/data/transcodes
    devices:
      # VA-API devices
      - /dev/dri/card0:/dev/dri/card0
      - /dev/dri/renderD128:/dev/dri/renderD128
    restart: unless-stopped
    environment:
		DOCKER_MODS: ghcr.io/kdkasad/docker-mods:jellyfin-jellyscrub
		# ...
```

## When to use it
This mod *will not* enable Jellyscrub on its own.
The Jellyscrub plugin for Jellyfin needs to be installed first.
See
[Jellyscrub's documentation](https://github.com/nicknsy/jellyscrub#installation)
for instructions.
