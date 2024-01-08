# Table of Contents

<!-- **Introduction** -->

- [About the Author](../gettingstarted/about-author.md)
- [Acknowledgments](../gettingstarted/acknowledgments.md)
- [Introduction](../gettingstarted/introduction.md)
<!-- - Developer Workstation Configuration
- Download & Install software (workstation setup)
- Technology Stack -->

[**Chapter-1: Building Containerized Microservices**](../microservices/0.gettingstarted.md)

- [Lab-1: Getting started with Microservices](../microservices/0.gettingstarted.md)
- [Lab-2: Docker Concepts](../microservices/0-docker.md)
- [Lab-3: Developing applications inside a Dev Containers](../microservices/0-dev-containers/)
- [Lab-4: Create your first containerized microservice with .NET Core](../microservices/1.aspnet-api.md)
- [Lab-5: Create your second containerized microservice with Node JS](../microservices/2.node-api.md)
- [Lab-6: Create your first containerized website using ASP.NET Core MVC](../microservices/3.aspnet-app.md)
- [Lab-7: Create your second containerized website using React JS](../microservices/4.react-app.md)
- [Lab-8: Create your first database with SQL server](../microservices/6.sqlserver-db.md)
- [Lab-9: Create a container for executing PostgreSQL database scripts](../microservices/7.postgresql-db.md)

[**Chapter-2: Create Azure Infrastructure with Terraform**](../azure/1-iac.md)

- [Lab-1: Infrastructure as Code (IaC)](../azure/1-iac.md) 
- [Lab-2: Microservices Architecture on AKS](../azure/1-microservices-architecture-on-aks.md)
- [Lab-3: Azure Account & Subscription Management](../azure/3-azure-account-subscription.md)
- [Lab-4: Create Terraform Foundation Part-1](../azure/6-tf-foundation-1.md)
- [Lab-5: Create Terraform Foundation Part-2](../azure/6-tf-foundation-2.md)
- [Lab-6: Create Log Analytics Workspace using terraform](../azure/7-log-analytics-workspace.md)
- [Lab-7: Create Virtual Network using terraform](../azure/8-vnet.md)
- [Lab-8: Create Azure Container Registry (ACR) using terraform](../azure/9-acr.md)
- [Lab-9: Create Azure Kubernetes Service (AKS) using terraform](../azure/11-aks.md)
- [Lab-10: Create Azure PostgreSQL - Flexible Server using terraform](../azure/12-postgresql.md)
- [Lab-11: Create Azure Key Vault using Terraform](../azure/13-key-vault.md)
- [Lab-12: Create Azure Cache for Redis using Terraform](../azure/14-redis-cache.md)
- [Lab-13: Create Front Door and CDN profile using Terraform](../azure/17-cdn-frontdoor.md)
- [Lab-14: Create Storage Account using terraform](../azure/15-storage-account.md)
- [Lab-15: Azure Event Hubs for Apache Kafka Introduction Part-1](../azure/16-event-hubs-part-1.md)
- [Lab-16: Create Azure Event Hubs for Apache Kafka using Terraform Part-2](../azure/16-event-hubs-part-2.md)

[**Chapter-3: Prepare Azure Kubernetes Service (AKS) for Microservices**](../kubernetes/0.getting-started.md)

- [Lab-1: Getting Started with Docker Container](../kubernetes/0.getting-started.md)
- [Lab-2: Prepare an application for Azure Kubernetes Service (AKS)](../kubernetes/1-prepare-app.md)
- [Lab-3: Deploying an .NET Core API to Azure Kubernetes Service (AKS)](../kubernetes/2-deploy-api.md)
- [Lab-4: Deploying an ASP.NET Core MVC to Azure Kubernetes Service (AKS)](../kubernetes/2-deploy-app.md)
- [Lab-5: Working with AKS cluster using kubectl](../kubernetes/3-working-with-aks.md)
- [Lab-6: Setup NGINX ingress controller in AKS using Terraform](../kubernetes/4-ingress-controller-nginx.md)
- [Lab-7: Setup Application Gateway ingress controller (AGIC) in AKS using Terraform](../kubernetes/4.1-ingress-controller-agic.md)
- [Lab-8: Setup Cert-Manager in AKS using Terraform](../kubernetes/5-cert-manager.md)
- [Lab-9: Issue the Let's Encrypt SSL Certificate to the Website](../kubernetes/5.1-issue-cert.md)
- [Lab-10: Troubleshooting Problems with Let's Encrypt Certificates](../kubernetes/5.2-cert-troubleshooting.md)
- [Lab-11: Integrating Azure Key Vault with AKS using Terraform](../kubernetes/6-aks-kv-integration.md)
- [Lab-12: Kubernetes Pod troubleshooting](../kubernetes/5.2-cert-troubleshooting.md)
- [Lab-13: Create a new user node pool in AKS](../kubernetes/aks-create-nodepool.md)
- [Lab-14: Upgrade or Resize node pools in AKS](../kubernetes/aks-upgrade-nodepool.md)

**Chapter 4: Azure DevOps - Build & Release Pipelines**

- Azure DevOps Overview
- Create new service connections
- Create Pipeline for .NET Core Web API
- Create Pipeline for ASP.NET Core Website
- Create Pipeline for Node JS API
- Create Pipeline for React JS website
- Create Pipeline for Database deployment

**Chapter 5: Argo CD - Deployments to Kubernetes**

- [Getting started with Argo CD](../argocd/argocd-intro.md)
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