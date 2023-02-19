## Introduction

This is the the Part-1 of the terraform foundation, in this lab we will setup the terraform environment in Azure DevOps and Azure portal to run terraform configuration in the upcoming labs;

Terraform environment setup is onetime activity, once this is completed then we are ready for any kind of azure resource creation with IaC automation.

## Objective

In this exercise we will accomplish & learn following:

- **Azure DevOps setup**
    - Task-1: Create a new project in azure DevOps
    - Task-2: Create a new Azure DevOps Repo for terraform
    - Task-3: Clone a new git repo
    - Task-4: Add .gitignore file in the git repo for terraform
- **Terraform Management setup**
    - Task-5: Create new resource group
    - Task-6: Create new storage account & container
    - Task-7: Create new Key vault 
    - Task-8: Create Service Principle 
    - Task-9: Create secrets in Key Vault 
    - Task-10: Setup Access Policy in Key Vault 
    - Task-11: Configure Service Principal Role Assignment 

## Prerequisites
  - Download & Install Terraform
  - Download & Install Azure CLI
  - Azure subscription
  - Register resource providers in the subscription
  - Visual studio code
  - Download & Install Git tools

**Implementation details**

All the tasks listed above will be performed one by one in this section.

In this lab we will perform some tasks in Azure DevOps web portal and some tasks in Azure Portal.


## Task-1: Create a new project in azure DevOps

This new project will be used for IaC, later you can create multiple git repos in this project based on the need.

Project Name - `IaC`

Project Description - This project contains the source code related to Infrastructure as code (IaC), IaC is a method of provisioning and managing infrastructure using code and automation rather than manual configuration.

follow these steps for creating a new project in azure DevOps.

1. Sign in to your organization in Azure DevOps
1. Select New project.
1. Enter project name & description

Once the project is created then you can add following repos for managing all your IaC related scripts or configuration, some of the  example here are:

- `terraform` - you can use this repo for terraform configuration
- `scripts` - you can use this repo for managing all kinds of scripts like PowerShell, Bash and CLI etc..
- `Bicep` - you can use this repo for Microsoft Bicep scripts etc..

![image.jpg](images/image-1.jpg)

## Task-2: Create a new Azure DevOps Repo for terraform

You can use Azure DevOps to create a new Git repository to store and manage terraform configuration source code. Here are the steps to create a new Git repository in Azure DevOps:

- Login into azure DevOps -  <https://dev.azure.com/>
- Select the project where we want to create the repo
- Click on `Repos` left nav link
- From the repo drop-down, select `New repository`
- In the `Create a new repository` dialog, verify that Git is the repository type and enter a name for the new repository. 
- You can also add a README and create a `.gitignore` for the type of code you plan to manage in the repo.

- use lower case for the repos (best practice)
  - repo name - terraform 

for example: 

![image.png](images/image-2.png)

- Initialize the main branch with a README or gitignore

- Create new feature branch (develop) from main

![image.jpg](images/image-3.jpg)

## Task-3: Clone the git repo

There are multiple ways to clone the git repo to your computer.

Here we will use the Git CMD tool, open the app in admin mode and follow these commands to clone the code locally.

```
C:\Users\anji.keesari>cd c:\Source\Repos\IaC

c:\Source\Repos\IaC>git clone https://keesari.visualstudio.com/IaC/_git/terraform
Cloning into 'terraform'...
remote: Azure Repos
remote: Found 4 objects to send. (26 ms)
Unpacking objects: 100% (4/4), 1.22 KiB | 62.00 KiB/s, done.

c:\Source\Repos\IaC>cd terraform

# open the folder in VS code
c:\Source\Repos\IaC\terraform>code .

c:\Source\Repos\IaC\terraform>
```

## Task-4: Add .gitignore file in the git repo for terraform

Update or add (if not existing) .**gitignore** for terraform ignore - this will allow us not to commit unnecessary terraform files in to git repo.

for example: 

``` title=".gitignore"
# Local .terraform directories
**/.terraform/*

# .tfstate files
*.tfstate
*.tfstate.*

# Crash log files
crash.log

# Exclude all .tfvars files, which are likely to contain sentitive data, such as
# password, private keys, and other secrets. These should not be part of version 
# control as they are data points which are potentially sensitive and subject 
# to change depending on the environment.
#
*.tfvars

# Ignore override files as they are usually used to override resources locally and so
# are not checked in
override.tf
override.tf.json
*_override.tf
*_override.tf.json

# Include override files you do wish to add to version control using negated pattern
#
# !example_override.tf

# Include tfplan files to ignore the plan output of command: terraform plan -out=tfplan
# example: *tfplan*

# Ignore CLI configuration files
.terraformrc
terraform.rc
```

**Terraform Management Setup** 

By following these steps, you can set up a Terraform management environment for running Azure resources. It's important to keep your configuration files and state file in a secure location, and to follow best practices for managing infrastructure as code.

since these steps are part of terraform setup itself therefore we can't create following resources using terraform, here we can use either az cli or PowerShell script to create following resources. Here we are going use PowerShell script

## Task-5: Create new resource group

We are going to create separate resource group for managing terraform management specific azure resource like azure storage account and azure key vault.

Resource Group Name - `rg-tfmgmt-dev` 

```
```

## Task-6: Create new storage account & container

We are going to use the azure storage account for storing the terraform remote state. This allows you to store the state file remotely and access it from anywhere, and also provides additional features like versioning, locking, and auditing.

Terraform remote state is a way to store and manage Terraform's state files remotely. By default, Terraform stores the state file on the local file system, but this can cause problems when working in a team or when scaling up infrastructure. Remote state storage provides a centralized location for storing state files, which makes it easier to manage state across teams and across environments.

 Create new storage account

```
```
create new container

```
```

## Task-7: Create new Key vault 

This Key Vault will be used for storing all kind of secrets related terraform management. it is very critical securing secrets like terraform service principle because these are actually used to authenticate Terraform to Azure and create azure resources in the Azure Portal.

```

```

## Task-8: Create Terraform Service Principle Credentials

In Terraform, a service principal is used to authenticate with Azure in order to manage resources. A service principal is like a user account that is used to access Azure resources programmatically, rather than interactively through the Azure portal.

To use a service principal with Terraform, you'll need to provide the client ID and client secret, which are used to authenticate with Azure. Here are the steps to create a service principal and retrieve its credentials:

1. Create a new Azure Active Directory (AD) application: Use the Azure portal or Azure CLI to create a new AD application. This will create a new service principal for you.

1. Assign a role to the service principal: Use the Azure portal or Azure CLI to assign a role to the service principal. This will determine the level of access that the service principal has to Azure resources.

1. Retrieve the client ID and client secret: Use the Azure portal or Azure CLI to retrieve the client ID and client secret for the service principal. The client ID is a unique identifier for the service principal, and the client secret is a password that is used to authenticate with Azure.


```

```
##  Task-9: Create secrets in Key Vault 

Terraform management secrets will be protected by storing in the azure key vault.

```
```

## Task-10: Setup Access Policy in Key Vault 
Terraform management service principle need access to azure key to retrieve the secrets stored in Azure Key Vault. 

##  Task-11: Configure Service Principal Role Assignment

The new Service principle needs at least contributor access at subscription level where azure resources will be created, 
if you want avoid unnecessary issues during resources creation or azure DevOps terraform automation you can even provide owner role at subscription level so that we don't run any issues while creating azure resource from terraform configuration.

```
```



## References


- <https://learn.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser>
- <https://learn.microsoft.com/en-us/azure/devops/repos/git/create-new-repo?view=azure-devops>
- <https://learn.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory>
