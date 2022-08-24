# Minecraft Bedrock Server
Run a bedrock server in a Docker container.

## Introduction
This Docker image will download the Bedrock Server app and set it up, along with its dependencies.

## Usage
### New installation
1. Prepare the persistent volumes:
    1. Create a volume for the configuration:<br/>
        `docker volume create --name "bedrock-config"`
    2. Create a volume for the worlds:<br/>
        `docker volume create --name "bedrock-worlds"`
2. Build the DockerFile:<br/>
    `Docker build -t alros/bedrock-server:[version] .`
3. Create the Docker container:
    ```bash
    docker create --name=minecraft -it\
        -v "bedrock-config:/bedrock-server/config"\
        -v "bedrock-worlds:/bedrock-server/worlds"\
        -p 19132:19132/udp\
        --restart unless-stopped\
        alros/bedrock_server:[version]
    ```
4. Prepare the config files
    1. Either start the server once and stop it
    2. or copy the files from the original archives
5. Configure the default files in the `config` volume:
    1. Configure the `server.properties` to your likings.
    2. Configure the `allowlist.json` in case you have set `allow-list=true` in the above step. Note: The `xuid` is optional and will automatically be added as soon as a matching player connects. Here's an example of a `allowlist.json` file:
        ```json
        [
            {
                "ignoresPlayerLimit": false,
                "name": "MyPlayer"
            },
            {
                "ignoresPlayerLimit": false,
                "name": "AnotherPlayer",
                "xuid": "274817248"
            }
        ]
        ```
    3. Configure the `permissions.json` and add the operators. This file consists of a list of `permissions` and `xuid`s. The `permissions` can be `member`, `visitor` or `operator`. The `xuid` can be copied from the `allowlist.json` as soon as the user connected once. An example could look like:
        ```json
        [
            {
                "permission": "operator",
                "xuid": "274817248"
            }
        ]
        ```
4. Start the server:<br/>
    `docker start minecraft`

5. Stop the server<br/>
    `docker stop minecraft`
## Commands
There are various commands that can be used in the console. To access the console, you need to attach to the container with the following command:
```
docker attach <container-id>
```
To leave the console without exiting the container, use `Ctrl`+`pq`.
