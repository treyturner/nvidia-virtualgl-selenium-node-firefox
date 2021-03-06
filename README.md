# nvidia-virtualgl-selenium-node-firefox
Selenium node running Firefox v57.0.4.
VirtualGL is used to enable hardware accelerated WebGL content.

This image is intended to be used via a selenium/hub instance, see below for a basic setup.

# Selenium grid setup

The following is a base docker-compose file intended to be launched by nvidia-docker-compose https://github.com/eywalker/nvidia-docker-compose

Tested with nvidia-docker-compose v0.0.5 and nvidia-docker 1.0.0

```yaml
version: '2'

services:
    selenium_hub:
        image: selenium/hub:3.8.1-erbium
        container_name: selenium_hub
        privileged: true
        ports:
            - 4444:4444
        environment:
            - GRID_TIMEOUT=120000
            - GRID_BROWSER_TIMEOUT=120000
        networks:
            - selenium_grid_internal

    nodefirefox:
        image: treyturner/nvidia-virtualgl-selenium-node-firefox
        depends_on:
            - selenium_hub
        ports:
            - 5900
        environment:
            - no_proxy=localhost
            - TZ=America/Chicago
            - HUB_PORT_4444_TCP_ADDR=selenium_hub
            - HUB_PORT_4444_TCP_PORT=4444
            - DISPLAY
        volumes:
            - /tmp/.X11-unix:/tmp/.X11-unix:rw
        networks:
            - selenium_grid_internal

networks:
    selenium_grid_internal:
```

# Launch commands
```bash
xhost +local:root
nvidia-docker-compose up -d
```
