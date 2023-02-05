# Chapter 2: Infrastructure as Code (IaC)

In this second chapter you will learn on how to create the infrastructure in azure cloud, the resources and infrastructure created as part of this chapter will be used for deploying your microservices architecture.


Here is the list of labs covered in this chapter:

**1. Introduction**
   - Understand Naming Conventions 
   - Create new azure account
   - Create new subscription

**2. Terraform Foundation Part-1 – Azure setup**

   - Azure DevOps setup
     - Create DevOps project & git repo
     - Clone the repo 
     - Add terraform ignore file
   - Terraform Management setup – manual 
     - Create new resource group
     - Create new storage account & container
     - Create new Key vault 
     - Create Service Principle 
     - Create secrets in Key Vault 
     - Setup Access Policy in Key Vault 
     - Configure Service Principal Role Assignment 
   - Terraform Management setup – automation
 
**3. Terraform Foundation Part-2 – Project setup**

   - Configure Environments/ workspaces
   - Configure Providers setup
   - Create variable definitions 
   - Create variable declaration
   - Setup azure naming conventions 
   - Create your first resource group using Terraform 
   - Terraform execution
     - tf_commands list
     - Terraform workspace setup
     - Run Terraform plan 
     - Run Terraform apply 
     - Commit resource group code

**4. Terraform Foundation Part-3 – Deployment setup**

   - Create new azure service connection 
   - Create variable groups 
   - Link Key vault in Azure DevOps
   - Create Azure DevOps YAML pipeline for Terraform
     - Environment setup  
     - Dev env plan stage
     - Dev env Apply state
     - Test env plan stage
     - Test env Apply state
     - Prod env plan stage
     - Prod env Apply state
   - Create approval policy
   - Create branch policy
   - Configure required permissions for service principle
   - Run the pipeline for the first time

**5. Create Log Analytics workspace using Terraform**

   - Create new resource group    
   - Configure variables
   - Create Log Analytics workspace
     - provider setup  
     - Terraform Plan
     - Terraform Apply
   - Validate the resource in the portal
   - Lock the resource group

**6. Create new Virtual Network using Terraform**

   - Create vnet resource group 
   - Configure variables
   - Create Hub vnet
     - calculate address ranges
     - Create hub vnet subnets
       - gateway subnet
       - application gateway subnet
       - bastion subnet
   - Validate resource
   - Create Spoke vnet
       - Create spoke vnet subnets
       - gateway subnet
       - virtual machine subnet
       - database subnet
       - aks subnet
   - Lock vnet resource group

**7. Create Azure Container Registry (ACR) using Terraform** 

   - Create ACR resource group 
   - Configure variables
   - Create Azure Container Registry
   - Create ACR user assigned identity
   - Create Diagnostics Settings for ACR
   - Validate ACR resource in the portal
   - Lock ACR resource group
  
**8. Create Azure Kubernetes Service (AKS)** 

   - Create a new or use existing resource group 
   - Configure AKS variables
   - Create Admin cluster Group
   - Create AKS cluster using terraform
     - privately enabled
     - Managed Identity enabled
     - Auto Scaling enabled
     - configure log analytics workspace
     - Azure CNI networking
   - Create diagnostics settings for AKS
   - Review AKS Cluster resource in the portal
   - Validate AKS cluster running Kubectl
   - Lock AKS cluster resource group

**9. Create Storage Account** 

   - Create a new or use existing resource group 
   - Configure Storage variables
   - Create Storage Account using terraform
     - Create a container for bash / ps scripts
     - Initialize Terraform
     - Create a Terraform execution plan
     - Apply a Terraform execution plan
   - Create diagnostics settings for AKS
   - Review Storage Account resource in the portal

**10. Create Azure Bastion Host** 

   - Create a new resource group 
   - Define and declare bastion host variables
   - Create a subnet for AzureBastionSubnet
  - Create a Azure Bastion host
    - Allocation method - static
    - Sku - standard
    - IP Based connection - true
  - Create bastion host diagnostic settings 
  - Create bastion host public ip address
  - Create public ip diagnostic settings 
     - Initialize Terraform
     - Create a Terraform execution plan
     - Apply a Terraform execution plan
   - Review bastion host & public ip in the portal

**11. Create Virtual Machine (Jumpbox)**

   - Create a new resource group 
   - Create a virtual network / use existing
   - Define and declare jumpbox variables
   - Create a subnet for VM
   - Create a public IP address (not needed)
   - Create a network security group and SSH inbound rule
     - Diagnostic settings - enable (Pending)
   - Create a virtual network interface card
     - Diagnostic settings - enable (Pending)
   - Connect the network security group to the  network interface
   - Create a storage account for boot diagnostics
   - Create SSH key
   - Create a Linux virtual machine using terraform
     - Use SSH to connect to virtual machine
     - boot_diagnostics_storage_account (Pending)
     - Initialize Terraform
     - Create a Terraform execution plan
     - Apply a Terraform execution plan
   - Review Jummpbox in the portal
   - Use SSH to connect to virtual machine
   - Lock the resource group


**12. Create Azure Key Vault**

   - Define and declare key vault variables
   - Create a Azure key vault using Terraform
     - Initialize Terraform
     - Create a Terraform execution plan
     - Apply a Terraform execution plan
   - Configure key vault access policy
   - Review key vault in the portal
   - Create diagnostic setting for key vault

**13. PostgreSQL database Server**

   - Define and declare variables used in PostgreSQL instance
   - Create a new resource group / use existing rg
   - Create a virtual network / use existing vnet
   - Create a subnet for postgreSQL
   - Define a private DNS zone within an Azure DNS 
   - Define a private DNS zone vnet link
   - Deploy a PostgreSQL Flexible Server on which the database runs using terraform
  - Instantiate an Azure PostgreSQL database
    - Initialize Terraform
    - Create a Terraform execution plan
    - Apply a Terraform execution plan
  
  **13. Azure Event Hub (Kafka)**

  - Define and declare variables used in Event Hub
  - Create new resource group / use existing
  - Create storage account for kafka event storage
  - Storage account container for kafka data store
  - Create kafka event hub Namespace
  - Create shared access policy rules at namespace level
  - Create a azure event hub (receiver topic)
  - Create event hubs consumer groups)
  - Create shared access policy rules at even hub level
    - Initialize terraform
    - Create a terraform execution plan
    - Apply a terraform execution plan
    - Review Kafka event hub in the portal