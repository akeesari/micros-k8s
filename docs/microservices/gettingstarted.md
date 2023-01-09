# Chapter-1: Getting started with Microservices

## Introduction

Microservices topic is very big and complex to understand, I am not going to talk much about the theory part of the Microservices instead focus on the planning and labs on Microservices architecture.

I am assuming you've already done your homework on Microservices architecture before reading this. Let's quickly recap very hight level what is Microservice before start the labs.
 

`Microservices` - It is an architectural style for developing modern applications. Microservices allow a large application to be separated into smaller independent parts.

- Microservices architecture =  microservices
- Microservices are small, independent, and loosely coupled. 
- A single small team of developers can write and maintain a service.
- Each service is self-contained 
- Each service should implement a single business capability within a bounded context. 
- Each service is a separate codebase, which can be managed by a small development team.
- Services are responsible for persisting their own data or external state

## Objective


let's do some planning and preparation here for the microservices architecture so that in the future labs we can directly jump into the lab work.

This module will teach you how many microservices we are going to create in subsequent labs and how they are organized. 

In this exercise we will accomplish & learn on following:

- **Task-1:** Identity list of business domains / microservices
- **Task-2:** Identity list of git repos needed 
- **Task-3:** Identity list of applicatios needed
- **Task-4:** Create DevOps Organization - Lab
- **Task-5:** Create DevOps Project - Lab
  
## Task-1: Identity list of business domains / microservices

One of the complext part of the Microservices architecture is, identifing the list of microservices needed.

use the below link to understand and anlyse your business domains and define the bounded context for your Microservices architectrue etc..

https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis

Since my focus here is not implementing any real world microservices scenarios therefore I'll just use domain names like below: 

- Domain-1
- Domain-2
- Domain-3

assuming you've finalized your business domains at this point so that the rest of the design and implementation will be will be easy.

## Task-2: Identity list of git repos needed

once you know the list of domains or microservices needed for your application then, it is time to understand how many git repos you need for your application. 

there are multiple ways to organize the source code and pipelines in the azure devops git. it is all depends on how you want to manage your source code and pipelines for your microservices architecture, how easy to maintain in the future.

My preference here is to create separate git repo for each domain.

For example:

- Organization Name (Organization1/keesari)
  - Project Name (Project1)
    - Repo-1 (Domain1 specific)
        - APIs - one or more APIs with separate folder
        - Websites - one or more websites with separate folder
        - Databses - one or more databases with separate folder
    - Repo-2 (Domain2 specific)
        - APIs
        - Websites
        - Databses
    - Repo-3 (Domain3 specific)
        - APIs
        - Websites
        - Databses
  - Project Name(Project2 completely different project sources)
    - Repo-1 (Domain1)
        - APIs
        - Websites
        - Databses

## Task-3: Identity list of Applications needed

Use this task to identity list of applications needed for your projects. 

Here is the list of applications we will be creating to get the feeling of Microservices pattern. Here we purposely selected different technology stack and languages to cover more learning scenarios.

- Create the first microservice with .NET Core Web Rest API with C#
    - name of the api is `aspnet-api`
- Create the second microservice with Node JS (Node)
    - name of the api is `nodejs-api`
- Create the third microservice with Java 
    - name of the api is `java-api`
- Create the first website with ASP.NET Core MVC (C#)
    - name of the app is `aspnet-app`
- Create the second website with React JS (Node)
    - name of the app is `reactjs-app`
- Create the third website with Blazor (C#)
    - name of the app is `blazor-app`
- Create first database with SQL server
    - name of the database is `sqlserver-db`
- Create second database with PostgreSQL
    - name of the database is `postgresql-db`


## Task-4: Create new Azrue DevOps Organization

To create a new Azure DevOps organization, follow these steps:

1. Sign in to Azure DevOps. - https://dev.azure.com/keesari

2. Click on `New organization` in the left nav.
3. Enter name of the Organization and create new organization.

![select-new-organization.png](images/select-new-organization.png)

Once you have completed these steps, you will have a new Azure DevOps organization that is ready for use. You can then invite members to join your organization and start working on projects.

## Task-4: Create new Azure DevOps Project

1. Sign in to the Azure DevOps website (https://dev.azure.com/) with your Azure DevOps account.

2. Click on the "Create a project" button.

3. Enter a name for your project and select a process template. The process template determines the default work item types, source control repository, and other settings for your project.

4. Click the "Create project" button to create your new project.

5. Follow the on-screen prompts to configure your project settings, including source control, work item types, and team members.

6. When you are finished, click the "Finish" button to complete the project creation process.

![create-new-project.png](images/create-new-project.png)


## Refererences

- https://microservices.io/patterns/microservices.html
- https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices
- https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops
- https://learn.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser