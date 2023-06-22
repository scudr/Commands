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


   