<!-- # Chapter 2.1: Azure Naming Conventions -->

## Naming conventions

Azure naming conventions are guidelines and best practices for naming resources in Azure. These conventions help us to ensure that resources are easily identifiable, consistent, and organized within your Azure environment. Some of the key components of Azure naming conventions include:

- **Consistency**: All resources should be named in a consistent manner, using the same naming convention across all resource types. This makes it easier to identify and locate resources in the Azure portal and other Azure tools.

- **Meaningful names**: Resources should be named in a way that makes their purpose and context clear. This includes using prefixes or suffixes to indicate the resource type, environment, or other relevant information.

- **Length and character limitations**: Azure has a maximum length limit and character limitations on the name of a resource, it's important to be aware of these limitations when creating a name.

- **Uniqueness**: Resources must have unique names within the subscription and resource group they are located in.

- **Avoid using special characters and spaces**: Azure resource names cannot contain special characters, such as “!”, “@”, “#”, and “$”, and spaces.

Using a consistent naming convention for resources can help us to improve the organization and management of your Azure environment, making it easier to find, understand and troubleshoot resources.

Examples of naming conventions for Azure resources:
```
- Virtual Machine: <prefix>-vm-<environment>-<number>
- Virtual Network: <prefix>-vnet-<environment>-<number>
- Storage Account: <prefix>-storage-<environment>-<number>
- SQL Server: <prefix>-sql-<environment>-<number>
```

## Tagging conventions

Tagging is useful for tracking and managing resources, as well as for cost allocation and reporting. Azure tagging conventions are guidelines and best practices for tagging resources in Azure, similar to naming conventions. These conventions help to ensure that resources are easily identifiable, consistent, and organized within your Azure environment.

Examples of tagging conventions for Azure resources:

```
Environment: Key: "Environment" Value: "Production"
Cost Center: Key: "Cost Center" Value: "CC100"
Application: Key: "Application" Value: "Project1"
```

It is always recommended to follow the industry standards and best practices rather than reinventing the weel. read the following links and understand the pattern and create your own standards if any deviations from these naming conventions and standards.

naming and tagging conventions template is available here for download and modify as per your company needs.

<https://raw.githubusercontent.com/microsoft/CloudAdoptionFramework/master/ready/naming-and-tagging-conventions-tracking-template.xlsx>

best way to force these naming conventions for entire organization level is to keep them in the terraform configuration file so that it can used or re-used while creating all the services. you will see these in actions during azure resource creation using terraform configuration.


## Reference

- [Microsoft MSDN - Develop your naming and tagging strategy for Azure resources](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/naming-and-tagging){:target="_blank"}
- [Microsoft MSDN - Define your naming convention](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-naming){:target="_blank"}
- [Microsoft MSDN - Abbreviation examples for Azure resources](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/azure-best-practices/resource-abbreviations){:target="_blank"}