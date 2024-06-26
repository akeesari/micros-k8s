# Table of Contents

<!-- **Introduction** -->

- [About the Author](../gettingstarted/about-author.md)
- [Acknowledgments](../gettingstarted/acknowledgments.md)
- [Introduction](../gettingstarted/introduction.md)
<!-- - Developer Workstation Configuration
- Download & Install software (workstation setup)
- Technology Stack -->

[**Chapter-1: Getting started with Microservices**](../microservices/0.gettingstarted.md)

- [Lab-1: Getting started with Microservices](../microservices/0.gettingstarted.md)
    - Task-1: Identify Microservices List:
    - Task-2: Identify the List of Git Repositories Needed
    - Task-3: Create new Azure DevOps Organization
    - Task-4: Create new Azure DevOps Project
- [Lab-2: Create your first Microservice with .NET Core](../microservices/1.aspnet-api.md)
    - Step-1: Create a new repo in azure DevOps
    - Step-2: Clone the repo from azure DevOps
    - Step-3: Create a new .NET Core Web API project
    - Step-4: Test the new .NET core Web API project
    - Step-5: Add Dockerfiles to the API project
    - Step-6: Docker Build & Run
    - Step-7: Push docker container to ACR
    - Step-8: Pull docker container from ACR
- [Lab-3: Create your second Microservice with Node JS](../microservices/2.node-api.md)
    - Step-1: Create a new Node JS API Project
    - Step-2: Test the Node JS API project
    - Step-3: Add Dockerfiles to the Node JS API
    - Task-4: Docker Build locally
    - Step-5: Docker Run locally
    - Step-6: Push docker container to ACR
- [Lab-4: Create your first website using ASP.NET Core MVC](../microservices/3.aspnet-app.md)
    - Step-1: Create a new ASP.NET Core Web App (MVC project)
    - Step-2: Test the new ASP.NET core Web App project
    - Step-3: Update home page contents [Optional]
    - Step-4: Add Dockerfiles to the MVC project
    - Task-5: Docker Build locally
    - Step-6: Docker Run locally
    - Step-7: Push docker container to ACR
- [Lab-5: Create your second website using React JS](../microservices/4.react-app.md)
    - Step-1: Install Node.js and NPM
    - Step-2: Create a new React JS application
    - Step-3: Add Dockerfiles to the MVC project
    - Step-4: Docker Build locally
    - Step-5: Docker Run locally
    - Step-6: Push docker container to ACR
<!-- - Create your third website using Blazor -->
- [Lab-6: Create your first database with SQL server](../microservices/6.sqlserver-db.md)
    - Introduction
    - Databases concepts
    - SQL Server Management Studio (SSMS)
    - Create Azure SQL Server
    - Creating your first database
    - Creating Tables
    - Inserting Data
    - Retrieving Data
    - Deleting Data
    - Dropping a Database
- [Lab-7: Create your second database with PostgreSQL](../microservices/7.postgresql-db.md)

[**Chapter 2: Azure Cloud- Infrastructure as Code (IaC)**](../azure/1-iac.md)

- [Lab-1: Infrastructure as Code (IaC)](../azure/1-iac.md) 
    - Overview
    - Infrastructure as Code (IaC)
    - Hight level Architecture
    - Benefits of IaC
    - Terraform Benefits
- [Lab-2: Microservices Architecture on AKS](../azure/1-microservices-architecture-on-aks.md)
    - High Level Architecture
    - Azure Reference Architecture Components 
- [Lab-3: Azure Account & Subscription Management](../azure/3-azure-account-subscription.md)
    - Create New Azure Account
    - Create New Azure Subscription
- [Lab-4: Create Terraform Foundation Part-1](../azure/6-tf-foundation-1.md)
    - Task-1: Create a new project in azure DevOps
    - Task-2: Create a new Azure DevOps Repo for terraform
    - Task-3: Clone the git repo
    - Task-4: Add .gitignore file in the git repo for terraform
    - Task-5: Create Terraform Service Principle Credentials
    - Task-6: Create new resource group
    - Task-7: Create new storage account & container
    - Task-8: Create new Key vault
    - Task-9: Create secrets in Key Vault
    - Task-10: Setup Access Policy in Key Vault
    - Task-11: Configure Service Principal Role Assignment
- [Lab-5: Create Terraform Foundation Part-2](../azure/6-tf-foundation-2.md)
    - Task-1: Create terraform environment variables
    - Task-2: Create terraform providers
    - Task-3: Configure terraform backend state
    - Task-4: Create terraform variables
    - Task-5: Create locals file
    - Task-6: Create azure resource group
    - Task-7: Store terraform commands
    - Task-8: Initialize Terraform
    - Task-9: Setup Terraform workspace
    - Task-10: Create a Terraform execution plan
    - Task-11: Apply a Terraform execution plan
    - Task-12: Verify the results
    - Task-13: Verify terraform state file.
- [Lab-6: Create Log Analytics Workspace using terraform](../azure/7-log-analytics-workspace.md)
    - Task-1: Configure terraform variables for Log Analytics workspace
    - Task-2: Create new resource group for Log Analytics workspace
    - Task-3: Create Log Analytics workspace using terraform
    - Task-4: Validate Log Analytics workspace in the Azure portal
    - Task-5: Lock the resource group
- [Lab-7: Create Virtual Network using terraform](../azure/8-vnet.md)
    - Task-1: Define and declare virtual network variables
    - Task-2: Create a resource group for virtual network
    - Task-3: Create Hub virtual network using terraform
    - Task-4: Create Spoke virtual network using terraform
    - Task-5: Create Diagnostics Settings for Networking
    - Task-6: Lock the resource group
- [Lab-8: Create Azure Container Registry (ACR) using terraform](../azure/9-acr.md)
    - Task-1: Define and declare ACR variables
    - Task-2: Create a resource group for ACR
    - Task-3: Create ACR user assigned identity
    - Task-4: Create Azure Container Registry (ACR) using terraform
    - Task-5: Create Diagnostics Settings for ACR
    - Task-6: Lock the resource group
    - Task-7: Validate ACR resource
        - Task-7.1: Log in to registry
        - Task-7.2: Push image to registry
        - Task-7.3: Pull image from registry
        - Task-7.4: List container images
    - Task-8: Restrict Access Using Private Endpoint
        - Task-8.1: Configure the Private DNS Zone
        - Task-8.2: Create a Virtual Network Link
        - Task-8.3: Create a Private Endpoint Using Terraform
        - Task-8.4: Validate private link connection using `nslookup` or `dig`
- [Lab-9: Create Azure Kubernetes Service (AKS) using terraform](../azure/11-aks.md)
    - Task-1: Define and declare AKS variables
    - Task-2: Create a resource group for AKS
    - Task-3: Create AKS user assigned identity
    - Task-4: Create Azure Kubernetes Services (AKS) using terraform
    - Task-5: Create Diagnostics Settings for AKS
    - Task-6: Review AKS Cluster resource in the portal
    - Task-7: Validate AKS cluster running Kubectl
    - Task-8: Allow AKS Cluster access to Azure Container
    - Task-9: Lock the resource group
- [Lab-10: Create Azure PostgreSQL - Flexible Server using terraform](../azure/12-postgresql.md)
    - Task-1: Define and declare PostgreSQL - Flexible Server variables
    - Task-2: Create an Azure resource group for PostgreSQL
    - Task-3: Create an Azure Virtual Network
    - Task-4: Create an Azure Subnet for PostgreSQL
    - Task-5: Create a Private DNS zone for PostgreSQL
    - Task-6: Associate PostgreSQL Private DNS zone with virtual network
    - Task-7: Generate PostgreSQL admin random password & store in Key Vault
    - Task-8: Create Azure PostgreSQL - Flexible Server using Terraform
    - Task-9: Configure Diagnostic Settings for Azure PostgreSQL - Flexible Server
    - Task-10: Set a user or group as the AD administrator for a PostgreSQL Flexible Server
    - Task-11: Create new Databases in PostgreSQL Server
    - Task-12: Create AD groups for database access
- [Lab-11: Create Azure Key Vault using Terraform](../azure/13-key-vault.md)
    - Task-1: Define and declare Azure Key vault variables
    - Task-2: Create Azure Key Vault using Terraform
    - Task-3: Configure diagnostic settings for Azure Key Vault using terraform
    - Task-4: Configure access policy for developer in Azure Key Vault
    - Task-5: Restrict Access Using Private Endpoint
        - Task-5.1: Configure the Private DNS Zone
        - Task-5.2: Create a Virtual Network Link
        - Task-5.3: Create a Private Endpoint Using Terraform
        - Task-5.4: Validate private link connection using `nslookup` or `dig`
    - Task-6: Created azure Key Vault secrets for sensitive information
- [Lab-12: Create Azure Cache for Redis using Terraform](../azure/14-redis-cache.md)
    - Task-1: Define and declare Azure Cache for Redis variables
    - Task-2: Create Azure Cache for Redis using terraform
    - Task-3: Configure diagnostic settings for Azure Cache for Redis using terraform    
    - Task-4: Securing an Azure Cache for Redis instance
        - Task-4.1: Create private DNS zone for Redis Cache using terraform
        - Task-4.2: Create virtual network link to associate redis private DNS zone to vnet
        - Task-4.3: Configure private endpoint for Azure Cache for Redis using terraform
        - Task-4.4: Validate private link connection using `nslookup` or `dig`  
- [Lab-13: Create Front Door and CDN profile using Terraform](../azure/17-cdn-frontdoor.md)
    - Task-1: Define and Declare Azure Front Door & CDN variables.
    - Task-2: Create a Front Door CDN profile using Terraform.
    - Task-3: Create a Front Door Endpoint using Terraform.
    - Task-4: Create a Front Door Origin Group using Terraform.
    - Task-5: Create a Front Door Origin using Terraform.
    - Task-6: Create custom domains for the Front Door using Terraform.
    - Task-7: Create a Front Door Route using Terraform
    - Task-8: Create a DNS TXT (temporary) record in DNS Zone
    - Task-9: Create DNS CNAME records in DNS Zone
    - Task-10: Configure diagnostic settings for CDN profile
    - Task-11: Apply lock on Front Door Profile
- [Lab-14: Create Storage Account using terraform](../azure/15-storage-account.md)
    - Task-1: Define and declare azure storage account variables
    - Task-2: Create azure storage account using terraform
    - Task-3: Create azure storage account container using terraform
    - Task-4: Configure diagnostic settings for azure storage account using terraform
    - Task-5: Configure diagnostic settings for azure storage account container using terraform
    - Task-6: Create storage account's Files Share using terraform
    - Task-7: Restrict Access Using Private Endpoint
        - Task-7.1: Configure the Private DNS Zone
        - Task-7.2: Create a Virtual Network Link Association
        - Task-7.3: Create Private Endpoints for azure Storage
        - Task-7.4: Validate private link connection using nslookup or dig
- [Lab-15: Azure Event Hubs for Apache Kafka Introduction Part-1](../azure/16-event-hubs-part-1.md)
    - What is Azure Event Hub?
    - Components of Azure Event Hubs
    - Why Azure Event Hubs?
    - Can you explain Apache Kafka on Azure Event Hubs?
    - Azure Event Hubs vs Apache Kafka
    - What is Azure Event Hubs namespace?
    - What are Partitions?
    - What are Shared Access Policies?
    - What are Event publishers?
    - What are Event consumers?
    - What is Event retention?
    - Explain Capture events:
    - Consumer groups
    - Application groups
    - What is Avro format in Apache Kafka?
    - What is Azure Schema Registry?
- [Lab-15: Create Azure Event Hubs for Apache Kafka using Terraform Part-2](../azure/16-event-hubs-part-2.md)
    - Task-1: Define and declare azure event hubs variables.
    - Task-2: Create storage account resources using terraform.
    - Task-3: Create Kafka azure event hubs namespace.
    - Task-4: Create diagnostic settings for event hub namespace.
    - Task-5: Shared access policies for event hub namespace Level
        - Task-5.1: Create shared access policy rule for listen.
        - Task-5.2: Create shared access policy rule for send.
        - Task-5.3: Create shared access policy rule for manage.
    - Task-6: Restrict access using private endpoint & virtual network.
        - Task-6.1: Configure the private DNS zone
        - Task-6.2: Create a virtual network link association
        - Task-6.3: Create private endpoints for azure event hubs
        - Task-6.4: Validate private link connection using nslookup or dig
    - Task-7: Create azure event hubs or Kafka topics.


- Lab-16: Create Virtual Machine (Jumpbox) using terraform
- Lab-17: Configure Private Endpoint & Private Links using terraform
- Lab-18: Azure DNS setup

[**Chapter 3: Kubernetes - Azure Kubernetes Service (AKS)**](../kubernetes/0.getting-started.md)

- [Prepare an application for Azure Kubernetes Service (AKS)](../kubernetes/1-prepare-app.md)
- [Deploying an application to Azure Kubernetes Service (AKS)](../kubernetes/2-deploy-api.md)
    - Step-1: Create a new namespace
    - Step-1: Create a Kubernetes deployment manifest
    - Step-2: Create a Kubernetes service manifest
    - Step-3: Apply the manifests to your AKS cluster
    - Step-4: Port forwarding
    - Step-5: Expose the service externally
    - Step-6: Verify that the application is running
- [Working with AKS cluster using kubectl](../kubernetes/3-working-with-aks.md)
    - Login to azure & set subscription
    - Connect to AKS cluster
    - Test workloads in AKS
    - Cluster-info
    - cluster resources 
    - Cluster details
    - kubectl commands
    - Troubleshooting errors
    - install kubelogin
- [Setup NGINX ingress controller in AKS using Terraform](../kubernetes/4-ingress-controller-nginx.md)
    - Step-1: Configure Terraform providers
    - Step-2: Create a new namespace for ingress
    - Step-2: Install ingress resources with helm-chart using terraform
    - Step 3. Verify Ingress-Nginx resources in AKS.
    - Step 4: Deploy sample applications for Ingress testing
    - Step-5: Create an ingress route
    - Step-6: Test the ingress controller
    - Step-7: Add DNS recordset in DNS Zone
- [Kubernetes Pod troubleshooting](../kubernetes/pod-troubleshooting.md)
    - Login to Azure
    - Connect to Cluster
    - Verify the pod status
    - Describe the pod
    - Check the logs
    - Debug the container
    - Exit from the container
    - Install curl
    - Curl internal pod URL
    - Restart the pod:

**Chapter 4: Azure DevOps - Build & Release Pipelines**

- Azure DevOps Overview
- Create new service connections
- Create Pipeline for .NET Core Web API
- Create Pipeline for ASP.NET Core Website
- Create Pipeline for Node JS API
- Create Pipeline for React JS website
- Create Pipeline for Database deployment

**Chapter 5: Argo CD - Deployments to Kubernetes**
- [Getting started with Argo CD](../argocd/1-argocd-intro.md)
    - What is GitOps?
    - Core components of GitOps
    - What is ArgoCD?
    - How ArgoCD works?
    - Why do we need to use ArgoCD?
    - Key features of ArgoCD
    - ArgoCD architecture components
    - ArgoCD supports tools
- Install ArgoCD
    - Step-1: Configure Terraform providers
    - Step-2: Create namespace for ArgoCD
    - Step-3: Install argocd in AKS with helm-chart using terraform
    - Step 4. Verify ArgoCD resources in AKS.
    - Step-5. port forwarding for login screen
    - Step-6. ArgoCD login with localhost
- Install ArgoCD CLI
    - Step 1.Install ArgoCD CLI
    - Step 2. Access the ArgoCD API Server
    - Step 3. Login to ArgoCD
    - Step 4. Logout ArgoCD
- Registering an AKS Cluster with ArgoCD
- Deploy your first application with ArgoCD
    - Task-1: Creating ArgoCD application using ArgoCD CLI
    - Task-2: Creating ArgoCD application using ArgoCD UI
    - Task-3: Creating ArgoCD application using YAML manifest
- Create new ArgoCD Project
- Create new Repository in ArgoCD
- Helm-Charts Introduction
    - Introduction
    - What is Helm Chart?
    - When do we need to use helm chart?
    - key features of Helm
    - How it works?
    - Explain Helm Architecture

<!-- - Overview
- Install Argocd helm charts using terraform
- Install Argocd helm charts without terraform
- Install Argocd CLI
- Create new Project & Repositories
- Create or deploy your first argocd application
- Registering an AKS Cluster with Argo CD
- Create project / folder structure for Argo CD
- Create Kubernetes deployment in Argo CD
- Create Kubernetes service in Argo CD
- Create Kustomization in Argo CD
- Create Ingress in Argo CD
- Automatically create multiple applications in Argo CD
- Argo CD Cheat-Sheet -->
  
**Chapter 6: Helm-Charts - Deployments to Kubernetes**

- Install Cert-Manager & Let's Encrypt helm charts on Kubernetes
- Install argocd helm chart using terraform
- Install pgadmin4 helm chart using terraform
- Install minio helm chart using terraform
- Helm hooks examples in Kubernetes
<!--   
**Chapter 7: Miscellaneous - Commands & cheat-sheets**

- Git cheat-sheet
- Docker cheat-sheet
- Terraform cheat-sheet
- CLI cheat-sheet
- Kubectl cheat-sheet
- Helm cheat-sheet
- ArgoCD cheat-sheet
- az acr Cheat Sheet 
-->