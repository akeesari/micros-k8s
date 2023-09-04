## Introduction

Getting your application ready to run on Azure Kubernetes Service (AKS) involves a series of steps to ensure a smooth deployment. Here's an overview of the process:

**1. Containerize the Application:**
   - Start by containerizing your application. Containerization allows your application to run consistently across different environments.
   
**2. Push the Image to Container Registry:**
   - To make your container image accessible to AKS, you need to store it in a container registry like Azure Container Registry (ACR).

## Prerequisites 

- Create and clone Microservices Repository
- Create a new Project (.NET core web API)
- Add Dockerfile

As part of the Microservice module first chapter we've already created our first Microservice with .NET Core, containerized the application and pushed to ACR. follow the instruction provided in that chapter and implement following steps to preparing your application ready for Azure Kubernetes Service.

[Create first Microservice with .NET Core](../microservices/aspnet-api.md) 




















- **Step-1:** Create Dockerfile in API project
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