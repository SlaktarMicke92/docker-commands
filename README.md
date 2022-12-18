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
