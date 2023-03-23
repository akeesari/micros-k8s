# Table of Contents

**Introduction**

- Developer Workstation Configuration
- Download & Install software (workstation setup)
- Technology Stack

**Chapter-1: Getting started with Microservices**

- Getting started with Microservices
    - Task-1: Identity list of business domains / microservices
    - Task-2: Identity list of git repos needed
    - Task-3: Identity list of Applications needed
    - Task-4: Create new Azure DevOps Organization
    - Task-5: Create new Azure DevOps Project
- Create your first Microservice with .NET Core
    - Task-1: Create a new repo in azure DevOps
    - Task-2: Clone the repo from azure DevOps
    - Task-3: Create a new .NET Core Web API project
    - Task-4: Test the new .NET core Web API project
    - Task-5: Add Docker files to the API project
    - Task-6: Docker Build & Run
    - Task-7: Push docker container image to ACR
    - Task-8: Pull docker container image from ACR
- Create your second Microservice with Node JS
<!-- - Create third Microservice with Java -->
- Create your first website using ASP.NET Core MVC
- Create your second website using React JS
- Create your third website using Blazor
- Create your first database with SQL server
- Create your second database with PostgreSQL

**Chapter 2: Azure Cloud- Infrastructure as Code (IaC)**

- Infrastructure as Code (IaC) Overview
- Azure Naming & Tagging Conventions
- Create Azure Free Account
- Create new subscription
- Registered resource providers
- Create Terraform Foundation Part-1
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
- Create Terraform Foundation Part-2
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
- Create Log Analytics Workspace using terraform
    - Task-1: Configure terraform variables for Log Analytics workspace
    - Task-2: Create new resource group for Log Analytics workspace
    - Task-3: Create Log Analytics workspace using terraform
    - Task-4: Validate Log Analytics workspace in the Azure portal
    - Task-5: Lock the resource group
- Create Virtual Network using terraform
    - Task-1: Define and declare virtual network variables
    - Task-2: Create a resource group for virtual network
    - Task-3: Create Hub virtual network using terraform
    - Task-4: Create Spoke virtual network using terraform
    - Task-5: Create Diagnostics Settings for Networking
    - Task-6: Lock the resource group
- Create Azure Container Registry (ACR) using terraform
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
- Create Azure Kubernetes Service (AKS) using terraform
    - Task-1: Define and declare AKS variables
    - Task-2: Create a resource group for AKS
    - Task-3: Create AKS user assigned identity
    - Task-4: Create Azure Kubernetes Services (AKS) using terraform
    - Task-5: Create Diagnostics Settings for AKS
    - Task-6: Review AKS Cluster resource in the portal
    - Task-7: Validate AKS cluster running Kubectl
    - Task-8: Allow AKS Cluster access to Azure Container
    - Task-9: Lock the resource group
- Create Storage Account using terraform
- Create Azure Bastion Host using terraform
- Create Virtual Machine (Jumpbox) using terraform
- Create Azure Key Vault using terraform
- Create PostgreSQL Server & databases using terraform
- Configure Private Endpoint & Private Links using terraform
- Create Application Gateway using terraform
- Azure DNS setup

**Chapter 3: Kubernetes - Azure Kubernetes Service (AKS)**

- Prepare an application for Azure Kubernetes Service (AKS)
- Deploying an application to Azure Kubernetes Service (AKS)
    - Step-1: Create a new namespace
    - Step-1: Create a Kubernetes deployment manifest
    - Step-2: Create a Kubernetes service manifest
    - Step-3: Apply the manifests to your AKS cluster
    - Step-4: Port forwarding
    - Step-5: Expose the service externally
    - Step-6: Verify that the application is running
- Working with AKS cluster using kubectl
    - Login to azure & set subscription
    - Connect to AKS cluster
    - Test workloads in AKS
    - Cluster-info
    - cluster resources 
    - Cluster details
    - kubectl commands
    - Troubleshooting errors
    - install kubelogin
- Setup NGINX ingress controller in Kubernetes cluster
    - Step-1: Configure Terraform providers
    - Step-2: Create a new namespace for ingress
    - Step-2: Install ingress resources with helm-chart using terraform
    - Step 3. Verify Ingress-Nginx resources in AKS.
    - Step 4: Deploy sample applications for Ingress testing
    - Step-5: Create an ingress route
    - Step-6: Test the ingress controller
    - Step-7: Add DNS recordset in DNS Zone
- Kubernetes Pod troubleshooting
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

- ArgoCD Introduction
    - Introduction
    - What is GitOps?
    - GitOps core Components
    - GitOps Operators
    - What is ArgoCD?
    - How ArgoCD works?
    - Why do we need to use ArgoCD?
    - Key Features of ArgoCD
    - ArgoCD Architecture Components
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
  
**Chapter 7: Miscellaneous - Commands & cheat-sheets**

- Git cheat-sheet
- Docker cheat-sheet
- Terraform cheat-sheet
- CLI cheat-sheet
- Kubectl cheat-sheet
- Helm cheat-sheet
- ArgoCD cheat-sheet
- az acr Cheat Sheet