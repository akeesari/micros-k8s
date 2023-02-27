Preparing an application for Azure Kubernetes Service (AKS) involves several steps. Here's an overview of the process:

- **Containerize the application:** The first step is to containerize your application. there are several steps involved in containerize the application.

- **Push the image to container registry:** Again there are several steps involved in pushing docker container image to ACR, those are listed in [Create first Microservice with .NET Core](../microservices/aspnet-api.md) chapter.

## Prerequisites 

- Create Repo
- Clone repo
- Create Project (.NET core web API)
- Add .gitignore
- Add Dockerfile

As part of the Microservice module first chapter we've already created our first Microservice with .NET Core, containerized the application and pushed to ACR. follow the instruction provided in that chapter and implement following steps to preparing your application ready for Azure Kubernetes Service.

- **Step-1:** Add Docker files to the API project
- **Step-2:** Docker Build & Run
    - docker build
    - docker run
    - run application locally
- **Step-3:** Push your Docker container image to ACR
    - azure Login
    - Login ACR
    - Tag your Docker container image 
    - Push Docker container image
    - Verify the image in ACR

By following these steps, you can prepare your application for deployment to Azure Kubernetes Service (AKS). Once your application is containerized and configured for Kubernetes, you can deploy it to AKS and take advantage of the powerful features and scalability of the platform.


## Reference
- <https://learn.microsoft.com/en-us/azure/aks/tutorial-kubernetes-prepare-acr?tabs=azure-cli>