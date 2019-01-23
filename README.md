# ubuntu-gui-docker

Docker compose apps to run ubuntu base (or android) image with visual NoVNC client remote connection. You can also upload/download files from container on your machine with fileBrowser service.

[![](https://images.microbadger.com/badges/image/javanile/novnc.svg)](https://microbadger.com/images/javanile/novnc "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/javanile/novnc.svg)](https://microbadger.com/images/javanile/novnc "Get your own version badge on microbadger.com")

## Run with ubuntu container and filebrowser service

[read issue here](https://github.com/novnc/noVNC/issues/169#issuecomment-443250680) .

In this way I have a base linux image with transfer file and GUI.

After, the new linux image must be extended with android-sdk and **built**, in order to use in a new docker-compose.
[example android-sdk image](https://github.com/thyrlian/AndroidSDK/blob/master/android-sdk/Dockerfile)

## Usage
**STEP #1** Run docker compose app, in _simple_ folder, with this yml:
```yml
version: '2'

services:
    vnc:
        hostname: vnc
#        build: .
        image: dorowu/ubuntu-desktop-lxde-vnc:latest
        environment:
            USER: ${BASIC_USER}
#            VNC_PASSWORD: ${BASIC_PASSWORD}
            HTTP_PASSWORD: ${BASIC_PASSWORD}
        ports:
            - "${UBUNTU_CONTAINER_PORT}:80"
        volumes:
            - home:/home
        networks:
            - default
    files:
        hostname: files
        image: filebrowser/filebrowser:v1.10.0
        # Uncoment for debug purposes
        ports:
            - ${FILE_BROWSER_PORT}:80
        command: --no-auth --baseurl=/files
        volumes:
            - home:/srv
        networks:
            - default

volumes:
    home:

networks:
    default:
```
**STEP #2** Run container with follow command
```bash
docker-compose up -d
```
**STEP #3** Now visit this http://localhost:UBUNTU_CONTAINER_PORT for ubuntu view, and http://localhost:FILE_BROWSER_PORT for fileBrowser app.

## Configuration
Environment variables exist in the docker file, defined in .env file, for configuration

### Variables
- **BASIC_USER**: the user for basic auth
- **BASIC_PASSWORD**: the password for basic auth
- **UBUNTU_CONTAINER_PORT**: Port that the Ubuntu VNC Server is listening on 
- **FILE_BROWSER_PORT**: Port that the file browser is listening on 


