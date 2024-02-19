# **Dockerfile Commands**

## Introduction
In this article, I am going to present a comprehensive cheat sheet of commonly used `Dockerfile` commands

## Dockerfile

A Dockerfile is a text document that contains instructions for building a Docker image. Docker can automatically build images by interpreting instructions from a Dockerfile. This page outlines the commands available for use within a Dockerfile."

## 1. FROM
Specifies the base image for your Docker image.
```Dockerfile
FROM <image>[:<tag>] [AS <name>]
```
Example:
```Dockerfile
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
```

## 2. RUN
Executes commands in the shell of the container.
```Dockerfile
RUN <command>
```
Example:
```Dockerfile
RUN apt-get update && apt-get install -y \
    git
```

## 3. COPY
Copies files or directories from the build context to the container's filesystem.
```Dockerfile
COPY <src> <dest>
```
Example:
```Dockerfile
COPY . /app
```

## 4. WORKDIR
Sets the working directory for any RUN, CMD, ENTRYPOINT, COPY, and ADD instructions that follow it.
```Dockerfile
WORKDIR /path/to/directory
```
Example:
```Dockerfile
WORKDIR /app
```

## 5. CMD
Specifies the default command to run when the container starts.
```Dockerfile
CMD ["executable", "param1", "param2"]
```
Example:
```Dockerfile
CMD ["dotnet", "MyApi.dll"]
```

## 6. ENTRYPOINT
Specifies the command to run when the container starts, allowing arguments to be passed.
```Dockerfile
ENTRYPOINT ["executable", "param1", "param2"]
```
Example:
```Dockerfile
ENTRYPOINT ["dotnet", "MyApi.dll"]
```

## 7. EXPOSE
Informs Docker that the container listens on specific network ports at runtime.
```Dockerfile
EXPOSE <port> [<port>/<protocol>...]
```
Example:
```Dockerfile
EXPOSE 80
```

## 8. ENV
Sets environment variables.
```Dockerfile
ENV <key> <value>
```
Example:
```Dockerfile
ENV ASPNETCORE_ENVIRONMENT=Production
```

## 9. ARG
Defines build-time variables.
```Dockerfile
ARG <name>[=<default value>]
```
Example:
```Dockerfile
ARG CONNECTION_STRING
```

## 10. VOLUME
Creates a mount point and marks it as holding externally mounted volumes from native host or other containers.
```Dockerfile
VOLUME /path/to/volume
```
Example:
```Dockerfile
VOLUME /var/log/app
```

## 11. LABEL
Adds metadata to an image.
```Dockerfile
LABEL <key>=<value> <key>=<value> ...
```
Example:
```Dockerfile
LABEL maintainer="John Doe <john@example.com>"
```

## 12. USER
Sets the user or UID to use when running the image.
```Dockerfile
USER <username | UID>
```
Example:
```Dockerfile
USER appuser
```

## 13. HEALTHCHECK
Defines a command to periodically check the container's health.
```Dockerfile
HEALTHCHECK [OPTIONS] CMD <command>
```
Example:
```Dockerfile
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost/health || exit 1
```

## 14. ONBUILD
Adds a trigger instruction when the image is used as the base for another build.
```Dockerfile
ONBUILD <INSTRUCTION>
```
Example:
```Dockerfile
ONBUILD COPY . /app
```

## 15. STOPSIGNAL
Sets the system call signal that will be sent to the container to exit.
```Dockerfile
STOPSIGNAL signal
```
Example:
```Dockerfile
STOPSIGNAL SIGTERM
```

## 16. SHELL
Overrides the default shell used for the shell form of commands.
```Dockerfile
SHELL ["executable", "parameters"]
```
Example:
```Dockerfile
SHELL ["/bin/bash", "-c"]
```

## CMD vs ENTRYPOINT


CMD and ENTRYPOINT are used to specify the default command to run when a container is started. However, they have different behaviors and can be used together in different ways depending on the requirements of your Docker image.

- **CMD**:

  - Sets default command and/or parameters.
  - Can be overridden from the command line.
  - Last `CMD` instruction takes effect if multiple are present.

- **ENTRYPOINT**:

  - Specifies main executable to run.
  - Allows arguments to be passed.
  - Arguments passed to `docker run` are appended to the `ENTRYPOINT` command.
  - Last `ENTRYPOINT` instruction takes effect if multiple are present.

**Best Practices:**

- Use **CMD** for default command and parameters.
- Use **ENTRYPOINT** for main executable, allowing additional arguments.

Example:

```Dockerfile
ENTRYPOINT ["dotnet", "MyApi.dll"]
```

In this example, `dotnet MyApi.dll` is the main executable, with any additional arguments passed when running the container.

## COPY vs ADD


COPY and ADD are used to copy files and directories from the host machine into the container's filesystem. While they have similar functionalities, there are some differences between them.

*COPY Instruction:*

The COPY instruction copies files or directories from the build context (i.e., the directory containing the Dockerfile) into the container's filesystem. It can copy local files/directories as well as files/directories from URLs. However, it does not support extracting files from compressed archives (e.g., .tar.gz).


*ADD Instruction:*

The ADD instruction has the same functionality as COPY, but it also supports additional features such as extracting compressed archives (e.g., .tar.gz) and copying files from URLs. However, because of these additional features, it's considered less predictable and is recommended to use COPY instead unless the extra functionality of ADD is specifically required.

| Feature              | COPY                                       | ADD                                        |
|----------------------|--------------------------------------------|--------------------------------------------|
| Functionality        | Copies files/directories from build context| Same as COPY, plus supports additional features like extracting compressed archives and copying files from URLs|
| Predictability       | More predictable and straightforward       | Provides additional functionality but less predictable|
| Best Practice        | Preferred for basic file copying tasks     | Use sparingly, only when additional features are needed|

In summary, `COPY` is preferred for basic file copying tasks due to its predictability, while `ADD` offers additional functionality but should be used with caution.

<!-- 
##  Summary

These are some of the most commonly used Dockerfile commands, these can be modified as per specific use cases and advanced scenarios.

1. `FROM`: Specifies the base image for subsequent instructions.
2. `COPY`: Copies files or directories from the build context into the image.
3. `ADD`: Similar to `COPY`, but can also fetch files from remote URLs and extract tar archives.
4. `RUN`: Executes commands in the shell within the image.
5. `WORKDIR`: Sets the working directory for subsequent instructions.
6. `ENV`: Sets environment variables in the image.
7. `EXPOSE`: Declares the ports on which the container listens for connections.
8. `CMD`: Specifies the default command to run when the container starts.
9. `ENTRYPOINT`: Configures the primary command to execute when the container starts.
10. `ARG`: Defines build-time variables.
11. `LABEL`: Adds metadata to the image.
12. `VOLUME`: Creates a mount point and marks it as externally mounted.
13. `USER`: Sets the user or UID to use when running the container.
14. `STOPSIGNAL`: Sets the system call signal that will be sent to the container to stop it.
15. `HEALTHCHECK`: Defines a command to check the container's health status.
16. `SHELL`: Specifies the default shell used for executing commands.
17. `MAINTAINER`: Sets the author field of the generated images.
18. `ONBUILD`: Adds a trigger instruction for when the image is used as a base for another build. -->

## References

- [Dockerfile reference](https://docs.docker.com/engine/reference/builder/#overview){:target="_blank"}

<!-- 

- <https://docs.docker.com/get-started/docker_cheatsheet.pdf> 

-->