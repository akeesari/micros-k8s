
## Introduction

Azure `Storage` is a cloud-based storage solution provided by Microsoft Azure, offering a scalable, secure, and highly available storage infrastructure for various types of data. Azure Storage allows users to store and retrieve data in the cloud using various storage services, and at the core of this storage ecosystem is the `Azure Storage Account`.

In this hands-on lab, I'll guide you through the process of creating an Azure Storage Account using Terraform. Additionally, we'll cover the creation of a Storage Account Container and the configuration of diagnostic settings.

Key Features of Azure Storage Account includes:

**Scalability:** Azure Storage Accounts provide a highly scalable solution, enabling users to scale their storage needs as their data requirements grow. The storage capacity can dynamically scale up or down based on demand.

**Durable and Highly Available:** Azure Storage ensures the durability and availability of data through redundancy. Data is replicated across multiple data centers to protect against hardware failures and ensure data integrity.

**Data Redundancy Options:** Users can choose redundancy options such as Locally Redundant Storage (LRS), Geo-Redundant Storage (GRS), Zone-Redundant Storage (ZRS), and more, depending on their specific needs for data redundancy and availability.

**Blob Storage:** Azure Storage supports various data types, and Blob Storage is designed for storing large amounts of unstructured data, such as documents, images, videos, and logs.

**File Storage:** Azure Storage provides a fully managed file share in the cloud, accessible via the Server Message Block (SMB) protocol. This is ideal for organizations that need a scalable file share that can be accessed from multiple virtual machines.

**Table Storage:** A NoSQL data store for semi-structured data, Azure Table Storage is suitable for applications that require high-speed access to large amounts of data.

**Queue Storage:** Azure Queue Storage is a message queuing service that enables communication between application components. It provides a scalable message queue for asynchronous processing.

**Disk Storage:** Azure Storage Accounts can be used to store Virtual Machine (VM) disks, providing persistent and scalable block storage for VMs in Azure.

**Security and Access Control:** Azure Storage offers robust security features, including encryption at rest, encryption in transit, and role-based access control (RBAC) to manage access permissions.

**Integration with Azure Services:** Azure Storage seamlessly integrates with various Azure services, making it a central component for applications deployed in the Azure cloud.

In the context of Microservices architecture and Azure Kubernetes Service (AKS), Azure Storage Account plays a pivotal role in providing scalable and reliable storage solutions for the diverse data requirements of microservices-based applications. 

## Technical Scenario

As a `Cloud Architect`, I need to design and implement a robust solution to address persistent storage challenges in my Microservices Architecture deployed on Azure Kubernetes Service (AKS). Using Azure Storage Account, specifically Azure File Storage, becomes handy to provide Persistent Volumes (PV),Persistent Volumes claims(PVC) as per the need in my Microservices. Additionally, I plan to utilize Storage Account containers for capturing Azure Event Hub (Kafka) messages, ensuring a seamless and well-integrated storage strategy within the broader microservices ecosystem.

## Objective

In this exercise we will accomplish & learn how to implement following:

- **Task-1:** Define and declare Azure Storage Account variables
- **Task-2:** Create Azure Storage Account using Terraform
- **Task-3:** Create Azure Storage Account container using Terraform
- **Task-4:** Configure diagnostic settings for Azure Storage Account using Terraform

## Architecture diagram

The following diagram illustrates the high level architecture of Storage Account usage:

<!-- ![Alt text](images/image-61.png) -->

## Prerequisites

Before proceeding with this lab, make sure you have the following prerequisites in place:

1. Download and Install Terraform.
2. Download and Install Azure CLI.
3. Azure subscription.
4. Visual Studio Code.
5. Log Analytics workspace - for configuring diagnostic settings.
7. Basic knowledge of Terraform and Azure concepts.

## Implementation details

Here's a step-by-step guide on how to create an Azure Storage Account using Terraform

**login to Azure**

Verify that you are logged into the right Azure subscription before start anything in visual studio code

```bash
# Login to Azure
az login 

# Shows current Azure subscription
az account show

# Lists all available Azure subscriptions
az account list

# Sets Azure subscription to desired subscription using ID
az account set -s "anji.keesari"
```

## Task-1: Define and declare Azure Storage Account variables

In this task, we will define and declare the necessary variables for creating the Azure Storage Account resource. 

*Variable declaration:*

``` bash title="variables.tf"
// ========================== Storage Account ==========================
# main.tf

provider "azurerm" {
  features = {}
}

# Variables
variable "resource_group_name" {
  description = "The name of the resource group in which to create the Storage Account."
}

variable "storage_account_name" {
  description = "The name of the Storage Account."
}

variable "location" {
  description = "The Azure region where the Storage Account will be created."
}

```

*Variable Definition:*

``` bash title="dev-variables.tfvars"
# Storage Account
st_name                             = "storage1" 
st_sku_name                         = "standard"

```

## Task-2: Create Azure Storage Account using Terraform

In this task, we will use Terraform to create the Azure Storage Account instance with the desired configuration.

``` bash title="storage-account.tf"
# Create Azure Storage Account using terraform
resource "azurerm_storage_account" "example" {
  name                     = var.storage_account_name
  resource_group_name      = var.resource_group_name
  location                 = var.location
  account_tier             = "Standard"
  account_replication_type = "LRS"

  tags = {
    environment = "dev"
  }
}
```
Run Terraform validation and formatting:

``` bash
terraform validate
terraform fmt
```

run terraform plan & apply

``` bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

Azure Storage Account - Overview blade 

<!-- ![Alt text](images/image-58.png) -->


## Task-3: Create Azure Storage Account Container using Terraform

In this task, we will use Terraform to create the Azure Storage Account Container 

``` bash title="storage-account.tf"
# Create Azure Storage Account container using terraform
resource "azurerm_storage_container" "example" {
  name                  = "examplecontainer"
  storage_account_name  = azurerm_storage_account.example.name
  container_access_type = "private"
}
```
Run Terraform validation and formatting:

``` bash
terraform validate
terraform fmt
```

run terraform plan & apply

``` bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

Azure Storage Account container

<!-- ![Alt text](images/image-58.png) -->


## Task-4: Configure diagnostic settings for Azure Storage Account using terraform

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure Storage Account instance.

``` bash title="storage-account.tf"
# create diagnostic setting for Storage Account
resource "azurerm_storage_account_diagnostic_setting" "example" {
  name               = "example"
  storage_account_id = azurerm_storage_account.example.id

  log {
    category = "StorageRead"
    enabled  = true

    retention_policy {
      enabled = true
      days    = 7
    }
  }
}
```

run terraform validate & format

``` bash
terraform validate
terraform fmt
```

run terraform plan & apply

``` bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

Azure Storage Account - Diagnostic settings from left nav


## Task-4: Create private endpoints for Azure Storage

By configuring Create private endpoints for Azure Storage, we can secure storage account from public access.

``` bash title="storage-account.tf"
# create diagnostic setting for Storage Account
resource "azurerm_storage_account_diagnostic_setting" "example" {
  name               = "example"
  storage_account_id = azurerm_storage_account.example.id

  log {
    category = "StorageRead"
    enabled  = true

    retention_policy {
      enabled = true
      days    = 7
    }
  }
}
```

run terraform validate & format

``` bash
terraform validate
terraform fmt
```

run terraform plan & apply

``` bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```


<!-- ![Alt text](images/image-59.png) -->

## Reference
- [Microsoft MSDN - Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/){:target="_blank"}
- [Microsoft MSDN - Create a storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal){:target="_blank"}
- [Microsoft MSDN - Storage account overview](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview){:target="_blank"}
- [Microsoft MSDN - Create a container](https://learn.microsoft.com/en-us/azure/storage/blobs/blob-containers-portal){:target="_blank"}
- [Microsoft MSDN - Use private endpoints for Azure Storage](https://learn.microsoft.com/en-us/azure/storage/common/storage-private-endpoints){:target="_blank"}
- [Terraform Registry - azurerm_storage_account](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account){:target="_blank"}
- [Terraform Registry - azurerm_storage_container](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_container){:target="_blank"}
- [Terraform Registry - azurerm_monitor_diagnostic_setting](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_diagnostic_setting){:target="_blank"}