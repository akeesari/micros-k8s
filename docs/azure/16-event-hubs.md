
## Introduction

Azure Event Hubs is a cloud-based, scalable data streaming platform provided by Microsoft Azure. It is designed to ingest and process large volumes of streaming data from various sources in real-time. Event Hubs is part of the Azure messaging services and is particularly well-suited for scenarios involving event-driven architectures and big data analytics.

In this hands-on lab, I'll guide you through the process of creating an `Azure Event Hub namespace`, `Azure event hubs` using terraform.

## Technical Scenario

As a `Cloud Architect` leading the digital transformation efforts for our large enterprise organization, I have been tasked with designing a robust solution for migrating data from our legacy platform to a modern SaaS multi-tenant platform following microservices architecture hosted on Azure Kubernetes Service (AKS). The primary focus of this migration is to enhance agility, scalability, and maintainability, with Azure Event Hubs identified as big data streaming platform migrating from legacy to new platform.


**Background**:

Our organization has relied on a legacy platform for an extended period sincde year, and our architecture board has recently finalized the design for a digital transformation initiative. To achieve this transformation, we have embraced a containerized microservices architecture deployed on Azure Kubernetes Service (AKS) along with Azure event hub. The adoption of this modern architecture aims to bring about increased agility, scalability, and maintainability.

**Legacy System**:

The legacy system currently holds a substantial volume of data that requires migration to the new microservices-based platform.

**Microservices Architecture**:

Our new platform is composed of multiple microservices, each dedicated to specific business functionalities. These microservices are meticulously designed to be loosely coupled and independently deployable, aligning with best practices in modern software architecture.

**Azure Event Hubs Integration**:

To enable real-time data streaming from the legacy system to the new microservices, we have strategically chosen Azure Event Hubs as our central event streaming platform.

**Publisher Applications** (Legacy System):

Publisher applications play a pivotal role in this migration by handling the responsibility of publishing data events to dedicated event hubs within Azure Event Hubs. Each publisher application corresponds to a specific data domain or entity type, ensuring a granular and organized approach to data migration.

**Event Serialization and Schema**:

For efficient data serialization, producer applications utilize frameworks such as Avro to serialize data into a common schema. This schema is registered in Azure Schema Registry, guaranteeing consistency in data representation and format across the migration process.

**Consumer Applications** (New Microservices):

On the new microservices platform, consumer applications are designed to seamlessly consume events related to their specific business domains. These microservices subscribe to the event hub, allowing them to receive real-time updates as data is streamed from the legacy system.


## Why Azure Event Hubs:

1. **Real-time Data Streaming:**
   Azure Event Hubs serves as the  real-time data streaming from the legacy system to our new microservices. This  ensuring that our organization stays responsive and adaptive to dynamic data changes.

2. **Event-driven Architecture:**
   Leveraging Azure Event Hubs enables us to embrace an event-driven architecture. This approach allows us to decouple components, ensuring that each microservice can independently consume data events relevant to its specific business domain.

3. **Scalability and Elasticity:**
   Azure Event Hubs provides the scalability and elasticity needed to handle large volumes of data events efficiently. This is crucial as our organization evolves, ensuring that our architecture can seamlessly grow to accommodate increased workloads.

4. **Publisher-Subscriber Model:**
   The publisher applications in our legacy system publish data events to dedicated event hubs within Azure Event Hubs. This follows a publisher-subscriber model, ensuring a structured and organized migration of data from the legacy system.

5. **Schema Consistency:**
   The utilization of serialization frameworks like Avro, coupled with Azure Schema Registry, guarantees consistency in data representation. This ensures that data maintains a uniform schema throughout its journey from the legacy system to the new microservices environment.

6. **Business Domain Relevance:**
   Consumer applications on the new microservices platform subscribe to specific event hubs, allowing them to receive real-time updates relevant to their designated business domains. This targeted approach enhances the efficiency and relevance of data consumption.


## Objective

In this exercise we will accomplish & learn how to implement following:

- **Task-1:** Define and declare azure event hubs variables
- **Task-2:** Create azure event hub namespace using terraform
- **Task-3:** Create azure event hub using terraform
- **Task-4:** Create azure storage account for event hubs using terraform
- **Task-5:** Configure diagnostic settings for azure event hub using terraform
<!-- - **Task-7:** Restrict Access using private endpoint
- **Task-7.1:** Configure the Private DNS Zone
- **Task-7.2:** Create a Virtual Network Link Association
- **Task-7.3:** Create Private Endpoints for azure Storage
- **Task-7.4:** Validate private link connection using nslookup or dig -->

## Architecture diagram

The following diagram illustrates the high level architecture of azure event hubs

<IMG  src="https://learn.microsoft.com/en-us/azure/event-hubs/media/event-hubs-about/event_hubs_architecture.png"  alt="Event Hubs"/>

<!-- [![Alt text](images/storage/image-1.png)](images/storage/image-1.png){:target="_blank"} -->

## Prerequisites

Before proceeding with this lab, make sure you have the following prerequisites in place:

1. Download and Install Terraform.
2. Download and Install Azure CLI.
3. Azure subscription.
4. Visual Studio Code.
5. Log Analytics workspace - for configuring diagnostic settings.
7. Basic knowledge of terraform and Azure concepts.

## Implementation details

Here's a step-by-step guide on how to create an azure event hub namespace and azure event hubs using Terraform

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

## Task-1: Define and declare azure event hubs variables

In this task, we will define and declare the necessary variables for creating the azure event hub namespace and azure event hub resources. 

*Variable declaration:*

``` bash title="variables.tf"

```

*Variable Definition:*

``` bash title="dev-variables.tfvars"
#
```


## Task-1: Create Storage Account resources using terraform

**Purpose:** This task involves creating an Azure Storage Account dedicated to capturing events from the Kafka system. 

``` bash title="event-hubs.tf"
provider "azurerm" {
  features = {}
}

resource "azurerm_resource_group" "example" {
  name     = "example-resources"
  location = "East US"
}

resource "azurerm_storage_account" "kafka_event_capture" {
  name                     = "kafkaeventcapturestorage"
  resource_group_name      = azurerm_resource_group.example.name
  location                 = azurerm_resource_group.example.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-1.1: Create Diagnostic Settings for Storage Account

**Purpose:** To enable monitoring and diagnostics, this task configures diagnostic settings for Storage Account. It captures log data related to storage reads, ensuring visibility into system activities.

``` bash title="event-hubs.tf"
resource "azurerm_monitor_diagnostic_setting" "storage_diagnostic_settings" {
  name               = "kafka-event-capture-diag"
  target_resource_id = azurerm_storage_account.kafka_event_capture.id

  log {
    category = "StorageRead"
    enabled  = true

    retention_policy {
      enabled = false
    }
  }
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-1.2: Storage Account Container for Kafka Event Capture

**Purpose:** This task creates a specific container within the Storage Account

``` bash title="event-hubs.tf"
resource "azurerm_storage_container" "kafka_event_capture_container" {
  name                  = "kafkaeventcapturecontainer"
  storage_account_name  = azurerm_storage_account.kafka_event_capture.name
  container_access_type = "private"
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-1.3: Create Diagnostic Settings at the Blob Level

**Purpose:**  This step involves configuring diagnostic settings at the blob level, offering granular insights into storage-related activities at the individual data blob level.

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

## Task-2: Create Kafka Azure Event Hubs Namespace

**Purpose:** Establishing an Azure Event Hubs Namespace dedicated to a business unit ensures a centralized, scalable, and reliable platform for handling real-time event streaming. It acts as the core component for event ingestion and distribution.

``` bash title="event-hubs.tf"
resource "azurerm_eventhub_namespace" "kafka_event_hub_namespace" {
  name                = "kafka-eventhub-namespace"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  sku                 = "Standard"
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-3: Create Diagnostic Settings for Kafka Event Hub Namespace

**Purpose:** This task configures diagnostic settings but at the Event Hub Namespace level. It captures logs specific to Kafka-related activities, providing visibility into the performance and health of the event hub.

``` bash title="event-hubs.tf"
resource "azurerm_monitor_diagnostic_setting" "kafka_namespace_diagnostic_settings" {
  name               = "kafka-namespace-diag"
  target_resource_id = azurerm_eventhub_namespace.kafka_event_hub_namespace.id

  log {
    category = "Kafka"
    enabled  = true

    retention_policy {
      enabled = false
    }
  }
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-4: Shared Access Policies for Event Hub Namespace Level

### Task-4.1: Create Shared Access Policy Rule for Listen

**Purpose:** This task creates a shared access policy rule with listening permissions. It enables entities to consume events from the Event Hub Namespace, supporting secure and controlled access to the streaming data.

``` bash title="event-hubs.tf"
resource "azurerm_eventhub_namespace_authorization_rule" "listen_rule" {
  namespace_name        = azurerm_eventhub_namespace.kafka_event_hub_namespace.name
  resource_group_name   = azurerm_resource_group.example.name
  name                  = "listen-rule"
  listen               = true
  send                 = false
  manage               = false
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

### Task-4.2: Create Shared Access Policy Rule for Send

**Purpose:** Establishing a shared access policy rule with sending permissions allows entities to publish events to the Event Hub Namespace. It ensures controlled data ingestion, preventing unauthorized entities from sending data.

``` bash title="event-hubs.tf"
resource "azurerm_eventhub_namespace_authorization_rule" "send_rule" {
  namespace_name        = azurerm_eventhub_namespace.kafka_event_hub_namespace.name
  resource_group_name   = azurerm_resource_group.example.name
  name                  = "send-rule"
  listen               = false
  send                 = true
  manage               = false
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

### Task-4.3: Create Shared Access Policy Rule for Manage

**Purpose:** Creating a shared access policy rule with management permissions provides entities the capability to manage and administer the Event Hub Namespace. It's crucial for maintaining the security and configuration of the event hub.

``` bash title="event-hubs.tf"
resource "azurerm_eventhub_namespace_authorization_rule" "manage_rule" {
  namespace_name        = azurerm_eventhub_namespace.kafka_event_hub_namespace.name
  resource_group

_name   = azurerm_resource_group.example.name
  name                  = "manage-rule"
  listen               = false
  send                 = false
  manage               = true
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

## Task-5: Restrict Access Using Private Endpoint

### Task-5.1: Configure the Private DNS Zone

**Purpose:** Creating a private DNS zone enhances security by allowing resolution of Azure Event Hubs privately. It's a prerequisite for establishing a private link between the virtual network and the Azure Event Hubs.

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

### Task-5.2: Create a Virtual Network Link Association

**Purpose:** This task associates the virtual network with the private DNS zone, enabling DNS resolution of Azure Azure Event Hubs services within the virtual network. It's a key step for establishing a private link.

``` bash title="event-hubs.tf"
resource "azurerm_private_dns_zone_virtual_network_link" "example" {
  name                  = "example-link"
  resource_group_name  = azurerm_resource_group.example.name
  private_dns_zone_name = "privatelink.blob.core.windows.net"
  virtual_network_id    = azurerm_virtual_network.example.id
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

### Task-5.3: Create Private Endpoints for Azure Event Hubs

**Purpose:** Creating private endpoints for Azure Azure Event Hubs ensures that data traffic between the virtual network and Azure Event Hubs remains within the Microsoft Azure network. It enhances security by avoiding exposure to the public internet.

``` bash title="event-hubs.tf"
resource "azurerm_private_endpoint" "storage_endpoint" {
  name                = "storage-endpoint"
  resource_group_name = azurerm_resource_group.example.name
  location            = azurerm_resource_group.example.location
  subnet_id           = azurerm_subnet.example.id

  private_service_connection {
    name                           = "blob"
    private_connection_resource_id = azurerm_storage_account.kafka_event_capture.id
    is_manual_connection           = false
  }
}
```

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```

**Run terraform plan & apply:**
```bash
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```

### Task-5.4: Validate Private Link Connection Using nslookup or dig

**Purpose:** Manually validating the private link connection ensures that the private endpoints are properly configured and functioning. Using nslookup or dig confirms the successful resolution of the private endpoint's DNS name within the virtual network.

**Run terraform validate & format:**
```bash
terraform validate
terraform fmt
```


This process ensures that the private link connection is successfully established and allows expected private IP address associated with our resource in the private virtual network.

## Reference
- [Microsoft MSDN - Azure Blob Storage documentation](https://learn.microsoft.com/en-us/azure/storage/blobs/){:target="_blank"}
- [Microsoft MSDN - Create a storage account](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create?tabs=azure-portal){:target="_blank"}
- [Microsoft MSDN - Storage account overview](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-overview){:target="_blank"}
- [Microsoft MSDN - Create a container](https://learn.microsoft.com/en-us/azure/storage/blobs/blob-containers-portal){:target="_blank"}
- [Microsoft MSDN - Use private endpoints for Azure Storage](https://learn.microsoft.com/en-us/azure/storage/common/storage-private-endpoints){:target="_blank"}
- [Terraform Registry - azurerm_storage_account](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account){:target="_blank"}
- [Terraform Registry - azurerm_storage_container](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_container){:target="_blank"}
- [Terraform Registry - azurerm_storage_share](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_share){:target="_blank"}
- [Terraform Registry - azurerm_monitor_diagnostic_setting](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_diagnostic_setting){:target="_blank"}
- [Terraform Registry - azurerm_private_dns_zone](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone){:target="_blank"}
- [Terraform Registry - azurerm_private_dns_zone_virtual_network_link](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone_virtual_network_link){:target="_blank"}
- [Terraform Registry - azurerm_private_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint){:target="_blank"}


Certainly! Here are the direct Terraform Registry links for the mentioned Azure resources:

1. **Azure Storage Account:**
   - [azurerm_storage_account](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_account)

2. **Azure Storage Container:**
   - [azurerm_storage_container](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/storage_container)

3. **Azure Monitor Diagnostic Setting:**
   - [azurerm_monitor_diagnostic_setting](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_diagnostic_setting)

4. **Azure Event Hub Namespace:**
   - [azurerm_eventhub_namespace](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/eventhub_namespace)

5. **Azure Event Hub Namespace Authorization Rule:**
   - [azurerm_eventhub_namespace_authorization_rule](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/eventhub_namespace_authorization_rule)

6. **Azure Private Endpoint:**
   - [azurerm_private_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_endpoint)

7. **Azure Private DNS Zone Virtual Network Link:**
   - [azurerm_private_dns_zone_virtual_network_link](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/private_dns_zone_virtual_network_link)

Feel free to explore the documentation for each resource for detailed information and examples.