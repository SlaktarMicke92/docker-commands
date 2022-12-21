# docker-commands
Keep track of docker commands. Additional Tips:
  * When met with &lt;container-id>, you only need to input the three first characters in the container id.

## Build
  * cd /directory/to/Dockerfile/
  * docker build -t &lt;image-name> .

## Run
Arguments:
  * -d - Detach
  * -p 0-9{2-5}:0-9{2-5} - Port on host to port in container
  * -w - Working directory, for example /app
  * -v - Bind mount (link) the host's present getting-started/app directory to the container's /app directory. Note: Docker requires absolute paths for binding mounts, so in this example we use pwd for printing the absolute path of the working directory, i.e. the app directory, instead of typing it manually.
  * Can also run commands after &lt;image-name>, for example: sh -c "yarn install && yarn run dev"
  * -e - Environment variables, example: -e MYSQL_PASSWORD=password
  * --network - Attach to network
  * --network-alias - Name for container in network
  * -i - Keep STDIN open even if not attached
  * -t - Allocate a pseudo-tty

docker run -dp ####:#### &lt;image-name>

## Start
docker start &lt;container-id>

## List
Arguments:
  * -a - List all containers
docker ps
### Images
docker images

## Removal
1. docker stop ###
2. docker rm ###
or
  * docker rm -f ###

## Run commands on container
  * docker exec ### &lt;command>

## Volume
First you need to create a volume, for persistency.
Then you bind it to container when r.
### Create
  * docker volume create &lt;volume-name>
### Run
  * docker run -dp ####:#### -v &lt;volume-name>:&lt;/path/to/mount/folder> &lt;image-name>
Example: docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started
You can also create a named volume when running 'docker run':
  * docker run -dp ####:#### -v &lt;volume-name-that-does-not-exist>:&lt;/path/to/mount/folder> &lt;image-name>

## Logs
Read logs in realtime:
  * docker logs -f &lt;container-id>
  
## Network
Create a network for attachment later:
  * docker network create todo-app

## History
List all the lines that an image contains:
  * docker image history &lt;image>

## Security
Do a scan and show vulnerabilities in the supported package managers:
  * docker scan &lt;image>
  * docker scan --no-trunc &lt;image>
  
## Docker Compose
###### docker-compose.yml
Key words:
  * services - Define service
    * &lt;name> - Name of service
      * &lt;image> - The image to use in the service named &lt;name>
      * command - Command(s) to run
      * ports - Specify host port and port inside container, example: ports: \n - 3000:3000
      * working_dir - Specify working directory, example: working_dir: /app
      * volumes - Specify volumes, example: volumes: \n - ./:/app \n &lt;named-volume>:/var/lib/mysql
      * environment - Set environment variables
### Volumes
To mount named volumes you have to define the volumes at the top level of the docker-compose.yml file.
  * volumes: \n &lt;name>:

### Run
  * docker compose up -d
  
### Logs
  * docker compose logs -f
  
### Stop
  * docker compose down
  * docker compose down --volumes

## Dockerfile examples
Reference: [NodeJS Docker Guide](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)\
Example of how to use caching to improve build times when making changes in the code (which will change how COPY . . works):
```
FROM node:18-alpine
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --production
COPY . .
CMD ["node", "src/index.js"]
```

## .dockerignore
Works like .gitignore, for when building containers

## Multi-Stage Builds
Tutorial won't really go through this topic, fill in more later. There is an example of how to ship a Java application without the unnecessary build tools:

```
FROM maven AS build
WORKDIR /app
COPY . .
RUN mvn package

FROM tomcat
COPY --from=build /app/target/file.war /usr/local/tomcat/webapps
```

An example of a NodeJS application with Multi-Stage Builds:

```
FROM node:18 AS build
WORKDIR /app
COPY package* yarn.lock ./
RUN yarn install
COPY public ./public
COPY src ./src
RUN yarn run build

FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
```
