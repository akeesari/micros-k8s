# Create your first pipeline - for .NET Core Web API

## Introduction:

In this lab we are going to create an azure DevOps pipeline for our first Microservice built using .NET Core Web API. This pipeline will  automate the continuous integration and continuous deployment (CI/CD) of build, test, and deployment process of a .NET Core Web API. 


## Technical Scenario

As a DevOps Engineer, you've been asked to create to create a new Azure DevOps pipeline using YAML for .NET Core Web API, this pipeline provides a powerful and flexible way to automate the build and deployment process of .NET Core Web API applications. It allows developers to focus on writing code while the pipeline takes care of the rest.


## Prerequisites

- Azure DevOps account  
- Source code repository
- Azure subscription
- Docker image

<!-- - **A Kubernetes cluster:** You will need a Kubernetes cluster to deploy your Helm chart to. 
- **Helm installed:** You will need to have Helm installed on your local machine. 
- **Azure CLI installed:** You will need to have the Azure CLI installed on your local machine. 
- **A basic understanding of Kubernetes:** You should have a basic understanding of Kubernetes concepts such as Pods, Deployments, Services, ConfigMaps, and Secrets.
- **An application to deploy:** You will need an application that you want to deploy to your AKS cluster. This could be a containerized application, a set of microservices, or any other application that can run on Kubernetes.
- **Azure Container Registry (ACR)** for storing the application image: You will need to have an Azure Container Registry (ACR) where you can store the container image for your application. -->

  
## Implementation details

In this exercise we will accomplish & learn how to implement following:

- Step-1: Create the pipeline
- Step-2: Build environment
- Step-3: Restore dependencies
- Step-4: Build your project
- Step-5: Run your tests
- Step-6: Collect code coverage
- Step-7: Publish artifacts to Azure Pipelines
- Step-7: Deploy a web app


## Step-1: Create the pipeline

Before following the create pipeline wizard instructions, let's quickly create a new file called `azure-pipelines.yaml` in the root folder of our first Microservice and commit this change.


Follow these instructions for creating a new Azure DevOps pipeline:

- Open your Azure DevOps account and navigate to the project where you want to create the pipeline.
- Click on the `Pipelines` menu and then click on the `Create pipeline` button.
- Select `Azure repos git` YAML 
- Select the source code repository where your .NET Core Web API code is hosted. 
- Select the source code repository where your .NET Core Web API code is hosted. 
- Select `Select an existing YAML file`
- Select the branch that you want to use for the pipeline. You can either choose the main branch or a specific branch.
- Choose the template for your pipeline. in this case we are going to use choose an existing template.
- Click on `continue` button
- Click on the Save from `Run` dropdown list.

That's it! You now have an Azure DevOps pipeline for your .NET Core Web API. your can configure the pipeline according to your needs. You can customize the pipeline by adding or removing stages, jobs, and tasks.


## Step-2: Build environment
``` yaml
trigger:
  branches:
    include:
    - refs/heads/develop
  paths:
   include:
     - aspnet-api/*
resources:
  repositories:
  - repository: self
    type: git
    ref: refs/heads/$(Build.SourceBranchName)
```
## Step-3: Restore dependencies
``` yaml
```
## Step-4: Build your project
``` yaml
```
## Step-5: Run your tests
``` yaml
```
## Step-6: Collect code coverage
``` yaml
```
## Step-7: Publish artifacts to Azure Pipelines
``` yaml
```
## Step-7: Deploy a web app
``` yaml
```

## Reference

- <https://learn.microsoft.com/en-us/azure/devops/pipelines/ecosystems/dotnet-core?view=azure-devops&tabs=dotnetfive>
