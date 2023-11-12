## Introduction

Azure Database for PostgreSQL - Flexible Server is a fully managed database service on the Microsoft Azure cloud platform, designed to host PostgreSQL databases. It provides a scalable, secure, and cost-effective solution for deploying, managing, and scaling PostgreSQL-based applications.

In this hands-on lab, I'll guide you through the process of creating an Azure PostgreSQL - Flexible Server using Terraform. We'll set up diagnostic settings to monitor this resource effectively.

Key features of Azure Database for PostgreSQL - Flexible Server:

1. **Managed Service:** It is a fully managed database service, meaning Microsoft Azure takes care of routine database management tasks, allowing developers to focus on building applications.

2. **Open Source Compatibility:** Based on the popular open-source PostgreSQL database engine, ensuring compatibility with PostgreSQL and support for a wide range of PostgreSQL features.

3. **Scalability:** Offers flexible compute and storage configurations to scale resources based on application requirements, providing the ability to scale up or down as needed.

4. **High Availability:** Provides built-in high availability with automatic backups and the ability to restore to any point in time within the retention period.

5. **Security Features:**
    - Supports Azure Active Directory authentication for enhanced security.
    - Enables data encryption in transit and at rest.
    - Firewall rules and Virtual Network Service Endpoints enhance network security.

6. **Automatic Patching:** Azure handles routine maintenance tasks, including software patching, ensuring that the database is up-to-date and secure.

7. **Monitoring and Diagnostics:** Integration with Azure Monitor provides real-time performance monitoring, diagnostics, and insights into the database's health and performance.

8. **Geo-replication:** Allows for setting up read replicas in different Azure regions for improved read scalability and disaster recovery.

9. **Flexible Deployment Options:** Supports deploying databases across different Azure regions and availability zones for better performance and fault tolerance.

10. **Developer Tools Integration:** Seamless integration with popular developer tools and frameworks, making it easy for developers to work with their preferred tools.

11. **Cost Management:** Provides cost-effective pricing models based on the chosen configuration, allowing users to optimize costs based on their application needs.

12. **Compatibility with Azure Services:** Integrates with other Azure services, such as Azure Logic Apps, Azure Functions, and more, for building end-to-end solutions.


## Technical Scenario
As a `Cloud Architect`, the task is to design and implement a database solution that aligns with the principles of microservices architecture. The solution should be robust, scalable, cost-effective, and includes key considerations such as scalability, high availability, security, geo-replication, and seamless integration with microservices.


## Objective

In this exercise we will accomplish & learn how to implement following:

- **Task-1:** Define and declare PostgreSQL - Flexible Server variables
- **Task-2:** Create an Azure resource group for PostgreSQL
- **Task-3:** Create an Azure virtual network 
- **Task-4:** Create an Azure subnet for PostgreSQL
- **Task-5:** Create a private DNS zone for PostgreSQL
- **Task-6:** Create a private DNS zone vnet link
- **Task-7:** Associate private DNS zone with virtual network
- **Task-8:** Create Azure PostgreSQL - Flexible Server using Terraform
- **Task-9:** Configure Diagnostic settings for PostgreSQL - Flexible Server
- **Task-10:** Set a user or group as the AD administrator for a PostgreSQL Flexible Server.
- **Task-11:** Create A new database in PostgreSQL Server
- **Task-11:** Create AD groups for database access

## Architecture diagram 

The following diagram illustrates the high level architecture of PostgreSQL - Flexible Server:

![Alt text](images/image-62.png)

## Prerequisites

Before proceeding with this lab, make sure you have the following prerequisites in place:

1. Download and Install Terraform.
2. Download and Install Azure CLI.
3. Azure subscription.
4. Visual Studio Code.
5. Log Analytics workspace - for configuring diagnostic settings.
7. Basic knowledge of Terraform and Azure concepts.

## Implementation details

Here's a step-by-step guide on how to create an Azure PostgreSQL - Flexible Server using Terraform


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

## Task-1: Define and declare PostgreSQL - Flexible Server variables

In this task, we will define and declare the necessary variables for creating the Azure PostgreSQL - Flexible Server resource. These variables will be used to specify the resource settings and customize the values according to our requirements in each environment.

This table presents the variables along with their descriptions, data types, and default values:



*Variable declaration:*

``` bash title="variables.tf"

```

*Variable Definition:*

``` bash title="dev-variables.tfvars"

```

## Task-2: Create an Azure resource group for PostgreSQL

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)


## Task-3: Create an Azure virtual network 

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)

## Task-4: Create an Azure subnet for PostgreSQL 

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)


## Task-5: Create a private DNS zone for PostgreSQL

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)


## Task-6: Create a private DNS zone vnet link

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)


## Task-7: Associate private DNS zone with virtual network

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - resource group

![Alt text](images/image-59.png)

## Task-8: Create Azure PostgreSQL - Flexible Server using Terraform

In this task, we will use Terraform to create the Azure PostgreSQL - Flexible Server instance with the desired configuration.

``` bash title="postgresql.tf"

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

Azure PostgreSQL - Flexible Server - Overview blade 

![Alt text](images/image-58.png)

## Task-9: Configure Diagnostic settings for Azure PostgreSQL - Flexible Server using terraform

By configuring diagnostic settings, we can monitor and analyze the behavior of the Azure PostgreSQL - Flexible Server instance.

``` bash title="postgresql.tf"

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


Azure PostgreSQL - Flexible Server - Diagnostic settings from left nav

![Alt text](images/image-59.png)

## Task-10: Set a user or group as the AD administrator for a PostgreSQL Flexible Server

Azure Access Policies in Key Vault are essential for developer who is managing  & who can perform operations on the secrets, keys, and certificates stored in the Key Vault. 

``` bash title="private_dns.tf"

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

Azure key vault - Azure Access Policies

![Alt text](images/image-60.png)


## Task-11: Create A new database in PostgreSQL Server

Azure Access Policies in Key Vault are essential for developer who is managing  & who can perform operations on the secrets, keys, and certificates stored in the Key Vault. 

``` bash title="private_dns.tf"

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

Azure key vault - Azure Access Policies

![Alt text](images/image-60.png)


## Task-12: Create AD groups for database access

Azure Access Policies in Key Vault are essential for developer who is managing  & who can perform operations on the secrets, keys, and certificates stored in the Key Vault. 

``` bash title="private_dns.tf"

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

Azure key vault - Azure Access Policies

![Alt text](images/image-60.png)

## Reference

here is the list of all the resources used while working on this technical story:

- https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/
- https://learn.microsoft.com/en-us/azure/postgresql/flexible-server/overview
- https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/postgresql_flexible_server
- https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/postgresql_flexible_server_database
- https://docs.microsoft.com/en-us/azure/developer/terraform/deploy-postgresql-flexible-server-database?tabs=azure-cli
- https://github.com/claranet/terraform-azurerm-db-postgresql-flexible/blob/master/variables.tf - very good
