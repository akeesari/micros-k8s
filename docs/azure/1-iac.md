<!-- # Module 2: Infrastructure as Code (IaC) -->

## Overview
This is the second module in the series, In this second module you will learn on how to create the infrastructure in azure cloud, the azure resources and azure infrastructure created as part of this module will be used for deploying your microservices architecture created in module-1.

Creation of these Azure resources will be completely automated using Terraform configuration and azure pipelines; 

There are multiple ways to Create or automate the azure resources but here we've chosen Terraform configuration so that we can get all the benefits cloud terraform.
 
Here is the list of high level azure resources used for running Microservices architecture; since these labs cover only implementation details it is always recommended to read the relevant documentation on particular service before start any labs so that is supper easy for work on these labs. you will see the links end of each lab for reference.

**Azure Kubernetes Service (AKS)** 

AKS is a managed Kubernetes cluster hosted in the Azure cloud. AKS is used for deploying all our microservices.

**Azure Container Registry(ACR)** 

Used Container Registry to store all our private Docker images, which are deployed to the cluster. AKS can authenticate with Container Registry using its Azure AD identity. 

**Virtual network.** 

All the services created in our labs will be completely secured using private Virtual Network. will show you Hub & Spoke vnet model in the lab.

**Application Gateway**

This is our entry point to from public DNS, Application Gateway route client requests to the right backend services. This routing provides a single endpoint for clients, and helps to decouple clients from services. reverse proxy, load balancer, SSL / TLS termination, Listeners, backend pools will be configured in this resource.


**Azure Database** 

Azure Database for PostgreSQL - Flexible Server is a fully managed database service for running PostgreSQL on the Azure platform. we will create this PostgreSQL - Flexible Server to allow us to easily create, configure, and manage PostgreSQL databases on Azure.


**Key vault**

We will use the Azure Key Vault which is a cloud-based service for securely storing and managing cryptographic keys, certificates, and other secrets.

**Azure Redis Cache**

Azure Redis Cache is a fully managed, in-memory data store that is based on the open-source Redis cache. we wil use this service for caching application specific data for fast data access and low latency. 

**Azure Storage account**

Azure Storage account is a Microsoft cloud storage service that allow us to store and retrieve large amounts of unstructured data.

**Azure Active Directory** 

AAD will be used for managing Identity and authentication between two services.

**Release management (CI & CD)**

We will use Helm-Charts, ArgoCD, Manifests, YAML Pipelines, Kustomization to support the release management.

**Azure Pipelines** 

Azure Pipelines are part of the Azure DevOps Services and run automated builds, tests, and deployments.

**Helm.** 

Helm is a package manager for Kubernetes, we used helm charts for deploying our Microservices into AKS.

**Ingress.** 

Without Ingress we can't expose any our URLs, Ingress is created using YAML files in helm charts, We used Application Gateway Ingress Controller (AGIC) ad-on with AKS for managing Routes.

**Azure Security & Governance**

Apart from above services we wil also focus on following services:

- `Azure Policy`: a service that allows us to create, assign, and manage policies for our Azure resources, ensuring that they comply with organizational standards and regulatory requirements.

- `Azure Active Directory (AAD)`: a service that provides identity and access management (IAM) capabilities, enabling us to secure access to out Azure resources.

- `Azure Monitor`: a service that allows us to collect, analyze, and act on telemetry data from our Azure resources and applications, enabling us to detect and diagnose security issues.

## Reference

https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices
