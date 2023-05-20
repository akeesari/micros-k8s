# Docker Cheat Sheet

## Introduction

This cheat sheet includes some of the most commonly used `Docker` commands, b


## Basic Commands

- docker run [image]: Start a new container from an image
- docker ps: List all running containers
- docker stop [container]: Stop a running container
- docker rm [container]: Remove a container
- docker images: List all available images
- docker pull [image]: Download an image from a registry
- docker push [image]: Upload an image to a registry
- docker build [options] [path]: Build an image from a Dockerfile

## Advanced Commands

- docker exec [container] [command]: Run a command inside a running container
- docker-compose up: Start a Docker Compose application
- docker network [subcommand]: Manage Docker networks
- docker volume [subcommand]: Manage Docker volumes
- docker logs [container]: View the logs of a container
- docker inspect [container]: Inspect a container
- docker diff [container]: Show changes to the filesystem of a container
- docker commit [container] [image]: Create a new image from a container's changes
- docker save [image]: Save an image to a tar archive
- docker load: Load an image from a tar archive

## Docker Compose Commands

- docker-compose up: Start a Docker Compose application
- docker-compose down: Stop a Docker Compose application
- docker-compose build: Build Docker Compose services
- docker-compose up --scale [service]=[num]: Scale a service to [num] instances

# Reference

- <https://docs.docker.com/get-started/docker_cheatsheet.pdf>