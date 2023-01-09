
**Building and Managing Microservices with Kubernetes using Argocd & Helm**

## Overview

This is the step by step implementation developer user guide on `Building and Managing Microservices with Kubernetes using Argocd & Helm`.

You can use this step by step implementation guide as your reference architecture for creating and managing your microservices archicture with Azure Kubernetes Services (AKS) using Argocd & Helm CI/CD devops process.

This reference architecture shows how to create multple microservice applications with different technology stack using different programing languages, containerize these microservices and push to Azure Container Registry (ACR) and finally deployed microservices to Azure Kubernetes Service (AKS).

You will learn and understand how to deploy microservice applications to AKS using Argocd and Helm Charts.

This reference practical step by step guid assumes that you've basic knowledge on Microservices architecture, Kubernetes, Azure DevOps, Argocd and Helm.

This reference practical guide covers only labs also called exercises with step by step implementatiion and few basic definitions, it will not cover any in dept knoweledge of the topic, you will see some links end of each lab to understand concepts and indept knowledge of the topic.

## Labs format

you will notice following sub-headings in each lab, let me first explain the purpose of each sub heading here so that it will be easy to relate when you are actually doing things in each lab

**Introduction**

This section covers very basic concepts and definitions of perticual lab, this will help you to quickly hook with the lab and get ready for actions. if you need any in dept knowledge of the topic then look into the links provided at the end of each lab.   

**Technical Scenario**

This section covers what is the purpose of that perticular lab, where and how we can use this implementation guid in the real scenario, try to relate this lab scenario with real world microservice architectrue with Kubernetes.
 
**Objective**

In this section we will cover list of all steps or tasks that will be implemented in that perticual lab

for example:

In this exercise we will accomplish & learn how to implement following:


- **Task-1:** Define and declare variables
- **Task-2:** Create a Azure service using Terraform
- **Task-3:** Initialize Terraform
- **Task-4:** Create a Terraform execution plan
- **Task-5:** Apply a Terraform execution plan

**Architecture diagram [Optional]**

In this section you will see the Architectrue diagram explaining about the scenario. This section is optional but will try to keep some picture, architecture diagrams are always helps to visualize the scenario and easy to understand how dots are connected. 

**Prerequisites**

This section covers list of tools or software or any Prerequisites that needs to be installed before start the lab so that you are not stuck in middle of the labs.

For example:

- Azure subscription
- Download and Install Terraform
- Download and Install azure CLI

**Implementation details**

Here you will actually see actions, all step by step implementation details will be covered in this section.

`login to Azure`

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
example of tasks:

**Task-1: Define and declare variables**

task-1 implementaion details goes here

```
source code or commands will be placed here
```

**Task-2: Create a Azure service using Terraform**

task-2 implementaion details goes here

```
source code or commands will be placed here
```

**References**

This section will cover list of all the references for further reading and further actions.

## References

- [Reference-architectures/containers/aks-microservices](https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices)
- [Building-and-managing microservices](https://www.oreilly.com/library/view/building-and-managing/9780137649686/)
