# **Docker Commands**

## Introduction
In this article, I am going to present a comprehensive cheat sheet of commonly used `Docker` commands

## Installing Docker

Here are the commands to install Docker on different operating systems:

```sh
# Ubuntu/Debian:
sudo apt-get update
sudo apt-get install docker.io


# MacOS (using Homebrew):
brew install docker

# Windows OS (using choco)
choco install docker-desktop
```
## Docker Install verify
To know docker is installed or not

```sh
which docker

# output
/usr/bin/docker
```

What is the version installed on your machine

```sh
docker -version
```

## General Commands

Start the docker daemon

```sh
docker -d
```

Get help with Docker. Can also use –help on all subcommands

```sh
docker --help
```

Display system-wide information

```sh
docker info
```

## Docker Image

 Docker image is a lightweight, standalone, and executable package that contains everything needed to run a piece of software, including the code, runtime, system tools, libraries, and dependencies. 

```sh
# List local images
docker images

# Delete an Image
docker rmi <image_name>

# Remove all unused images
docker image prune
```


## Docker Build

Build an image from a Dockerfile

```sh
# Build an image from a Dockerfile and tag it with a specified name.
docker build -t <image_name>

# build an image and tag with naming conventions
docker build -t projectname/domainname/appname:yyyymmdd.sequence .
# Example
docker build -t sample/aspnet-api:20230226.1 .

# Build an image from a Dockerfile without the cache
docker build -t <image_name> . –no-cache
```

## Docker Run

```sh
# Create and run a container from an image, with a custom name:
docker run --name <container_name> <image_name>

# Run a container with and publish a container’s port(s) to the host.
docker run -p <host_port>:<container_port> <image_name>

# Run a container in the background
docker run -d <image_name>

# Remove a stopped container:
docker rm <container_name>

# Example: 
docker run --rm -p 8080:80 project1/domain1/app1:20230226.1
```

- **--rm:** This option automatically removes the container when it exits. It ensures that the container is cleaned up after it finishes running. This is useful for temporary or disposable containers.
- **-p 8080:80:** This option maps the host machine's port 8080 to the container's port 80. It establishes a network connection between the host and the container, allowing access to the containerized application via port 8080 on the host.

Exit the container

```sh
exit
```
## Docker Push


```sh
# Publish an image to Docker Hub
docker push <username>/<image_name>
``` 
## Docker container

A Docker container is a lightweight, standalone, and executable runtime instance of a Docker image. It represents a running process that is isolated from the host system and other containers. Docker container providing a consistent and reproducible environment for running applications. Containers are highly portable and can be easily moved and deployed across different environments, such as development, testing, staging, and production. 

## Docker Hub

Docker Hub is a cloud-based registry service provided by Docker that allows developers to store and share container images. It serves as a centralized repository for Docker images,

```sh
# Login into Docker
docker login -u <username>

# Publish an image to Docker Hub
docker push <username>/<image_name>

# Search Hub for an image
docker search <image_name>

# Pull an image from a Docker Hub
docker pull <image_name>
```


## Docker network

This command creates a new bridge network named "network1" that containers can connect to for networked communication.

```sh
docker network create -d bridge network1
```

## Clean up resources

you can use the `docker system prune` command to clean up all dangling or unused resources, including images, containers, volumes, and networks that are not tagged or connected to a running container. This command is helpful for freeing up disk space and removing unnecessary resources.

```sh
# before cleaning up Docker, first check all the available resources using the following commands:

docker  container ls
docker  image ls
docker  volume ls
docker  network ls
docker  info

docker system prune
# or
docker system prune -a

```

If you need to clean up all containers and images locally in Docker Desktop, you can use the following commands:

```sh
# To delete all containers including its volumes use,
docker rm -vf $(docker ps -aq)

# To delete all volumes use,
docker volume rm $(docker volume ls -q)

# To delete all the images,
docker rmi -f $(docker images -aq)
```

## Docker Compose Commands

Below are some commonly used Docker Compose commands:

### Starts services

```bash
docker-compose up
```

Starts the services defined in your `docker-compose.yml` file. It creates and starts containers as specified in the configuration.


```bash
docker-compose up -d
```

Starts the services in the background (detached mode).

### Stops services

```bash
docker-compose down
```
Stops and removes containers, networks, volumes, and other services defined in your `docker-compose.yml` file.


```bash
docker-compose down -v
```
Stops and removes containers, networks, volumes, and other services while also removing volumes.
    

```bash
docker-compose down --volumes --rmi all
```
Stops and removes containers, networks, volumes, and other services, while also removing volumes and images.    

```bash
docker-compose stop
```

Stops the services defined in your `docker-compose.yml` file without removing them.

### Lists the containers

```bash
docker-compose ps
```

Lists the containers that are part of your Docker Compose setup, showing their status.
    
```bash
docker-compose ps -a
```
Lists all containers, including stopped ones, that are part of your Docker Compose setup.

### Displays log

```bash
docker-compose logs
```
Displays log output from services. You can use the `-f` option to follow the logs in real-time.

```bash
docker-compose logs webserver
```

Displays logs for a specific service.

### Executes a command

```bash
docker-compose exec webserver ls -l
```
Executes a command in a running service container.

### Builds services

```bash
docker-compose build
```
Builds or rebuilds services defined in your `docker-compose.yml` file.


### Restarts services

```bash
docker-compose restart
```
Restarts services.


### Displays configuration

```bash
docker-compose config
```
Validates and displays the configuration of your `docker-compose.yml` file.

### Pauses services

```bash
docker-compose pause
```
Pauses all services. Containers remain running, but they stop processing requests.

```bash
docker-compose unpause
```
Unpauses services after they have been paused.

```bash
docker-compose top
```
Displays the running processes of a service.

### Scales service

```bash
docker-compose scale webserver=3
```
Scales a service to the specified number of instances.

### Display events

```bash
docker-compose events
```
### docker compose config

Parse, resolve and render compose file in canonical forma

```bash
docker-compose config
```

Streams real-time events from your services.


## Docker commands Summary
### Basic Commands

- `docker run [image]`: Start a new container from an image
- `docker ps`: List all running containers
- `docker stop [container]`: Stop a running container
- `docker rm [container]`: Remove a container
- `docker images`: List all available images
- `docker pull [image]`: Download an image from a registry
- `docker push [image]`: Upload an image to a registry
- `docker build [options] [path]`: Build an image from a Dockerfile

### Advanced Commands

- `docker exec [container] [command]`: Run a command inside a running container
- `docker-compose up`: Start a Docker Compose application
- `docker network [subcommand]`: Manage Docker networks
- `docker volume [subcommand]`: Manage Docker volumes
- `docker logs [container]`: View the logs of a container
- `docker inspect [container]`: Inspect a container
- `docker diff [container]`: Show changes to the filesystem of a container
- `docker commit [container] [image]`: Create a new image from a container's changes
- `docker save [image]`: Save an image to a tar archive
- `docker load`: Load an image from a tar archive

## References

- [Overview of docker compose CLI](https://docs.docker.com/compose/reference/){:target="_blank"}

<!-- 

- <https://docs.docker.com/get-started/docker_cheatsheet.pdf> 

-->