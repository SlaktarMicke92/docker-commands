# docker-commands
Keep track of docker commands. Additional Tips:
  * When met with <container-id>, you only need to input the three first characters in the container id.

## Build
  * cd /directory/to/Dockerfile/
  * docker build -t <image-name> .

## Run
Arguments:
  * -d - Detach
  * -p 0-9{2-5}:0-9{2-5} - Port on host to port in container
  * -w - Working directory, for example /app
  * -v - Bind mount (link) the host's present getting-started/app directory to the container's /app directory. Note: Docker requires absolute paths for binding mounts, so in this example we use pwd for printing the absolute path of the working directory, i.e. the app directory, instead of typing it manually.
  * Can also run commands after <image-name>, for example: sh -c "yarn install && yarn run dev"
  * -e - Environment variables, example: -e MYSQL_PASSWORD=password
  * --network - Attach to network
  * --network-alias - Name for container in network
  * -i - Keep STDIN open even if not attached
  * -t - Allocate a pseudo-tty

docker run -dp ####:#### <image-name>

## Start
docker start <container-id>

## List
Arguments:
  * -a - List all containers
docker ps

## Removal
1. docker stop ###
2. docker rm ###
or
  * docker rm -f ###

## Run commands on container
  * docker exec ### <command>

## Volume
First you need to create a volume, for persistency.
Then you bind it to container when r.
### Create
  * docker volume create <volume-name>
### Run
  * docker run -dp ####:#### -v <volume-name>:</path/to/mount/folder> <image-name>
Example: docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
You can also create a named volume when running 'docker run':
  * docker run -dp ####:#### -v <volume-name-that-does-not-exist>:</path/to/mount/folder> <image-name>

## Logs
Read logs in realtime:
  * docker logs -f <container-id>
  
## Network
Create a network for attachment later:
  * docker network create todo-app
  
## Docker Compose
###### docker-compose.yml
Key words:
  * services - Define service
    * <name> - Name of service
      * <image> - The image to use in the service named <name>
      * command - Command(s) to run
      * ports - Specify host port and port inside container, example: ports: \n - 3000:3000
      * working_dir - Specify working directory, example: working_dir: /app
      * volumes - Specify volumes, example: volumes: \n - ./:/app \n <named-volume>:/var/lib/mysql
      * environment - Set environment variables
### Volumes
To mount named volumes you have to define the volumes at the top level of the docker-compose.yml file.
  * volumes: \n <name>:

### Run
  * docker compose up -d
  
### Logs
  * docker compose logs -f
  
### Stop
  * docker compose down
  * docker compose down --volumes
