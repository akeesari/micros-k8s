# Chapter-1.1 Create first microservice with .NET Core

## Introduction

This is our first lab in the Microservices architecture series, here we are going to create a simple Restful service using ASP.NET core web API project template. 

This sample demonstrates how to create restful service and build container images using ASP.NET Core Restful APIs. 

API stands for Application Programing Interface.

ASP.NET Core is a modern, cross-platform framework for building web applications and APIs. It is a lightweight, high-performance framework that is designed to be modular and flexible, allowing developers to build web applications and APIs that can be easily scaled and modified over time.


## Technical Scenario

This lab will help you to understand how start your journey with Microservices Architecture, we will start with basics like creating repo, creating small project and finally containerize that small microservice we've created and push to the Azure container registry (ACR).

we are basically preparing an application for the Kubernetes deployment. the microservice we are going to create here in this lab will be used in the subsequent labs like creating devops pipelines or deploying to Azure Kubernets services (AKS).

## Objective

In this exercise we will accomplish & learn how to implement following:

- Task-1: Create a new repo in azure devops
- Task-2: Clone the repo
- Task-3: Create a new Web API project
- Task-4: Test Web API project
- Task-5: Add Docker files to the project
- Task-6: Build & Test docker container image locally
- Task-7: Publish docker container image to ACR

## Prerequisites

- An organization in Azure DevOps
- A project in Azure DevOps
- Create repository permission
- Download and install software for .NET development 
- Docker and the VS Code Docker extension must be installed
 

## Task-1: Create a new repo in azure devops

To create a new repository in Azure DevOps, follow these steps:

1. Login into azure devops -  https://dev.azure.com/keesari
2. Select the project where we want to create the repo
3. Click on `Repos` left nav link
4. From the repo drop-down, select `New repository`
5. In the `Create a new repository` dialog, verify that Git is the repository type and enter a name for the new repository. 
6. You can also add a README and create a `.gitignore` for the type of code you plan to manage in the repo.
7. use lower case for the repos (best practice)
    - repo name - `aspnetapi` 

For example: 

![image.png](/.attachments/image-1980253c-d30e-4d72-885e-879f1a7cae3e.png)

## Task-2: Clone the repo from azure devops

**Option-1:** 

To clone a repository from Azure DevOps, you will need to have the Git client installed on your local machine. Then, follow these steps:

1. Sign in to the Azure DevOps website (https://dev.azure.com/) with your Azure DevOps account.

2. Navigate to the project that contains the repository you want to clone.

3. Click on the "Repos" tab in the navigation menu.

4. Find the repository you want to clone and click on the "Clone" button.

5. Copy the URL of the repository.

6. Open a terminal window or command prompt on your local machine, and navigate to the directory where you want to clone the repository.

7. Run the following command to clone the repository:

```
git clone <repository URL>
```

When prompted, enter your Azure DevOps credentials.

The repository will be cloned to your local machine, and you can start working with the code.

Examples:

```
C:\Users\anji.keesari>cd C:\Source\Repos
C:\Source\Repos>git clone https://dev.azure.com/keesari/project1/_git/aspnetapi

or

# cloning from main branch for the first time
git clone https://dev.azure.com/keesari/project1/_git/aspnetapi  -b main C:\Source\Repos\aspnetapi

# cloning from feature branches
git clone https://dev.azure.com/keesari/project1/_git/aspnetapi  -b develop C:\Source\Repos\aspnetapi

```

regularly used git commands.

```
git init
got add .
git commit -a -m "My fist commit."
git status
git pull
git push
```

**Option-2:** 

use these commands to add code to remote repository

```
git init

git add .

git commit -m "My fist commit."

git remote add origin https://dev.azure.com/keesari/project1/_git/aspnetapi

git remote -v

git push -f origin master
```

## Task-3: Create a new .NET Core Web API project

Most of the labs we will be using VS Code instead of Visual Studio to make things faster and easy and save time.

**Best Practice** - try to use VS code isntead of visual studio 2022 (or Latest version)

To create a new .NET Core Web API project, you will need to have the .NET Core SDK installed on your machine. You can download the .NET Core SDK from the .NET website (https://dotnet.microsoft.com/download).

Once you have the .NET Core SDK installed, follow these steps to create a new .NET Core Web API project:

1. Open a terminal window and navigate to the directory where you want to create your project.
2. Run the following command to create a new .NET Core Web API project:
3. Navigate to the project directory by running the following command:

```
dotnet new webapi -o aspnetapi
or dotnet new webapi -o aspnetapi --no-https -f net7.0
cd todoapi
code . 
or 
code -r ../aspnetapi
```

Note: 

- `-o` parameter creates a directory
- `--no-https` flag creates an app that will run without an HTTPS certificate
- `-f` parameter indicates creation


for example:
```
C:\WINDOWS\system32>cd C:\Source\Repos

C:\Source\Repos>dotnet new webapi -o todoapi
The template "ASP.NET Core Web API" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\Source\Repos\todoapi\todoapi.csproj...
  Determining projects to restore...
  Restored C:\Source\Repos\todoapi\todoapi.csproj (in 247 ms).
Restore succeeded.

C:\Source\Repos>cd aspnetapi

C:\Source\Repos\todoapi>code .
```

exampel of Adding a package 

```
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```
4. Run the following command to restore the project's dependencies:

```
dotnet restore
```

**Mac User**

If you're on a Mac with an Apple M1 chip, you need to install the Arm64 version of the SDK.

https://dotnet.microsoft.com/en-us/download/dotnet/7.0

Check the install typing following in terminal

```
dotnet
```

expected output, If the installation succeeded, you should see an output similar to the following:

```
anjikeesari@Anjis-MacBook-Pro-2 MyMicroservice % dotnet

Usage: dotnet [options]
Usage: dotnet [path-to-application]

Options:
  -h|--help         Display help.
  --info            Display .NET information.
  --list-sdks       Display the installed SDKs.
  --list-runtimes   Display the installed runtimes.

path-to-application:
  The path to an application .dll file to execute.
```


## Task-4: Test the new .NET core Web API project

1.  Run the following command to build the project:

```
dotnet build
```
2.  Run the following command to start the development server:

```
dotnet run
```

VS code will automatically launch with following URL, port # here is randomly generated.  

https://localhost:7157

http://localhost:5136/WeatherForecast

or 

open the above URL in any of your browsers and you will get a `404 error. `
Don’t worry. Just type swagger at the end of the URL and press enter and you will get the following webpage.

https://localhost:7157/swagger/index.html


for example:

```
C:\Source\Repos\todoapi>dotnet build
Microsoft (R) Build Engine version 17.0.0+c9eb9dd64 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  todoapi -> C:\Source\Repos\todoapi\bin\Debug\net6.0\todoapi.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:03.84

C:\Source\Repos\todoapi>dotnet run
Building...
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:7227
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5257
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\Source\Repos\todoapi\
```

If you are able to see this swagger URL in your browser then everything is created and setup as expected.

![image.png](/.attachments/image-0dc56065-d4a1-4de0-8564-ce1489b97718.png)

Stoping the application in VS Code

```
ctrl + c
```

It is time to push your basic project tempalte source into Azure DevOps Git repo.


**Best Practice:**  It is always recommended to push source code changes into git repo before starting the new task.

Use these commands to push the source code.

```
git add .
git commit -am "My fist commit - Create Web API project"
git push
```

## Task-5: Add Docker files to the API project

To add Dockerfiles to a .NET Core Web API project, follow these steps:

option-1:

1. Make sure that you have Docker installed on your machine. You can download Docker from the Docker website (https://www.docker.com/get-started).

2. In your .NET Core Web API project, create a new file named "Dockerfile" at the root of the project.

3. Add the following lines to the Dockerfile:

``` Docker
FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.csproj .
RUN dotnet restore

# copy and build everything else
COPY . .
RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS runtime
WORKDIR /app
COPY --from=build /app/out ./
ENTRYPOINT ["dotnet", "MyProject.dll"]
```

Option-2:


1. Open the project folder in VS Code.
2. Open Command Palette (`Ctrl+Shift+P`) and issue Docker Images: Build Image... command.
3. Click Yes

![image.png](/.attachments/image-ba1c74e0-9357-444c-a313-8abad0c64137.png)

4. Select ASP.NET Core

![image.png](/.attachments/image-dfcdd208-6fc9-4c28-be25-cafca17068e2.png)

5. Select Linux (recommended)
6. Change the port for application endpoint to `5000`

![image.png](/.attachments/image-23817eed-0ac6-4586-8b59-b1a46c77e2da.png)

7. Dockerfile and .dockerignore files are added to the workspace.

Code will start building at this stage.

for example:

![image.png](/.attachments/image-be6bb7c9-eb9a-40ca-b066-c80026bd6f7c.png)

**Docker file contents**

Here is the Docker file contents, use these commands in case if you need to create the docker file manually instead of automatically creating using VS Code or Visual Studio.

``` Docker
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 5257

ENV ASPNETCORE_URLS=http://+:5257

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["todoapi.csproj", "./"]
RUN dotnet restore "todoapi.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "todoapi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "todoapi.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "todoapi.dll"]

```
## Task-6: Build & publish container image

To build and publish a container image for a .NET Core Web API project, you will need to have Docker installed on your machine. You can download Docker from the Docker website (https://www.docker.com/get-started).

Once you have Docker installed, follow these steps to build and publish a container image for your .NET Core Web API project:

1. Open a terminal window and navigate to the root of the project.

2. Run the following command to build the Docker image:


**docker build**

Run docker build command to build the source code

```
docker build -t projectname/domainname/todo-api:20221127.1 .
```

**Best Practice:**

use this docker build pattern.

`docker build -t projectname/domainname/appname:yyyymmdd.sequence .`


output
```
C:\Source\Repos\todoapi>docker build -t projectname/domainname/todo-api:20221127.1 .
[+] Building 9.5s (19/19) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                          0.0s 
 => => transferring dockerfile: 878B                                                                                                                          0.0s 
 => [internal] load .dockerignore                                                                                                                             0.0s 
 => => transferring context: 374B  
            
...
                                                                                                               
```
Verify the new image

if you open the docker desktop you should be able to see the newely created image there.

![image.png](/.attachments/image-a7743901-4b2d-4e9c-b046-5cbe854297c6.png)

**docker run**

Run the following command to start a container based on the image:

```
docker run --rm -p 8080:80 projectname/domainname/todo-api:20221127.1
```
output
```
C:\Source\Repos\todoapi>docker run --rm -p 8080:80 projectname/domainname/todo-api:20221127.1
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://[::]:5257
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app/

```

Verify the new container

Open the docker desktop to see the newely created container.

![image.png](/.attachments/image-541292db-40a6-442c-87fd-0429b4c73b8d.png)

**delete all containers & images** [optional]


use these commands in case if you need to clean-up the containers or images

```
# To delete all containers including its volumes use,
#     docker rm -vf $(docker ps -aq)

# To delete all the images,
#     docker rmi -f $(docker images -aq)
```

use these links for testing when you run docker command from vs code

http://localhost:8080/swagger/index.html
http://localhost:8080/api/aspnetapi/v1/heartbeat/ping
http://localhost:8080/api/aspnetapi/v1/weatherforecast

Trouble Shooting 

Check the following setting

![image.png](/.attachments/image-3021b7f4-8df3-4149-a49b-a00b2e73aec7.png)

Update port number

![image.png](/.attachments/image-801c4b54-4050-402e-b546-6fa987dce9b5.png)


## Task-7: Publish docker container image to ACR

To publish a Docker container image to Azure Container Registry (ACR), you will need to have the following:

1. Open a terminal window and navigate to the root of the project.

2. Run the following command to build the Docker image:

```
docker build -t myproject .
```
3. Run the following command to tag the image with the ACR login server:
```
docker tag myproject myacrlogin.azurecr.io/myproject:1.0.0
```
4. Run the following command to log in to ACR:
```
az acr login --name myacrlogin
```
5. Run the following command to push the image to ACR:
```
docker push myacrlogin.azurecr.io/myproject:1.0.0
```
6. Run the following command to pull the image from ACR:
```
docker pull myacrlogin.azurecr.io/myproject:1.0.0
```
7. Run the following command to start a container based on the image:
```
docker run -p 5000:80 --rm myacrlogin.azurecr.io/myproject:1.0.0
```

```
az login
az account set -s "subscription name"

# Log in to the container registry

az acr login --name keesarilabdev

# To get the login server address
az acr list --resource-group rg-ACR --query "[].{acrLoginServer:loginServer}" --output table

keesarilabdev.azurecr.io

# Now, tag your local tenant image with the acrLoginServer (keesarilabdev) address of the container registry

docker tag project1/sample/aspnet-api:20220829.1 keesarilabdev.azurecr.io/project1/sample/aspnet-api:20220829.1

# Push images to registry

docker push keesarilabdev.azurecr.io/project1/sample/aspnet-api:20220829.1

# List images in registry
az acr repository list --name keesarilabdev --output table

# show tags of a image in the acr
az acr repository show-tags --name keesarilabdev --repository project1/sample/aspnet-api --output table
```


## Reference:

- https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-7.0&tabs=visual-studio-code
- https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/README.md
- https://code.visualstudio.com/docs/containers/overview
- https://code.visualstudio.com/docs/containers/quickstart-aspnet-core
- https://code.visualstudio.com/docs/containers/quickstart-node
- https://dotnet.microsoft.com/en-us/learn/aspnet/microservice-tutorial/intro
- https://learn.microsoft.com/en-us/training/modules/microservices-aspnet-core/?WT.mc_id=dotnet-35129-website


