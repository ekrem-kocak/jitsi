# Jitsi Self-Hosting Guide - Docker Installation and Excalidraw Integration

## Installation Steps

- You can download the package from the following link: https://github.com/jitsi/docker-jitsi-meet/releases/tag/stable-8319 and extract it to a folder.

![image](https://user-images.githubusercontent.com/64695572/224693374-c71fb916-eb5a-4e70-8ab2-9214d92d2c22.png)

1. Create a .env file by copying and adjusting env.example: `cp env.example .env` 
2. Set strong passwords in the security section options of .env file by running the following bash script `./gen-passwords.sh`
3. Create required `CONFIG` directories
    - For linux:
    
    ```
    mkdir -p ~/.jitsi-meet-cfg/{web,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri}
    
    ```
    
    - For Windows:

    ```
    echo web,transcripts,prosody/config,prosody/prosody-plugins-custom,jicofo,jvb,jigasi,jibri | % { mkdir "~/.jitsi-meet-cfg/$_" }
    
    ```
4. Open the .env file with an editor and go to the bottom line of the file.
    ```
    ENABLE_LOBBY=1
    ENABLE_PREJOIN_PAGE=1
    ENABLE_WELCOME_PAGE=1
    DISABLE_AUDIO_LEVELS=0
    ENABLE_NOISY_MIC_DETECTION=1
    ENABLE_GUESTS=1
    ENABLE_XMPP_WEBSOCKET=0
    ENABLE_RECORDING = 1
    ENABLE_SERVICE_RECORDING = 0
    ENABLE_FILE_RECORDING_SHARING = 1
    WHITEBOARD_ENABLED = 1
    WHITEBOARD_COLLAB_SERVER_PUBLIC_URL = "http://localhost:3001"
    ``` 
    - Assign the WHITEBOARD_COLLAB_SERVER_PUBLIC_URL and the port for the Excalidraw room.
    - If you want to add additional features, you can refer to this link: https://github.com/jitsi/docker-jitsi-meet/blob/master/web/rootfs/defaults/settings-config.js.
5. The following lines were added to the docker-compose.yml file:
    ```
    excalidraw:
        image: docker.io/excalidraw/excalidraw:latest
        environment:
            - EXCALIDRAW_ENABLE_LIVE_COLLABORATION=true
            - EXCALIDRAW_ALLOW_ANONYMOUS_EDITS=true
        ports:
            - "5000:5000"
            
    excalidraw-room:
        image: docker.io/excalidraw/excalidraw-room:latest
        environment:
          - PORT=3001
          - EXCALIDRAW_ROOMS__DEFAULT__NAME=example
          - EXCALIDRAW_ROOMS__DEFAULT__VIEWS=30
          - EXCALIDRAW_ROOMS__DEFAULT__EDITORS=30
        ports:
          - "3001:3001"
    ```
6. Run the command `docker-compose up -d` and Jitsi will be ready to use at http://localhost:8000/.


### If you encounter an error due to HTTPS, you can make the following changes in Google Chrome:
- Go to chrome://flags/#allow-insecure-localhost.
  ![image](https://user-images.githubusercontent.com/64695572/224695480-8c0a1296-c6a2-4cc7-a10f-5b89f98f3810.png)
- Enable this option.
