# Minecraft Bedrock Server
Run a bedrock server in a Docker container.

## Introduction
This Docker image will download the Bedrock Server app and set it up, along with its dependencies.

## Usage
### New installation
1. Prepare the persistent volumes:
    1. Create a volume for the configuration:<br/>
        ```
       docker volume create --name "bedrock-config"
        ```
    3. Create a volume for the worlds:<br/>
        ```
       docker volume create --name "bedrock-worlds"
        ```
2. Build the DockerFile:<br/>
    ```
   Docker build -t alros/bedrock_server:latest .
    ```
4. Create the Docker container:
    ```bash
    docker create --name=minecraft -it \
        -v "bedrock-config:/bedrock-server/config" \
        -v "bedrock-worlds:/bedrock-server/worlds" \
        -p 19132:19132/udp \
        --restart unless-stopped \
        alros/bedrock_server:latest
    ```
5. Start the server:<br/>
    ```
   docker start minecraft
    ```

6. Stop the server<br/>
    ```
   docker stop minecraft
    ```
### For Docker Compose
Create a file `docker-compose.yml`
```
version: '3'
services:
  minecraft-bedrock-server:
    image: alros/bedrock_server:latest
    container_name: alros-bedrock-server
    ports:
      - 19132:19132/udp
    volumes:
      - ./bedrock-config:/config
      - ./bedrock-worlds:/worlds
    environment:
      EULA: "TRUE"
      GAMEMODE: survival
      DIFFICULTY: easy
    restart: unless-stopped
```
To run
```
docker-compose up -d
```
## Commands
There are various commands that can be used in the console. To access the console, you need to attach to the container with the following command:
```
docker attach <container-id> bash
```
To leave the console without exiting the container, use `Ctrl`+`pq`.
