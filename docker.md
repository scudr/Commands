# Docker Tutorial Commands

## Docker Image Commands

1. `docker image build .` 
   - This command builds a Docker image from a Dockerfile and a "context". The `.` is specifying the current directory as the context.

2. `docker image ls`
   - This command lists all Docker images that are currently stored on your host machine.

3. `docker image inspect 2b`
   - This command shows detailed information in JSON format about the Docker image with the ID or tag "2b".

4. `docker image build -t my-image .` 
   - Similar to the first build command, but it tags the resulting image with the name "my-image".

## Docker Run Commands

1. `docker run --rm 2b ls /app`
   - This command runs a specific command (`ls /app`) in a new container. The container is removed after the command is executed.

2. `docker run --rm 78 curl example.com`
   - Runs the `curl example.com` command in a new container and removes the container afterwards.

3. `docker run --rm f43 --argument`
   - Starts a new container using the "f43" image and passes `--argument` as a command line argument to the container's entrypoint.

4. `docker run --rm my-image`
   - Starts a new container from the "my-image" Docker image and removes the container after it stops.

## Dockerfile Instructions

1. `FROM ubuntu`
   - This command initializes a new build stage and sets the base image for subsequent instructions.

2. `COPY . /app`
   - This instruction copies new files or directories from `<src>` and adds them to the filesystem of the container at the path `<dest>`.

3. `RUN apt -y update && apt -y install curl`
   - RUN instruction allows you to install applications and packages inside your Docker image.

4. `ENTRYPOINT [ "/app/app.sh" ]`
   - ENTRYPOINT allows you to configure a container that will run as an executable.

5. `CMD [ "--argument" ]`
   - CMD instruction provides defaults for an executing container, in this case, `--argument` is the default.

6. `ENV curl_bin="curl"`
   - ENV instruction sets the environment variable `curl_bin` to "curl".

7. `WORKDIR /app`
   - WORKDIR sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile.

8. `USER root`
   - The USER instruction sets the user name to use when running the image.

9. `EXPOSE 8080`
   - The EXPOSE instruction informs Docker that the container listens on the specified network ports at runtime.

10. `LABEL maintainer="carlos escudero <carlos@escudero.me>"`
    - LABEL instruction adds metadata to an image.

### Examples
#### V1
```bash
FROM ubuntu
COPY . /app
RUN apt -y update && apt -y install curl
ENTRYPOINT [ "/app/app.sh" ]
CMD [ "--argument" ]
```


#### V2
```bash
FROM ubuntu
COPY . /app
RUN apt -y update && apt -y install curl
ENTRYPOINT [ "/app/app.sh", "--argument"]
```

#### V3
```bash
FROM ubuntu
COPY . /app
ENV  curl_bin="curl"
RUN apt -y update && apt -y install "$curl_bin"
ENTRYPOINT [ "/app/app.sh" ]
CMD [ "--argument" ]
```

## Miscellaneous

- `docker run --privileged --rm aptman/qus --static -- --path arm`
  - Runs a command in privileged mode in a new container and removes the container afterwards.

- `sudo apt -y install qemu qemu-system-misc qemu-user-static qemu-user binfmt-support`
  - Installs QEMU emulator on the host machine, which is used to run code compiled for different CPU architectures.

- `docker run --rm --platform linux/arm alpine uname`
  - Runs the `uname` command in a new Docker container with the platform specified as "linux/arm", allowing you to test arm architecture-based Docker images.

- `docker run --rm alpine uname -a`

## Docker Run with Entrypoint
1. `docker run --entrypoint sh --rm alpine -c 'touch /tmp/file && chown 1001:1001 /tmp/file && echo "File permission changed."'` 

   This command runs a Docker container from the alpine image, creates a file in the /tmp directory, changes the file's owner to user 1001, and then prints "File permission changed".

2. `docker run --entrypoint sh --cap-drop CAP_CHOWN --rm alpine -c 'touch /tmp/file && chown 1001:1001 /tmp/file && echo "File permission changed."'`

   This command is similar to the previous one but it also drops the `CAP_CHOWN` capability which prevents the container from changing ownership of files.

3. `docker run --entrypoint sh --cap-drop CAP_CHOWN --cap-add CHOWN --rm alpine -c 'touch /tmp/file && chown 1001:1001 /tmp/file && echo "File permission changed."'`

   This command drops the `CAP_CHOWN` capability and then adds it back. This will allow the container to change file ownership again.

4. `docker run --entrypoint sh --cap-drop CAP_CHOWN --cap-add CHOWN --cap-add LEASE --cap-drop NET_ADMIN --rm alpine -c 'touch /tmp/file && chown 1001:1001 /tmp/file && echo "File permission changed."'`

   This command adds a few more capabilities and drops `NET_ADMIN`. It illustrates how to add and drop multiple capabilities.

5. `docker run --entrypoint sh --cap-add CHOWN --cap-drop ALL --rm alpine -c 'touch /tmp/file && chown 1001:1001 /tmp/file && echo "File permission changed."'`

   This command adds the `CHOWN` capability but drops all other capabilities. This restricts what the container can do to only changing file ownership.


## Docker Build and Run with Stress Testing
1. `docker build -t stress .` 

   This command builds a Docker image using the Dockerfile in the current directory and tags it as `stress`.

2. `docker run --rm stress --cpu 4 --timeout 5`

   This command runs a Docker container from the `stress` image, limits the container to use 4 CPUs, and sets a timeout of 5 seconds.

3. `docker run --rm --cpus 1.6 stress --cpu 4 --timeout 5`

   This command runs a Docker container from the `stress` image, limits the container to use 1.6 CPUs, and sets a timeout of 5 seconds.

4. `docker run --rm --cpu-period 100000 --cpu-quota 160000 stress --cpu 4 --timeout 5`

   This command runs a Docker container from the `stress` image, sets the CPU period to 100000 and the CPU quota to 160000. This means the container can use 1.6 CPUs.

5. `docker run --rm stress --vm 1 --vm-bytes 3.99G --timeout 5`

   This command runs a Docker container from the `stress` image and creates a VM inside the container with 1 CPU and 3.99GB of memory. It then sets a timeout of 5 seconds.

## Docker RUN with Logging
1. `docker run --name test-container my-image`

   This command runs a Docker container named `test-container` from the `my-image` image.

2. `docker logs test-container`

   This command displays the logs from the `test-container`.

3. `docker ps -a`

   This command lists all Docker containers, both running and stopped.

4. `docker inspect test-container --format '{{.ID}}'`

   This command retrieves the ID of `test-container`.

5. `sudo ls /var/lib/docker/containers/<container-id>`

   This command lists the contents of the directory associated with the specific container ID.

6. `docker run --name test-container-2 --log-driver none my-image`

   This command runs a Docker container named `test-container-2` from the `my-image` image and sets the logging driver to none, disabling logging for this container.

7. `docker inspect test-container-2 --format '{{.ID}}'`

   This command retrieves the ID of `test-container-2`.

## LABEL

LABEL maintainer-"carlos escudero <carlos@escudero.me>"
LABEL foo=bar
LABEL baz=quux

## WORKDIR
```bash
WORKDIR /
RUN pwd
WORKDIR /app
ENTRYPOINT [" ./app.sh"]
```

## USER
```bash
USER root
RUN apt -y update && apt -y install "$curl_bin"
RUN useradd - p supersecret newuser
CMD ["--argument"]
USER newuser
ENTRYPOINT ["/app/app.sh"]
```

## EXPOSE
```bash
From ubuntu
COPY . /app
ENV curl_bin=curl
RUN apt -y update && apt -y install "$curl_bin"
CMD ["--argument"]
ENTRYPOINT ["/app/app.sh"]
EXPOSE 8080
```



You may need to register QEMU static binaries in the kernel with docker run --rm --privileged multiarch/qemu-user-static --reset -p yes, and after that you should be able to run ARM (or other architectures) images on your x86 machine.
After running this command, you should be able to run docker run --rm --platform linux/arm alpine uname.

- `docker build -t my-image:latest-x86 --pull .`

- `docker build -t my-image:latest-arm --platform linux/arm  --pull .`

- `docker run --rm my-image:latest-arm`



# Useful Docker Daemon Options
# Docker Daemon

The Docker daemon, also known as `dockerd`, is a persistent background process that manages Docker containers and handles container objects on a host system. The daemon listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. The Docker daemon can also communicate with other Docker daemons to manage Docker services.

It's important to note that the user does not interact directly with the daemon, but instead through the Docker client. When you run Docker commands such as `docker run` or `docker build`, the client sends these commands to `dockerd`, which carries them out. 

Below are some useful Docker daemon options:
### `--config`
This option allows you to specify the location of the Docker configuration files. 
Example: `dockerd --config /etc/docker`

### `--data-root`
This option allows you to set the Docker root directory, which is where all Docker data is stored (images, containers, etc).
Example: `dockerd --data-root /var/lib/docker_custom`

### `--debug`
This option allows you to enable debug mode, which provides more detailed logs for debugging purposes.
Example: `dockerd --debug`

### `--default-runtime`
This option allows you to set the default runtime Docker will use.
Example: `dockerd --default-runtime runc`

### `--log-level`
This option allows you to set the logging level. Possible values are `debug`, `info`, `warn`, `error`, `fatal`.
Example: `dockerd --log-level error`

### `--insecure-registry`
This option allows you to enable insecure registry communication, i.e., Docker does not perform SSL verification when pulling from or pushing to these registries.
Example: `dockerd --insecure-registry myregistrydomain.com:5000`

### `--add-runtime`
This option allows you to add a new runtime to Docker.
Example: `dockerd --add-runtime custom=/usr/bin/custom_runtime`


# Docker Commands: Deep Dive

This document provides an overview of various Docker commands, including how to run Docker within Docker, interact with the Docker API, install Docker, list Docker containers, and more.

```bash
# Docker within Docker
# The following command allows us to run Docker commands within a Docker container. 
# It starts a new container from 'my-image', mounts the Docker socket from the host to the container, 
# starts the container with bash shell, and provides an interactive terminal. 
# The '--rm' option removes the container after it exits.
docker run --rm -v /var/run/docker.sock:/var/run/docker.sock --entrypoint bash -i -t my-image

# Curl with Docker Socket
# This command sends an HTTP request through the Docker socket to the Docker API. 
# It sends a request to the Docker API to list all containers via the Docker socket.
curl --unix-socket /var/run/docker.sock http://anything/containers/json

# Docker Installation
# This command fetches and executes the Docker installation script.
curl -L get.docker.io | bash

# Listing Docker Containers
# This command lists all running Docker containers.
docker ps

# Docker Run with --rm
# This command starts a new Docker container and removes it after exit.
docker run --rm my-image

# Docker Run with Mounted File
# This command starts a new Docker container from 'alpine' image with '/app/include/header.txt' from the host 
# mounted to '/header.txt' inside the container.
docker run --rm -it -v /app/include/header.txt:/header.txt alpine

# Docker Volume Create
# This command creates a new Docker volume named 'temp'.
docker volume create temp

# Docker Container with Mounted Volume
# This command creates a new Docker container from 'alpine' image with 'temp' volume from the host 
# mounted to '/tmp' inside the container.
docker container create -v temp:/tmp alpine
```

```bash
# Inspecting a Docker Network
docker network inspect network-a

# Creating a Container and Connecting to a Network
docker container create -it --name container-c --entrypoint sh --net network-a curlimages/curl

# Stopping a Container
docker container stop container-c

# Connecting a Container to a Different Network
docker network connect network-b container-c

# Starting a Container
docker container start container-c

# Attaching to a Container
docker container attach container-c
```



```bash
docker network inspect network-a

docker container create -it --name container-c --entrypoint sh --net network-a curlimages/curl

docker container stop container-c

docker network connect network-b container-c

docker container start container-c

docker container attach container-c
```

## Macvlan

The `macvlan` network driver allows containers to have their own unique MAC addresses assigned from the underlying network, enabling them to appear as separate physical devices on the network. This provides direct network access for containers.

## IPvlan

The `ipvlan` network driver is similar to `macvlan`, but it allows containers to have their own unique IP addresses on the network. It provides direct network access while maintaining the ability to isolate traffic between containers.

## Overlay Network

The overlay network driver enables multi-host communication and network connectivity for Docker services or swarm services. It allows containers running on different Docker hosts to communicate with each other securely.

Standard examples and usage of these networking modes can be found in the Docker documentation.

## Run Docker Container with Volume

```bash
docker run -i -t --rm --mount 'type=volume,source=super-super-important,destination=/super-important' alpine
```
This command runs an Alpine-based Docker container, and mounts the 'super-super-important' volume to the '/super-important' path inside the container.

## Run Docker Container and Write Data

```bash
docker run --rm -v /tmp/lima/stuff:/stuff alpine sh -c 'echo "This is file number 10" > /stuff/file-10'
```
This command runs an Alpine Docker container, maps the '/tmp/lima/stuff' directory from the host into the '/stuff' directory in the container, and writes "This is file number 10" to the file '/stuff/file-10'.

## Create Backup from Docker Volume
```bash
docker run --rm -v $PWD:/target -v super-super-important:/backup alpine tar cvzf /target/backup.tar -C /backup .
```
This command runs a Docker container and maps the current directory from the host into the '/target' directory in the container and the 'super-super-important' volume to '/backup'. Then it uses the 'tar' command to create a gzip compressed tarball of the '/backup' directory, storing the tarball in the '/target' directory.

## List Contents of Tar File
```bash
tar -tf backup.tar
```
This command extracts the backup.tar file from the host's current directory ('/target') to the new Docker volume ('super-super-important-restore') mounted at '/restore' in the container.

## Create Large Random File
```bash
time docker run --rm -v /tmp/lima/stuff:/app alpine dd if=/dev/urandom of=/app/stuff.txt bs=1G count=1
```
This command creates a 1 GB file filled with random data at '/tmp/lima/stuff/stuff.txt' on the host. The 'time' command is used to measure how long the operation takes.

## Run Docker Registry

```bash
docker run --rm --name registry -p 5000:5000 -d registry:2
```
This command starts a new Docker registry container which listens on port 5000.

## Push Docker Image to Registry
```bash
docker push localhost:5000/my-image:latest
```
This command pushes the 'my-image:latest' image to the local Docker registry.
## Remove Docker Image from Local Cache
```bash
docker rmi localhost:5000/my-image
docker rmi my-image -f
```
These commands remove 'my-image' from the local Docker image cache. The '-f' flag is used to force the removal if the image is being used by a stopped container or has other tags.

## Run Docker Image from Registry
docker run --rm localhost:5000/my-image

```bash
docker run -d -p 8800:80 httpd
docker run -d -p 8801:80 httpd
-d: detach run in the background
80 apache is listening in this port
httpd: apache daemon

curl localhost:8800



 docker build -t first-container .

 docker image ls
```
# Steps for downloading the APP to my AWS CLOUD 9 - Video "WORKING WITH CONTAINERS 101"

## Pull the Docker image
```bash 
docker pull russau/first-container
```
## start a container 
```bash
docker run -d --name my-container russau/first-container
```
## Get a shell inside my running container
```bash
docker exec -it my-container sh  or docker exec -it my-container bash
```

# Copy a directory in my container to an specific dir in my host 
```bash
 docker cp my-container:/app /home/ec2-user/environment/first-container/
 ```
```bash
docker build -t first-container .
```
#  docker run -d -p 8080:8080 first-container                                                                                                                                                    

```bash 
docker ps
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS          PORTS                                       NAMES
91a1502eeca9   first-container          "docker-entrypoint.s…"   3 minutes ago    Up 3 minutes    0.0.0.0:8080->8080/tcp, :::8080->8080/tcp   quizzical_clarke
```
## launch a shell inside this container
```bash
 docker exec -it 91 sh, where 91 is the container ID, see line before. 
```
-it iterative , i uses stdin and t terminal

ps -a  to see the processes running on my container
```bash
 docker stop 91

 docker rm 91
```
 to pass an environment variable to my containerized app docker run -e MESSAGE_COLOR=#0000ff -p 8080:8080 first-container


 # Make my host file system available inside my container by using bind mount 
```bash
docker run -v ~/input.txt:/app/input.txt -p 80:8080 first-container
```
```dockerfile
FROM node:gallium-alpine3.14
WORKDIR /app
ENV NODE_ENV=production


#copy application files
COPY app .
RUN npm install

CMD node server.js
EXPOSE 8080
```

1. `FROM node:gallium-alpine3.14`: This line is telling Docker to use the `node:gallium-alpine3.14` image as the base for the new image you're building. This is a Node.js image built on the Alpine Linux 3.14 distribution with Node.js version "gallium" (which is a codename for a specific Node.js version).

2. `WORKDIR /app`: This sets the working directory in the Docker container to `/app`. This means that any subsequent commands that run as part of the build process will be run in this directory.

3. `ENV NODE_ENV=production`: This sets an environment variable in the Docker image. In this case, it's setting `NODE_ENV` to `production`, which is a common practice in Node.js applications to indicate that the application should run in production mode.

4. `COPY app .`: This is copying the local `app` directory (on your machine, where you're running the `docker build` command) into the Docker image. The `.` refers to the current location in the Docker image, which is `/app` as set by the `WORKDIR` command.

5. `RUN npm install`: This command is running `npm install` in the Docker image as it's being built. This installs the dependencies defined in the `package.json` file that was copied into the image with the `app` directory.

6. `CMD node server.js`: This sets the default command to be run when a container is started from the image. In this case, it's running the Node.js application by calling `node server.js`. This assumes there's a `server.js` file in the `/app` directory.

7. `EXPOSE 8080`: This tells Docker that the container will be listening on port 8080 when it's run. This doesn't actually publish the port; it's more like documentation to tell the user that the application inside the container will be using this port.
