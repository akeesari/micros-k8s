#  Create your first Microservice with .NET Core

## Introduction

This is our first lab in the Microservices architecture series, here we are going to create a simple Restful service using ASP.NET core web API project template. 

This sample demonstrates how to create restful service and build container images using ASP.NET Core Restful APIs. 

<!-- API stands for Application Programing Interface. -->

`ASP.NET Core` is a modern, cross-platform framework for building web applications and APIs. It is a lightweight, high-performance framework that is designed to be modular and flexible, allowing developers to build web applications and APIs that can be easily scaled and modified over time.


## Technical Scenario

As a `Backend (BE)` developer you've been asked to create a Restful service using .NET core Web API which is one of the service in the microservices list.
 
This lab will help you to understand how to start your journey with Microservices Architecture, we will start with basics like creating repo, creating small project and finally containerize that small Microservice we've created and push to the Azure container registry (ACR).

We are basically preparing an application for the Kubernetes deployment. The Microservices we are going to create here in this lab will be used in the subsequent labs like while creating DevOps pipelines or while deploying to Azure Kubernetes services (AKS).

## Objective

In this exercise we will accomplish & learn how to implement following:

- Step-1: Create a new repo in azure DevOps
- Step-2: Clone the repo
- Step-3: Create a new Web API project
- Step-4: Test Web API project
- Step-5: Add Dockerfiles to the project
- Step-6: Build & Test docker container image locally
- Step-7: Publish docker container image to ACR

## Prerequisites

- An Organization in Azure DevOps
- A Project in Azure DevOps
- Create Repository permission
- Git client tool
- Download and install software for .NET development 
- Docker and the VS Code Docker extension
 

## Step-1: Create a new repo in azure DevOps

To create a new repository in Azure DevOps, follow these steps:

1. Login into azure DevOps -  <https://dev.azure.com>
2. Select the project where we want to create the repo
3. Click on `Repos` left nav link
4. From the repo drop-down, select `New repository`
5. In the `Create a new repository` dialog, verify that Git is the repository type and enter a name for the new repository. 
6. You can also add a README and create a `.gitignore` for the type of code you plan to manage in the repo.
7. I'd prefer to use lower case for all repos (one of the best practice)
    - Repo name - `aspnetapi` 

!!! Best-Practice
    Use lower case for all repos in azure DevOps

For example: 

![image.jpg](images/image-11.jpg)

## Step-2: Clone the repo from azure DevOps

To clone a repository from Azure DevOps, you will need to have the Git client installed on your local machine. follow these steps to clone the source code locally:

1. Sign in to the Azure DevOps website <https://dev.azure.com/> with your Azure DevOps account.

2. Navigate to the project that contains the repository you want to clone.

3. Click on the `Repos` tab in the navigation menu.

4. Find the repository you want to clone and click on the `Clone` button.

5. Copy the URL of the repository.

6. Open a terminal window or command prompt on your local machine, and navigate to the directory where you want to clone the repository.

or 

Run the following command to clone the repository:

```
git clone <repository URL>
```

When prompted, enter your Azure DevOps credentials.

The repository will be cloned to your local machine, and you can start working with the code.

Examples:

```
C:\Users\anji.keesari>cd C:\Source\Repos
C:\Source\Repos>git clone https://keesari.visualstudio.com/Microservices/_git/aspnetapi

or

# cloning from main branch for the first time
git clone git clone https://keesari.visualstudio.com/Microservices/_git/aspnetapi  -b main C:\Source\Repos\Microservices\aspnetapi

# cloning from feature branches
git clone https://keesari.visualstudio.com/Microservices/_git/aspnetapi  -b develop C:\Source\Repos\Microservices\aspnetapi

```

Some of the regularly used git commands, may be useful. for more information look into our [Git Cheat-Sheet](../miscellaneous/git-cheat-sheet.md)

```
git init
got add .
git commit -a -m "My fist commit."
git push --set-upstream origin main
git status
git pull
git push
```

<!-- 
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
-->

## Step-3: Create a new .NET Core Web API project

We will be using Visual Studio Code instead of Visual Studio to make things faster and easy and save time and money.

!!! Best-Practice
    Try to use Visual Studio code instead of visual studio.

To create a new .NET Core Web API project, you will need to have the .NET Core SDK installed on your machine. You can download the .NET Core SDK from the .NET website <https://dotnet.microsoft.com/download>.

Once you have the .NET Core SDK installed, follow these steps to create a new .NET Core Web API project:

1. Open a terminal window and navigate to the directory where you want to create your project.
2. Run the `dotnet new` command to create a new .NET Core Web API project:
Let's take a look some useful `dotnet` command before creating the project.
Use this command to get the `dotnet` commands help so that your get idea on how use these commands better. 
```
dotnet --help
```
Use this command to get list of available `dotnet` project templates
```
dotnet new --list
```
output

```
These templates matched your input: 

Template Name                                 Short Name           Language    Tags
--------------------------------------------  -------------------  ----------  -------------------------------------
ASP.NET Core Empty                            web                  [C#],F#     Web/Empty
ASP.NET Core gRPC Service                     grpc                 [C#]        Web/gRPC
ASP.NET Core Web API                          webapi               [C#],F#     Web/WebAPI
ASP.NET Core Web App                          razor,webapp         [C#]        Web/MVC/Razor Pages
ASP.NET Core Web App (Model-View-Controller)  mvc                  [C#],F#     Web/MVC
ASP.NET Core with Angular                     angular              [C#]        Web/MVC/SPA
ASP.NET Core with React.js                    react                [C#]        Web/MVC/SPA
ASP.NET Core with React.js and Redux          reactredux           [C#]        Web/MVC/SPA
Blazor Server App                             blazorserver         [C#]        Web/Blazor
Blazor WebAssembly App                        blazorwasm           [C#]        Web/Blazor/WebAssembly/PWA
Class Library                                 classlib             [C#],F#,VB  Common/Library
Console App                                   console              [C#],F#,VB  Common/Console
.
.
and more....
```
Use this command to actually create new project
```
dotnet new webapi -o aspnetapi

or 
dotnet new webapi -o aspnetapi --no-https -f net7.0

cd aspnetapi

code . 

or 
code -r ../aspnetapi
```
!!! Notes
```
  `-o` parameter creates a directory
  `--no-https` flag creates an app that will run without an HTTPS certificate
  `-f` parameter indicates creation
```
Output
```
C:\WINDOWS\system32>cd C:\Source\Repos

C:\Source\Repos>dotnet new webapi -o aspnetapi
The template "ASP.NET Core Web API" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on C:\Source\Repos\aspnetapi\aspnetapi.csproj...
  Determining projects to restore...
  Restored C:\Source\Repos\aspnetapi\aspnetapi.csproj (in 247 ms).
Restore succeeded.

C:\Source\Repos>cd aspnetapi

C:\Source\Repos\aspnetapi>code .
```
3. Here is the example of adding packages to .net projects.
```
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```
4. Run the following command to restore the project's dependencies:
```
dotnet restore
```

!!! Mac User
    If you're on a Mac with an Apple M1 chip, you need to install the Arm64 version of the SDK before following above commands.

<https://dotnet.microsoft.com/en-us/download/dotnet/7.0>

Check the install typing by running following in terminal

```
dotnet
```

You should see an output similar to the following if the installation is successful

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


## Step-4: Test the new .NET core Web API project

1.  Run the following command to build the project:
 `dotnet build` command will look for the project or solution file in the current directory and compile the code in it. It will also restore any dependencies required by the project and create the output files in the bin directory.
```
dotnet build
```
output
```
Microsoft (R) Build Engine version 17.0.1+b177f8fa7 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  AspNetApi -> C:\Source\Repos\AspNetApi\aspnet-api\bin\Debug\net6.0\AspNetApi.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:01.51
```
2.  Run the following command to start the development server:
 `dotnet run` command will look for the project or solution file in the current directory and compile the code in it. After compiling, it will run the application and any output will be displayed in the console.
```
dotnet run
```
output
```
Building...
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: https://localhost:7136
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5136
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: C:\Source\Repos\AspNetApi\aspnet-api\
```
You will notice the URL in the output, copy the URL and paste it in your favorite browser. you will get a `404 error. ` don’t worry. Just type swagger at the end of the URL and press enter and you will get the following webpage.

![image.jpg](images/image-1.jpg)

- <https://localhost:7136>

- <https://localhost:7136/swagger/index.html> - Swagger URL

- <https://localhost:7136/api/aspnetapi/v1/weatherforecast> - API endpoint URL

If you are able to see this swagger URL in your browser then everything is created and setup as expected.

![image.jpg](images/image-2.jpg)

Use the following command to stop the application in VS Code
```
ctrl + c
```
It is time to push your basic project template source into Azure DevOps Git repo.

!!! Best-Practice
    It is always recommended to push source code changes into git repo before starting the new Step.

Use these git commands to push the source code.

```
git add .
git commit -am "My fist commit - Create Web API project"
git push
```

## Step-5: Add Docker files to the API project

There are multiple way to create `Dockerfile` depending on your code editor. 
Here are the step-by-step instructions for creating a `Dockerfile` in a .NET Core Web API project:

1. First, open your .NET Core Web API project in Visual Studio code or your favorite code editor.
1. Next, create a new file in the root directory of your project and name it Dockerfile (with no file extension).
1. Open the Dockerfile and add the following code to the file:
``` Dockerfile
#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["AspNetApi.csproj", "."]
RUN dotnet restore "./AspNetApi.csproj"
COPY . .
WORKDIR "/src/."
RUN dotnet build "AspNetApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "AspNetApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "AspNetApi.dll"]
```
This code defines a Docker image that is based on the aspnet:6.0 image from Microsoft's container registry. The image is divided into four stages:

   - `base:` sets up the working directory and exposes port 80.
   - `build:` restores the project dependencies, builds the project in Release mode, and copies the build output to the /app/build directory.
   - `publish:` publishes the project in Release mode and copies the published output to the /app/publish directory.
   - `final:` sets the working directory to `/app` and copies the published output from the `publish` stage to the current directory. It also specifies the entry point for the container, which is the `dotnet` command with the name of your project's DLL file.


## Step-6: Docker Build & Run

`docker build` is a command that allows you to build a Docker image from a Dockerfile. The Dockerfile is a text file that contains instructions for Docker to build the image, including the base image to use, the files to include, the commands to run, and the ports to expose.

To build and publish a container image for a .NET Core Web API project, you will need to have Docker installed on your machine. You can download Docker from the Docker website <https://www.docker.com/get-started>.

Once you have Docker installed, follow these steps to build and publish a container image for your .NET Core Web API project:

1. Open a terminal window and navigate to the root of the project.
2. Run the `docker build` command to build the Docker image:
```
docker build -t sample/aspnet-api:20230226.1 .
```
output
```

[+] Building 9.5s (19/19) FINISHED
 => [internal] load build definition from Dockerfile                                                                                                          0.0s 
 => => transferring dockerfile: 878B                                                                                                                          0.0s 
 => [internal] load .dockerignore                                                                                                                             0.0s 
 => => transferring context: 374B  
..            
..
..

 => => naming to docker.io/sample/aspnet-api:20230226.1                                                                                                             
```
Verify the new image
if you open the docker desktop you should be able to see the newly created image there.
![image.jpg](images/image-3.jpg)
3. Run the `docker run` command to start a container based on the image:
```
docker run --rm -p 8080:80 sample/aspnet-api:20230226.1
```
output
```
info: Microsoft.Hosting.Lifetime[14]  
      Now listening on: http://[::]:80
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
      Content root path: /app/
```
Wait for the container to start. You should see output in the terminal indicating that the container is listening on port 80.
Open the docker desktop to see the newly created container in the docker desktop app
![image.jpg](images/image-4.jpg)


Open a web browser and navigate to <http://localhost:8080/api/values> (or whatever URL corresponds to your Web API endpoint) to confirm that the Web API is running inside the Docker container.

use these links for testing when you run docker command from vs code

- <http://localhost:8080/swagger/index.html>
- <http://localhost:8080/api/aspnetapi/v1/heartbeat/ping>
- <http://localhost:8080/api/aspnetapi/v1/weatherforecast>

![image.jpg](images/image-5.jpg)

!!! Best-Practice
    Use the following pattern for docker container image naming conventions 

```
docker build -t projectname/domainname/appname:yyyymmdd.sequence .

example:
docker build -t project1/sample/aspnet-api:20230226 .
```

That's it! You've successfully created a Dockerfile and built a Docker image for your .NET Core Web API project. You can now distribute the Docker image to other machines or deploy it to a cloud service like Azure or AWS.


!!! Tip
    delete all containers & images

Use these commands in case if you need to clean-up the containers or images locally from Docker desktop

```
# To delete all containers including its volumes use,
#     docker rm -vf $(docker ps -aq)

# To delete all the images,
#     docker rmi -f $(docker images -aq)
```


<!-- 
TroubleShooting 

Check the following setting

![image.png](/.attachments/image-3021b7f4-8df3-4149-a49b-a00b2e73aec7.png)

Update port number

![image.png](/.attachments/image-801c4b54-4050-402e-b546-6fa987dce9b5.png)
 -->

## Step-7: Push docker container image to ACR

To publish a Docker container image to Azure Container Registry (ACR), you will need to have the following:

1. Create an Azure Container Registry. If you don't have one, you can create one by following the instructions in the Azure Portal or using Azure CLI.
1. Log in to your Azure Container Registry using the Docker command-line interface. You can do this by running the following command:
``` sh
# azure Login
az login

# set the azure subscription
az account set -s "anji.keesari"

# Log in to the container registry
az acr login --name acr1dev

# To get the login server address for verification
az acr list --resource-group rg-acr-dev --query "[].{acrLoginServer:loginServer}" --output table

# output should look similar to this.

# AcrLoginServer    
# ------------------
# acr1dev.azurecr.io
```
1. `Tag` your Docker container image with the full name of your Azure Container Registry, including the repository name and the version tag. You can do this by running the following command:
```
docker tag sample/aspnet-api:20230226.1 acr1dev.azurecr.io/sample/aspnet-api:20230226.1
```
Use this command to see a list of your current local images
```
docker images
```
output
```
REPOSITORY                             TAG          IMAGE ID       CREATED         SIZE
acr1dev.azurecr.io/sample/aspnet-api   20230226.1   1bab8ba123ca   2 hours ago     213MB
```
1. Push your Docker container image to your Azure Container Registry using the Docker command-line interface. You can do this by running the following command:
```
docker push acr1dev.azurecr.io/sample/aspnet-api:20230226.1
```
output
```
The push refers to repository [acr1dev.azurecr.io/sample/aspnet-api]
a592c2e20b23: Pushed
5f70bf18a086: Layer already exists
d57ad0aaee3b: Layer already exists
aff5d88d936a: Layer already exists
b3b2bd456a19: Layer already exists
2540ef4bc011: Layer already exists
94100d1041b6: Layer already exists
bd2fe8b74db6: Layer already exists
20230226.1: digest: sha256:026ec79d24fca0f30bcd90c7fa17e82a2347cf7bc5ac5d762a630277086ed0d1 size: 1995
```
5. Wait for the push to complete. Depending on the size of your Docker container image and the speed of your internet connection, this may take a few minutes.
5. Verify the newly pushed image to ACR.
``` sh
# List images in registry
az acr repository list --name acr1dev --output table
```
output
```
Result
-------------------------------
mcr.microsoft.com/dotnet/aspnet
mcr.microsoft.com/dotnet/sdk
sample/aspnet-api
```
6. Show the new tags of a image in the acr
``` sh
az acr repository show-tags --name acr1dev --repository sample/aspnet-api --output table
```
output
```
Result
----------
20230220.1
20230226.1
```

That's it! You've successfully pushed your Docker container image to Azure Container Registry. You can now use the Azure Portal or Azure CLI to manage your container images and deploy them to Azure services like Azure Kubernetes Service (AKS).

## Step-8: Pull docker container image from ACR

To pull a Docker container image from Azure Container Registry (ACR), you need to perform the following steps:


1. Log in to your Azure Container Registry using the Docker command-line interface. You can do this by running the following command:
``` sh
# Log in to the container registry
az acr login --name acr1dev
```
1. Pull your Docker container image from your Azure Container Registry using the Docker command-line interface. You can do this by running the following command:
``` sh
docker pull acr1dev.azurecr.io/sample/aspnet-api:20230226.1
```
output
``` 
20230226.1: Pulling from sample/aspnet-api
01b5b2efb836: Already exists
c4c81489d24d: Already exists
95b82a084bc9: Already exists
bb369c4b0f26: Already exists 
c888ac593815: Already exists
14ce87409b2e: Already exists
4f4fb700ef54: Already exists
d15d1be868b7: Already exists
Digest: sha256:026ec79d24fca0f30bcd90c7fa17e82a2347cf7bc5ac5d762a630277086ed0d1
Status: Downloaded newer image for acr1dev.azurecr.io/sample/aspnet-api:20230226.1
acr1dev.azurecr.io/sample/aspnet-api:20230226.1
```
1. Wait for the pull to complete. Depending on the size of your Docker container image and the speed of your internet connection, this may take a few minutes.
![image.jpg](images/image-6.jpg)
1. Verify the recently pulled container image from ACR to make sure it running as expected
``` sh
docker run --rm -p 8080:80 acr1dev.azurecr.io/sample/aspnet-api:20230226.1
```
Test the container image running following URL

![image.jpg](images/image-7.jpg)

<http://localhost:8080/swagger/index.html>

That's it! You've successfully pulled your Docker container image from Azure Container Registry. You can now use the Docker command-line interface to manage your container images and run them locally or deploy them to other environments.


## Reference:

- <https://learn.microsoft.com/en-us/aspnet/core/tutorials/first-web-api?view=aspnetcore-7.0&tabs=visual-studio-code>
- <https://github.com/dotnet/dotnet-docker/blob/main/samples/aspnetapp/README.md>
- <https://code.visualstudio.com/docs/containers/overview>
- <https://code.visualstudio.com/docs/containers/quickstart-aspnet-core>
- <https://code.visualstudio.com/docs/containers/quickstart-node>
- <https://dotnet.microsoft.com/en-us/learn/aspnet/microservice-tutorial/intro>
- <https://learn.microsoft.com/en-us/training/modules/microservices-aspnet-core/?WT.mc_id=dotnet-35129-website>


