<!-- # Module 2: Infrastructure as Code (IaC) -->

## Overview
This is the second module in the series, In this second module you will learn on how to create the infrastructure in azure cloud, the azure resources and azure infrastructure created as part of this module will be used for deploying your microservices architecture created in module-1.

Creation of these Azure resources will be completely automated with IaC approach using Terraform configuration and azure DevOps pipelines; 

Let's quickly talk about Infrastructure as Code (IaC) in few line here. 

`Infrastructure as Code (IaC)` is an approach to managing IT infrastructure in which the infrastructure is defined and managed using code, rather than through manual configuration. IaC is a set of practices and tools that allow developers and operations teams to automate the process of deploying and managing infrastructure.

With IaC, infrastructure is defined using either Terraform, YAML or JSON, which describes the desired state of the infrastructure. This code is then stored in a version control system like Azure DevOps, allowing teams to collaborate on changes and track changes over time. The code can then be used to provision infrastructure resources, such as servers, networks, and storage, databases in an automated and repeatable way.

Some benefits of IaC include:

- Faster deployment: IaC allows for rapid and consistent deployment of infrastructure, reducing the time it takes to set up and configure environments.
- Improved reliability: By automating the deployment and management of infrastructure, IaC reduces the risk of human error and ensures consistency across environments.
- Increased scalability: IaC enables teams to easily scale infrastructure resources up or down as needed, without manual intervention.
- Better collaboration: By storing infrastructure code in version control, teams can collaborate on changes and track changes over time.
- Greater agility: IaC allows for more agile development and deployment, making it easier to iterate quickly and respond to changing requirements.

There are several tools available for implementing IaC, including Terraform, AWS CloudFormation, and Ansible. These tools allow teams to define infrastructure as code and manage the deployment and configuration of infrastructure resources in an automated and repeatable way.

Here we've chosen Terraform because of following:

Terraform is a popular infrastructure as code tool that enables users to define, provision, and manage infrastructure resources across multiple cloud provider. Here are some advantages of Terraform over other tools:

- Multi-cloud support: Terraform supports multiple cloud providers, including AWS, Azure, Google Cloud, and more. This allows users to manage resources across multiple providers using a single configuration language and tool.
- Declarative syntax: Terraform uses a declarative syntax to define infrastructure resources. This makes it easy to understand and maintain infrastructure code over time, as well as to collaborate on changes with team members.
- Infrastructure as code best practices: Terraform follows infrastructure as code best practices, such as version control, code review, and testing, making it easier to manage infrastructure as part of a software development process.
- Modularity and reusability: Terraform allows users to define reusable modules that can be shared across multiple infrastructure projects. This promotes code reuse and simplifies the process of managing infrastructure resources.
- Community support: Terraform has a large and active community of users who contribute to the development of the tool, as well as share best practices, tips, and modules. This provides a wealth of resources and support for users of the tool.

Overall, Terraform provides a powerful and flexible tool for managing infrastructure as code that is well-suited for complex and multi-cloud environments. Its declarative syntax, modularity, and community support make it a popular choice for teams looking to adopt infrastructure as code best practices.

## Azure service summary
 
Here is the list of high level azure resources used for running Microservices architecture; since these labs cover only implementation details it is always recommended to read the relevant documentation on particular service before start any labs so that is supper easy for work on these labs. you will see the links end of each lab for reference.

**Log Analytics workspace** 

A Log Analytics workspace allows us to log data from Azure Monitor and other Azure services. we are going to use this resource for storing and analyzing log data from various sources created as part of the Microservices architecture need.


**Virtual network.** 

All the services created in our labs will be completely secured using private Virtual Network. will show you Hub & Spoke vnet model in the lab.

**Azure Container Registry(ACR)** 

We are going to use Container Registry to store all our private Docker images, which are deployed to the cluster. AKS can authenticate with Container Registry using its Azure AD identity. 

**Application Gateway**

This is our entry point to from public DNS, Application Gateway route client requests to the right backend services. This routing provides a single endpoint for clients, and helps to decouple clients from services. reverse proxy, load balancer, SSL / TLS termination, Listeners, backend pools will be configured in this resource.

**Azure Kubernetes Service (AKS)** 

Azure Kubernetes Service (AKS): AKS is a fully managed Kubernetes service that provides a platform for deploying and scaling containerized applications. It is a popular choice for deploying Microservices in Azure due to its scalability, resilience, and ease of management. AKS is used for deploying all our microservices.

**Azure Database** 

Azure Database for PostgreSQL - Flexible Server is a fully managed database service for running PostgreSQL on the Azure managed platform. we will create this PostgreSQL - Flexible Server to allow us to easily create, configure, and manage PostgreSQL databases on Azure.


**Azure Key Vault**

We will use the Azure Key Vault which is a cloud-based service for securely storing and managing cryptographic keys, certificates, and other secrets related to microservices applications.

**Azure Redis Cache**

Azure Redis Cache is a fully managed, in-memory data store that is based on the open-source Redis cache. we will use this service for caching application specific data for fast data access and low latency. 

**Azure Storage account**

Azure Storage account is a Microsoft cloud storage service that allow us to store and retrieve large amounts of unstructured data there are different places we are going to use blob storage, will learn more on this during the labs.

**Azure Active Directory** 

AAD will be used for managing Identity and authentication between two services.

**Azure Pipelines** 

Azure Pipelines are part of the Azure DevOps Services and run automated builds, tests, and deployments.

**Helm Chart** 

Helm is a package manager for Kubernetes, we will use helm charts for deploying our Microservices into AKS.

**ArgoCD** 

ArgoCD is continuous delivery tool for Kubernetes applications. It provides a simple and automated way to manage and deploy applications to a Kubernetes cluster by using GitOps methodology. we will use the ArgoCD for deploying our Microservices into AKS.

**Azure Security & Governance**

Apart from above services we will also focus on following services:

- `Azure Policy`: a service that allows us to create, assign, and manage policies for our Azure resources, ensuring that they comply with organizational standards and regulatory requirements.

- `Azure Active Directory (AAD)`: a service that provides identity and access management (IAM) capabilities, enabling us to secure access to out Azure resources.

- `Azure Monitor`: a service that allows us to collect, analyze, and act on telemetry data from our Azure resources and applications, enabling us to detect and diagnose security issues.

## Reference

<https://learn.microsoft.com/en-us/azure/architecture/reference-architectures/containers/aks-microservices/aks-microservices>
