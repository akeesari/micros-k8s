## Introduction

Part-1 of the terraform foundation we will setup the terraform environment in Azure DevOps and Azure portal to run terraform configuration in the upcoming labs;

setting up the terraform environment is kind of onetime activity and this help for IaC automation we need create these resource manually in the azure portal for the first time. 

## Objective

In this exercise we will accomplish & learn following:

- Azure DevOps setup
  - Task-1: Create new project in azure DevOps
  - Task-2: Create new Repo for terraform
  - Task-3: Clone a new git repo
  - Task-4: Add terraform ignore file in the git repo
- Terraform Management setup in Azure Portal– manual
  - Task-5: Create new resource group
  - Task-6: Create new storage account & container
  - Task-7: Create new Key vault
  - Task-8: Create Service Principle
  - Task-9: Create secrets in Key Vault
  - Task-10: Setup Access Policy in Key Vault
  - Task-11: Configure Service Principal Role Assignment
- Terraform Management setup – automation

## Prerequisites
  - Download & Install Terraform
  - Download & Install Azure CLI
  - Azure subscription
  - Register resource providers in the subscription [optional] - applicable only in case anything is missing. 
  - Visual studio code

## Implementation details

All the tasks listed above will be performed one by one in this section.

In this lab we will perform some tasks in Azure DevOps web portal and some tasks in Azure Portal.


## Task-1: Create new project in azure DevOps

This new project will be used for IaC, later you can create multiple git repos in this project based on the need.

for example:

- `terraform` - use this repo for terraform configuration 
- `scripts` - use this repo for managing all kinds of scripts like PowerShell, Bash and CLI etc..
- `Bicep` - use this repo for Microsoft Bicep scripts

follow these steps for creating a new project in azure devops.

1. Sign in to your organization in Azure DevOps
1. Select New project.
1. Enter project name & description

Name - `terraform`
Description - `TBD`


## References


- [Create a project in Azure DevOps](https://learn.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser)
- https://learn.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory
