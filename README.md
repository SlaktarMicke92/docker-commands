# docker-commands
Keep track of docker commands

## Build
cd /directory/to/Dockerfile/
docker build -t image-name .

## Run
Arguments
-d - Detach
-p 0-9{2-5}:0-9{2-5} - Port on host to port in container

docker run -dp ####:#### image-name

## List
docker ps

## Removal
docker stop ###
docker rm ###
or
docker rm -f ###

## Run commands on container
docker exec ### <command>

## Volume
First you need to create a volume, for persistency.
Then you bind it to container when r.
### Create
docker volume create <volume-name>
### Run
docker run -dp ####:#### -v <volume-name>:</path/to/mount/folder> <image-name>
Example: docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
