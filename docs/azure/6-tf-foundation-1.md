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
    - Task-5: Create terraform Service Principle 
    - Task-6: Create new azure resource group
    - Task-7: Create new azure storage account & container
    - Task-8: Create new azure Key vault 
    - Task-9: Create secrets in azure  Key Vault 
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

We are going to create separate resource group for managing terraform management specific azure resource like azure storage account and azure key vault.


## Task-5: Create Terraform Service Principle Credentials

In Terraform, a service principal is used to authenticate with Azure in order to manage resources. A service principal is like a user account that is used to access Azure resources programmatically, rather than interactively through the Azure portal.

To use a service principal with Terraform, you'll need to provide the client ID and client secret, which are used to authenticate with Azure. Here are the steps to create a service principal and retrieve its credentials:

1. Create a new Azure Active Directory (AD) application: Use the Azure portal or Azure CLI to create a new AD application. This will create a new service principal for you.

1. Assign a role to the service principal: Use the Azure portal or Azure CLI to assign a role to the service principal. This will determine the level of access that the service principal has to Azure resources.

1. Retrieve the client ID and client secret: Use the Azure portal or Azure CLI to retrieve the client ID and client secret for the service principal. The client ID is a unique identifier for the service principal, and the client secret is a password that is used to authenticate with Azure.

az cli commands

``` ps1
az login
az ad app create --display-name <APP_NAME>
az ad app create --display-name "sp-tfmgmt-dev"
az ad app show --id <APP_ID> --query "appId"
az account show --query "tenantId"
```

![image6.jpg](images/image-7.jpg)

## Task-6: Create new resource group

Create new azure resource group using az cli
Resource Group Name - `rg-tfmgmt-dev` 

``` ps1
az group create --name <GROUP_NAME> --location <LOCATION>
az group create -n "rg-tfmgmt-dev"-l "east us"

```

![image6.jpg](images/image-8.jpg)

## Task-7: Create new storage account & container

We are going to use the azure storage account for storing the terraform remote state. This allows you to store the state file remotely and access it from anywhere, and also provides additional features like versioning, locking, and auditing.

Terraform remote state is a way to store and manage Terraform's state files remotely. By default, Terraform stores the state file on the local file system, but this can cause problems when working in a team or when scaling up infrastructure. Remote state storage provides a centralized location for storing state files, which makes it easier to manage state across teams and across environments.

 Create new azure storage account using az cli

``` ps1
az storage account create --name <ACCOUNT_NAME> --resource-group <RESOURCE_GROUP_NAME> --location <LOCATION> --sku <SKU>
az storage account create -n "tfmgmtstates" -g "rg-tfmgmt-dev" -l "east us" --sku "Standard_LRS"

```
![image6.jpg](images/image-9.jpg)

create new storage account container

``` ps1

az storage container create --name <CONTAINER_NAME> --account-name <ACCOUNT_NAME> --account-key <ACCOUNT_KEY>
az storage container create -n "terraformstates" --account-name "tfmgmtstates" --account-key "koB5PQEGX5pEHVAWsyM0efP3aeFsuNhw8dzRXvqrLXXcD12VEIC4HkhNnwDAGWUJcZWb8Q3C8yxZ+AStXGHDGQ=="

```
![image6.jpg](images/image-10.jpg)

## Task-8: Create new Key vault 

This Key Vault will be used for storing all kind of secrets related terraform management. it is very critical securing secrets like terraform service principle because these are actually used to authenticate Terraform to Azure and create azure resources in the Azure Portal.

create new azure key vault using az cli

``` ps1
az keyvault create --name <VAULT_NAME> --resource-group <RESOURCE_GROUP_NAME> --location <LOCATION>
az keyvault create -n "kv-tfstates-dev" -g "rg-tfmgmt-dev" -l "east us"

```
![image6.jpg](images/image-11.jpg)


##  Task-9: Create secrets in Key Vault 

Terraform management secrets will be protected by storing in the azure key vault.

create secretes in azure key vault using az cli

``` ps1
az keyvault secret set --vault-name <VAULT_NAME> --name <SECRET_NAME> --value <SECRET_VALUE>

az keyvault secret set --vault-name "kv-tfstates-dev" --name "tf-subscription-id" --value "1115d52c-5170-4366-b262-cc12cba2d222"
az keyvault secret set --vault-name "kv-tfstates-dev" --name "tf-client-id" --value "11183cf3-7184-457a-a71b-eb5fb7e02222"
az keyvault secret set --vault-name "kv-tfstates-dev" --name "tf-client-secret" --value "1118Q~eQTMTWEwpvK~CHeTIrU7l7xnhw9wE1z222"
az keyvault secret set --vault-name "kv-tfstates-dev" --name "tf-tenant-id" --value "1113c4a0-f87d-46ad-b4be-3ee05cefe222"
az keyvault secret set --vault-name "kv-tfstates-dev" --name "tf-access-key" --value "1115PQEGX5pEHVAWsyM0efP3aeFsuNhw8dzRXvqrLXXcD12VEIC4HkhNnwDAGWUJcZWb8Q3C8yxZ+AStXGHDG222"

```
![image6.jpg](images/image-12.jpg)

## Task-10: Setup Access Policy in Key Vault 
Terraform management service principle need access to azure key to retrieve the secrets stored in Azure Key Vault. 

create azure key vault access policy using az cli

``` ps1
az keyvault set-policy --name <VAULT_NAME> --object-id <OBJECT_ID> --secret-permissions <SECRET_PERMISSIONS> --key-permissions <KEY_PERMISSIONS>
az keyvault set-policy --name "kv-tfstates-dev" --object-id "a68e4529-b584-43c6-9ffd-4ca681da9efc" --secret-permissions get list --key-permissions get list

```

![image6.jpg](images/image-6.jpg)

##  Task-11: Configure Service Principal Role Assignment

The new Service principle needs at least contributor access at subscription level where azure resources will be created, 
if you want avoid unnecessary issues during resources creation or azure DevOps terraform automation you can even provide owner role at subscription level so that we don't run any issues while creating azure resource from terraform configuration.

add owner role assignment to service principle using az cli

``` ps1
az role assignment create --assignee-object-id <SERVICE_PRINCIPAL_OBJECT_ID> --role Owner --scope <SCOPE>

Note:- make sure that you will get the service principle object id here instead of app registration object id
service principle object id will be found in `Enterprise Application`

az role assignment create --assignee-object-id "a68e4529-b584-43c6-9ffd-4ca681da9efc" --role Owner --scope "/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be" 

```
![image6.jpg](images/image-13.jpg)

you can do the above things using re-usable PowerShell script.

Here is the PowerShell function, this will allow you to run in multiple environments.

Let's look at the complete PowerShell file 

``` ps1 title="tf-mgmt.ps1"
<#
.SYNOPSIS
    Configures Azure for secure Terraform access.
.DESCRIPTION
    Configures Azure for secure Terraform access using Azure Key Vault.
    The following steps are automated:
    - Creates an Azure Service Principle for Terraform.
    - Creates a new Resource Group.
    - Creates a new Storage Account.
    - Creates a new Storage Container.
    - Creates a new Key Vault.
    - Configures Key Vault Access Policies.
    - Creates Key Vault Secrets for these sensitive Terraform login details:
       - 'tf-subscription-id'
       - 'tf-client-id'
       - 'tf-client-secret'
       - 'tf-tenant-id'   
       - 'tf-access-key' 
.EXAMPLE
    Connect-AzAccount -UseDeviceAuthentication
    .\scripts\TerraformManagement.ps1 -adminUserDisplayName 'AnjKeesari@gmail.com'
    Displays device login link, then configures secure Terraform access for admin user 'AnjKeesari@gmail.com'
.NOTES
    Assumptions:
    - Azure PowerShell module is installed: https://docs.microsoft.com/en-us/powershell/azure/install-az-ps
    - You are already logged into Azure before running this script (eg. Connect-AzAccount)
    - Use "Connect-AzAccount -UseDeviceAuthentication" if browser prompts don't work.
    - select-AzSubscription -SubscriptionName 'Dev'
#>



[CmdletBinding()]
param (    
    $adminUserDisplayName = "anjkeesari@gmail.com",# This is used to assign yourself access to KeyVault
    $servicePrincipalName = "sp-tfmgmt-dev",
    $resourceGroupName = "rg-tfmgmt-dev",
    $location = "East US",
    $storageAccountSku = "Standard_LRS",
    $storageContainerName = "tfmgmttates",
    $vaultName = "kv-tfstates-dev",
    $storageAccountName = "sttfstatesdev",
    $subscriptionID = "111111-ffec-4773-b939-fc5be0d00222"
)

# Azure login
Write-Host "Checking for an active Azure login..."

$azContext = Get-AzContext

if (-not $azContext) {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw "There is no active login for Azure. Please login first using 'Connect-AzAccount'"
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'


# Service Principle
Write-Host "Checking for an active Service Principle: [$servicePrincipalName]..."

# Get current context
$terraformSP = Get-AzADServicePrincipal -DisplayName $servicePrincipalName
Write-Host "SUCCESS!" -ForegroundColor 'Green'

if (-not $terraformSP) {
    Write-Host "Creating a Terraform Service Principle: [$servicePrincipalName] ..."
    try {
        $terraformSP = New-AzADServicePrincipal -DisplayName $servicePrincipalName -Role 'Contributor' -ErrorAction 'Stop'
        $servicePrinciplePassword = $newSpCredential.secretText
    } catch {
        Write-Host "ERROR!" -ForegroundColor 'Red'
        throw $_
    }
    Write-Host "SUCCESS!" -ForegroundColor 'Green'

} else {
    # Service Principle exists so renew password (as cannot retrieve current one-off password)
    $newSpCredential = $terraformSP | New-AzADSpCredential
    $servicePrinciplePassword = $newSpCredential.secretText
}

# Get Subscription
Write-Host "`nFinding Subscription and Tenant details..."
try {
    $subscription = Get-AzSubscription -SubscriptionID $subscriptionID -ErrorAction 'Stop'
} catch {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw $_
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'

# New Resource Group
if(Get-AzResourceGroup -Name $resourceGroupName -ErrorAction SilentlyContinue)
{  
    Write-Host -ForegroundColor Magenta $resourceGroupName "- Terraform Management Resource Group already exists."  
}  
else  
{  
    Write-Host "`nCreating Terraform Management Resource Group: [$resourceGroupName]..."
    try {
        $azResourceGroupParams = @{
            Name        = $resourceGroupName
            Location    = $location
            Tag         = @{ keep = "true" }
            Force       = $true
            ErrorAction = 'Stop'
            Verbose     = $VerbosePreference
        }
        New-AzResourceGroup @azResourceGroupParams | Out-String | Write-Verbose
    } catch {
        Write-Host "ERROR!" -ForegroundColor 'Red'
        throw $_
    }
    Write-Host "SUCCESS!" -ForegroundColor 'Green'
}


# New storage account
 if(Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName -ErrorAction SilentlyContinue)  
 {  
      Write-Host -ForegroundColor Magenta $storageAccountName "- Storage Account for terraform states already exists."     
 }  
 else  
 {  
    Write-Host "`nCreating Storage Account for terraform states: [$storageAccountName]..."
    try {
        $azStorageAccountParams = @{
            ResourceGroupName = $resourceGroupName
            Location          = $location
            Name              = $storageAccountName
            SkuName           = $storageAccountSku
            Kind              = 'StorageV2'
            ErrorAction       = 'Stop'
            Verbose           = $VerbosePreference
        }
        New-AzStorageAccount @azStorageAccountParams | Out-String | Write-Verbose
    } catch {
        Write-Host "ERROR!" -ForegroundColor 'Red'
        throw $_
    }
    Write-Host "SUCCESS!" -ForegroundColor 'Green'    
}

# Select Storage Container
Write-Host "`nSelecting Default Storage Account..."
try {
    $azCurrentStorageAccountParams = @{
        ResourceGroupName = $resourceGroupName
        AccountName       = $storageAccountName
        ErrorAction       = 'Stop'
        Verbose           = $VerbosePreference
    }
    Set-AzCurrentStorageAccount @azCurrentStorageAccountParams | Out-String | Write-Verbose
} catch {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw $_
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'


# # New Storage Container
# if(Get-AzStorageContainer -Name $storageContainerName -ErrorAction SilentlyContinue)  
# {  
#     Write-Host -ForegroundColor Magenta $storageContainerName "- Storage Container already exists."  
# }  
# else  
# {
#     Write-Host "`nCreating Storage Container: [$storageContainerName]..."
#     try {
#         $azStorageContainerParams = @{
#             Name        = $storageContainerName
#             Permission  = 'Off'
#             # ErrorAction = 'Stop'
#             Verbose     = $VerbosePreference
#         }

#         $storageAccountKey = (Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName)[0].Value
#         $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

#         New-AzStorageContainer @azStorageContainerParams -Context $storageContext | Out-String | Write-Verbose
#     } catch {
#         Write-Host "ERROR!" -ForegroundColor 'Red'
#         throw $_
#     }
#     Write-Host "SUCCESS!" -ForegroundColor 'Green'
# }

# New KeyVault

Write-Host "`nCreating Key Vault for terraform secrets: [$vaultName]..."
if(Get-AzKeyVault -Name $vaultName -ErrorAction SilentlyContinue)  
{  
     Write-Host -ForegroundColor Magenta $vaultName "- Key Vault already exists."  
}  
else  
{
    try {

        Register-AzResourceProvider -ProviderNamespace "Microsoft.KeyVault"
        $azKeyVaultParams = @{
            VaultName         = $vaultName
            ResourceGroupName = $resourceGroupName
            Location          = $location
            ErrorAction       = 'Stop'
            Verbose           = $VerbosePreference
        }
        New-AzKeyVault @azKeyVaultParams | Out-String | Write-Verbose
    } catch {
        Write-Host "ERROR!" -ForegroundColor 'Red'
        throw $_
    }
    Write-Host "SUCCESS!" -ForegroundColor 'Green'
}

# Set KeyVault Access Policy
Write-Host "`nSetting KeyVault Access Policy for Admin User: [$adminUserDisplayName]..."
$adminADUser = Get-AzADUser -DisplayName $adminUserDisplayName
Write-Host "adminADUser = ${adminADUser}" -ForegroundColor 'Green'
try {
    $azKeyVaultAccessPolicyParams = @{
        VaultName                 = $vaultName
        ResourceGroupName         = $resourceGroupName
        UserPrincipalName         = $adminUserDisplayName
        PermissionsToKeys         = @('Get', 'List')
        PermissionsToSecrets      = @('Get', 'List', 'Set')
        PermissionsToCertificates = @('Get', 'List')
        ErrorAction               = 'Stop'
        Verbose                   = $VerbosePreference
    }
    Set-AzKeyVaultAccessPolicy @azKeyVaultAccessPolicyParams -PassThru | Out-String | Write-Verbose
} catch {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw $_
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'

Write-Host "`nSetting KeyVault Access Policy for Terraform SP: [$servicePrincipalName]..."
try {
    $azKeyVaultAccessPolicyParams = @{
        VaultName                 = $vaultName
        ResourceGroupName         = $resourceGroupName
        ObjectId                  = $terraformSP.Id
        PermissionsToKeys         = @('Get', 'List')
        PermissionsToSecrets      = @('Get', 'List', 'Set')
        PermissionsToCertificates = @('Get', 'List')
        ErrorAction               = 'Stop'
        Verbose                   = $VerbosePreference
    }
    Set-AzKeyVaultAccessPolicy @azKeyVaultAccessPolicyParams | Out-String | Write-Verbose
} catch {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw $_
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'


# Terraform login variables
# Get Storage Access Key
$storageAccessKeys = Get-AzStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName
$storageAccessKey = $storageAccessKeys[0].Value # only need one of the keys

$terraformLoginVars = @{
    'tf-subscription-id' = $subscription.Id
    'tf-client-id'       = $terraformSP.appId
    'tf-client-secret'   = $servicePrinciplePassword
    'tf-tenant-id'       = $subscription.TenantId
    'tf-access-key'      = $storageAccessKey
}
Write-Host "`nTerraform login details:"
$terraformLoginVars | Out-String | Write-Host

# Create KeyVault Secrets
Write-Host "`nCreating KeyVault Secrets for Terraform..."
try {
    foreach ($terraformLoginVar in $terraformLoginVars.GetEnumerator()) {
        $AzKeyVaultSecretParams = @{
            VaultName   = $vaultName
            Name        = $terraformLoginVar.Key
            SecretValue = (ConvertTo-SecureString -String $terraformLoginVar.Value -AsPlainText -Force)
            ErrorAction = 'Stop'
            Verbose     = $VerbosePreference
        }
        Set-AzKeyVaultSecret @AzKeyVaultSecretParams | Out-String | Write-Verbose
    }
} catch {
    Write-Host "ERROR!" -ForegroundColor 'Red'
    throw $_
}
Write-Host "SUCCESS!" -ForegroundColor 'Green'

```
## Verify resources

![image6.jpg](images/image-14.jpg)


## References


- <https://learn.microsoft.com/en-us/azure/devops/organizations/projects/create-project?view=azure-devops&tabs=browser>
- <https://learn.microsoft.com/en-us/azure/devops/repos/git/create-new-repo?view=azure-devops>
- <https://learn.microsoft.com/en-us/power-apps/developer/data-platform/walkthrough-register-app-azure-active-directory>
