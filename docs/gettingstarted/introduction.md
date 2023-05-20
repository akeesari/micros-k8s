
**Build and Deploy Microservices Applications on Kubernetes platform using Argocd & Helm**

## Overview

In recent years, microservices architecture has become increasingly popular as a way to build complex applications that are scalable, reliable, and maintainable. Microservices architecture breaks down large applications into smaller, independent services that communicate with each other through APIs. Each service can be developed, deployed, and scaled independently, allowing for greater flexibility and agility.

However, managing microservices can be challenging, particularly when it comes to deployment and scaling. Kubernetes is a powerful tool for managing containerized applications and has become the de facto standard for deploying microservices. Kubernetes provides a robust platform for managing containerized applications, but it can be complex to set up and manage.

In this book, we will show you how to build and deploy microservices applications on a Kubernetes using ArgoCD and Helm. ArgoCD is a powerful continuous delivery tool that simplifies the deployment of Kubernetes applications. Helm is a package manager for Kubernetes that makes it easy to deploy and manage complex applications.

This book `Build and Deploy Microservices Applications on Kubernetes platform using Argocd & Helm` covers the step by step instructions for Developer, DevOps engineer, or IT professional.

You can use this step by step implementation guide as your reference architecture for creating, deploying and managing your microservices architecture with Azure Kubernetes Services (AKS) using Argocd & Helm CI/CD DevOps process.

This reference architecture shows how to create multiple Microservice applications with different technology stack using different programming languages, containerize these microservices and push to Azure Container Registry (ACR) and finally deployed microservices to Azure Kubernetes Service (AKS). You will also learn and understand how to deploy Microservice applications to AKS using Argocd and Helm Charts.

We assumes that you've basic knowledge on Microservices architecture, Kubernetes, Azure DevOps, Argocd and Helm technologies. This reference practical guide covers only labs also called exercises (implementation details) with step by step instruction and few basic definitions, it will not cover any in depth knowledge of the topic, you will see some links end of each lab to understand concepts and in depth knowledge of the topic.

We will start with an introduction to microservices architecture and Kubernetes. We will then cover the basics of containerization and how to use containers to package and deploy microservices. Next, we will introduce ArgoCD and show you how to use it to manage the deployment of your Kubernetes applications. Finally, we will cover Helm and show you how to use it to deploy and manage complex microservices applications.


Whether you are a developer, DevOps engineer, or IT professional, this book will provide you with the knowledge and skills you need to build and deploy microservices applications on a Kubernetes using ArgoCD and Helm. We hope that this book will be a valuable resource for anyone who wants to learn how to build and deploy microservices applications on Kubernetes.

## Organization of this book
Very hight level this book is organized into total 6 Chapters, and each chapter has multiple exercises or labs to complete the chapter. 

- In the first chapter we will create multiple Microservices applications using different technology stack to show the Microservice Architecture pattern. Here we will be creating only basic structure of each application, end to end application implementation will be out of scope. The Microservices applications created in this chapter will be used in the subsequent labs.

- In the second chapter we will create Infrastructure needed for hosting Microservices applications. Here we will create various Azure cloud resource using Terraform. we want to make sure that the infrastructure creation process is completely automated with IaC approach.

- In the third chapter we will talk about docker, container,image, Prepare an application for Azure Kubernetes Service (AKS) deployment, Containerize the application, Deploying an application to Kubernetes and working with AKS cluster.

- In the fourth chapter we will start with Azure DevOps, DevOps best-practices, CI/CD strategy for microservices on Kubernetes, create DevOps pipeline for each Microservices applications.

- In the fifth chapter we will start ArgoCD concepts, Installing ArgoCD, Installing ArgoCD CLI, creating projects, repositories, applications in ArgoCD and deploying Microservices in ArgoCD.

- In the sixth chapter we will start Helm concepts, Create your basic Helm chart, explain frequently used commands and converting basic helm chart for Microservices applications 
 ArgoCD, Installing ArgoCD CLI, creating projects, repositories, applications in ArgoCD and deploying Microservices in AroCD.

<!-- 
This book starts with Getting Started where you will the introduction of this book (current chapter) -->

## Lab format

In each chapter there will be multiple labs, structure of each lab has similar format with sub-heading. let me first explain the purpose of each sub-heading here so that it will be easy to relate when you are actually doing things in each lab

**Introduction**

This section covers very basic concepts and definitions of practical lab, this will help you to quickly hook with the lab and get ready for actions. if you need any in depth knowledge of the topic then look into the links provided at the end of each lab.   

**Technical Scenario**

This section covers what is the purpose of that particular lab, where and how we can use this implementation guide in the real scenario, try to relate this lab scenario with your real world Microservice architecture with Kubernetes.
 

**Prerequisites**

This section covers list of tools or software or any Prerequisites that needs to be installed before start the lab so that you are not stuck in middle of the labs.

For example:

- Azure subscription
- Download and Install Terraform
- Download and Install azure CLI


**Implementation details**

In this section we will cover list of all steps or tasks that will be implemented in that particular lab

for example:

In this exercise we will accomplish & learn how to implement following:


- **Task-1:** Define and declare variables
- **Task-2:** Create a Azure service using Terraform
- **Task-3:** Initialize Terraform
- **Task-4:** Create a Terraform execution plan
- **Task-5:** Apply a Terraform execution plan

**Architecture diagram [Optional]**

In this section you will see the Architecture diagram explaining about the scenario. This section is optional but will try to keep some picture, architecture diagrams are always helps to visualize the scenario and easy to understand how dots are connected. 



**login to Azure**

Verify that you are logged into the right azure subscription before start anything in visual studio code

```
# Login to Azure
az login 

# Shows current Azure subscription
az account show

# Lists all available Azure subscriptions
az account list

# Sets Azure subscription to desired subscription using ID
az account set <subscription_id>
```


Here you will actually see actions,  step by step implementation details will be covered in this section.


example of tasks:

**Task-1: Define and declare variables**

task-1 implementation details goes here

```
source code or commands will be placed here
```

**Task-2: Create a Azure service using Terraform**

task-2 implementation details goes here

```
source code or commands will be placed here
```

**References**

This section will cover list of all the references for further reading and further actions.

for example:

References

- [https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices){:target="_blank"}

## Intended Audience of the book

The intended audience for this book would be software developers, architects, DevOps engineers, and anyone else who is interested in building microservices applications using Kubernetes and related tools.

Specifically, the book may be of interest to individuals who are already familiar with Kubernetes and want to learn how to build and deploy microservices applications on the platform using Argocd and Helm. It may also be of interest to those who are new to Kubernetes and want to learn how to use it for microservices development.

The book may be particularly useful for professionals working in organizations that are transitioning to a microservices architecture or using Kubernetes for container orchestration. It may also be relevant for students or researchers who are interested in learning about modern software development practices.

In general, the book is likely to be most useful for individuals who already have some experience with software development or DevOps, as it assumes a basic level of knowledge in these areas. However, the book may still be accessible to those who are new to these topics and are willing to invest the time and effort required to learn.

## key benefits of reading this book

Some of the key benefits of reading this book will include:

- Practical guidance on building microservices applications: The book provides practical guidance and best practices for building microservices applications using Kubernetes and related tools. Readers will learn how to design, develop, and deploy microservices applications in a Kubernetes environment, using tools like Helm and Argocd.

- Comprehensive coverage of key topics: This book covers a wide range of topics related to microservices development, including containerization, deployment strategies, and more. Readers will gain a comprehensive understanding of the key concepts and techniques involved in building microservices applications on Kubernetes.

- Real-world examples and case studies: This book includes real-world examples and case studies that illustrate how microservices applications can be built and deployed using Kubernetes and related tools. These examples will help readers understand how to apply the concepts and techniques covered in the book to real-world scenarios.

- Hands-on exercises and code samples: This book includes hands-on exercises and code samples that readers can use to practice their skills and build their own microservices applications. These exercises and samples will help readers gain practical experience working with Kubernetes and related tools.

- Guidance on best practices: This book provides guidance on best practices for building microservices applications on Kubernetes. Readers will learn how to design and deploy microservices applications that are scalable, reliable, and maintainable.

Overall, this book provides a comprehensive guide to building microservices applications on Kubernetes, with a focus on practical guidance, real-world examples, and hands-on exercises. Readers will gain the skills and knowledge needed to design, develop, and deploy microservices applications in a Kubernetes environment, using the latest tools and best practices.

