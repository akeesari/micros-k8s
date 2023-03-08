# Chapter-1: Getting started with Microservices

## Introduction

Getting started with microservices requires a solid understanding of the architecture and the technology stack, as well as careful planning and implementation of the individual services. With the right approach, however, microservices can help you build flexible, scalable, and maintainable applications.

In general building true microservices architecture needs good understand and theoretical and practical knowledge, I am not going to talk much about the theory part of the Microservices here instead I'll focus on how to plan and create on sample Microservices.

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
- **Task-3:** Identity list of applications needed
- **Task-4:** Create DevOps Organization - Lab
- **Task-5:** Create DevOps Project - Lab
  
## Task-1: Identity list of business domains / microservices

One of the complex part of the Microservices architecture is, identifying the list of microservices needed. Services should be organized around business capabilities, and each service should have a clear and well-defined responsibility.

use the below link to understand and analyze your business domains and define the bounded context for your Microservices architecture etc..

<https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis>

Since my focus here is not implementing any real world microservices scenarios in this book but let's take hypothetical scenario and assume you've 3 domains like below:

- Domain-1
- Domain-2
- Domain-3

assuming you've finalized your business domains at this point so that the rest of the design and implementation will depend on these domains.

## Task-2: Identity list of git repos needed

Once you know the list of domains or microservices needed for your application then, it is time to understand how many git repos you need for your application. 

there are multiple ways to organize the source code and pipelines in the azure DevOps git. it is all depends on how you want to manage your source code and pipelines for your microservices architecture, how easy to maintain in the future.

My preference here is to create separate git repo for each domain or Microservice.  

For example:

- Organization Name (Organization1)
  - Project Name (Project1)
    - Repo-1 (Domain1 specific)
        - APIs - create one or more APIs with separate folder
        - Websites -create  one or more websites with separate folder
        - Databases - create one or more databases with separate folder
    - Repo-2 (Domain2 create specific)
        - APIs - create one or more APIs with separate folder
        - Websites - create one or more APIs with separate folder
        - Databases - create one or more APIs with separate folder
    - Repo-3 (Domain3 specific)
        - APIs - create one or more APIs with separate folder
        - Websites - create one or more APIs with separate folder
        - Databases - create one or more APIs with separate folder
  - Project Name(Project2 completely different project sources)
    - Repo-1 (Domain1)
        - APIs - create one or more APIs with separate folder
        - Websites - create one or more APIs with separate folder
        - Databases - create one or more APIs with separate folder

## Task-3: Identity list of Applications needed

Use this task to identity list of microservices applications needed for your projects. 

`Choose your technology stack:`

Microservices can be built using a variety of technology stacks. Some popular choices include .NET Core, Node.js, Java, and Python etc..


Here is the list of applications we will be creating to get the feeling of Microservices pattern. Here we purposely selected different technology stack and languages to cover more learning scenarios.


- Create the first Microservice with .NET Core Web Rest API with C#
    - name of the api is `aspnet-api`
- Create the second Microservice with Node JS (Node)
    - name of the api is `nodejs-api`
- Create the third Microservice with Java 
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


## Task-4: Create new Azure DevOps Organization

To create a new Azure DevOps organization, follow these steps:

1. Sign in to Azure DevOps. - <https://dev.azure.com>
2. Click on `New organization` in the left nav.
![image.jpg](images/image-8.jpg)
3. Enter name of the Organization and create new organization.
![image.jpg](images/image-9.jpg)

Once you have completed these steps, you will have a new Azure DevOps organization that is ready for use. You can then invite members to join your organization and start working on projects.

## Task-5: Create new Azure DevOps Project

1. Sign in to the Azure DevOps website <https://dev.azure.com/> with your Azure DevOps account.

2. Click on the `Create a project` button.

3. Enter a name for your project and select a process template. The process template determines the default work item types, source control repository, and other settings for your project.

4. Click the `Create project` button to create your new project.

5. Follow the on-screen prompts to configure your project settings, including source control, work item types, and team members.

6. When you are finished, click the `Finish` button to complete the project creation process.

Project Name - `Microservices`

Description - `Microservices project will be used to roll out sample microservices applications for demonstrating microservices architecture.`


![image.jpg](images/image-10.jpg)

That's it! we have created new organization in azure DevOps and created new project so that we can start working on Microservices in the next labs.


## References

- <https://microservices.io/patterns/microservices.html>
- <https://learn.microsoft.com/en-us/azure/architecture/guide/architecture-styles/microservices>
- <https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/create-organization?view=azure-devops>
- <https://learn.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser>