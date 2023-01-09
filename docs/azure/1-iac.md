# Chapter 2: Infrastructure as Code (IaC)

## Overview
In this second chapter you will learn on how to create the infrastructure in azure cloud, the azure resources and azure infrastructure created as part of this chapter will be used for deploying your microservices architecture created in chapter-1.

Creation of these Azure resources will be completely automated using Terraform configuration and azure pipelines; there are multiple ways to Create or automate the azure resources but here we've chosen Terraform configuration so that we can get all the benefits cloud agnostic.
 
Here is the list of high level azure resources used for running Microservices architecture; since these labs cover only implementation details it is always recommended to read the MSDN documentation on perticular service before start any labs so that is supper easy for work on these labs. you will see the links end of each lab for reference.

**Azure Kubernetes Service (AKS)** 

AKS is a managed Kubernetes cluster hosted in the Azure cloud. AKS is used for deploying all our microservices.

**Virtual network.** 

All the services created in our labs will be completely secured using private Virtual Network.

**Application Gateway**

This is our entry point to from public DNS, Application Gateway route client requests to the right backend services. This routing provides a single endpoint for clients, and helps to decouple clients from services.


Aggregate multiple requests into a single request between the client and the backend.

Offload functionality from the backend services, such as SSL termination.

**Azure Active Directory.** 

AAD is used for managing Identity and authentication between two services.


**Azure Container Registry.** 

Used Container Registry to store all our private Docker images, which are deployed to the cluster. AKS can authenticate with Container Registry using its Azure AD identity. 


**Azure Pipelines.** 

Azure Pipelines are part of the Azure DevOps Services and run automated builds, tests, and deployments.

**Helm.** 

Helm is a package manager for Kubernetes, we used helm charts for deploying our Microservices into AKS.

**Ingress.** 

Without Ingress we can't expose any our URLs, Ingress is created using YAML files in helm charts, We used Application Gateway Ingress Controller (AGIC) ad-on with AKS for managing Routes.


**Azure Monitor.** 

Monitoring is enabled in our services and also making sure that  Azure Monitor collects and stores metrics and logs, application telemetry, and platform metrics for the Azure services created in all our labs. 



# Reference:

https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices
