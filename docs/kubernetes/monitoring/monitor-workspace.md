# **Create an Azure Monitoring Workspace Using Terraform**

## **Introduction**

Azure Monitor workspaces store metric data collected by Azure Monitor, focusing primarily on Prometheus metrics.

This article explores the detailed steps required to set up an Azure Monitoring Workspace using terraform.

## **Technical Scenario**

Consider an enterprise managing several AKS clusters with the need for a unified monitoring solution. One of the prerequisites for enabling monitoring for Kubernetes clusters is having an Azure Monitoring workspace ready. This allows us to set up Prometheus and Grafana and integrate them with AKS.

## **Prerequisites**

Before proceeding, ensure the following prerequisites are met:

- An active Azure subscription with sufficient permissions (Contributor or Owner role).
- Terraform installed and configured locally.
- Familiarity with terraform syntax and Azure resources.
- Azure CLI installed for authentication purposes.
- Predefined Azure AD groups for managing role assignments.
- Basic knowledge of Azure Monitor and its resources.

## **Implementation Details**

The following steps detail the implementation process:

## **Task-1: Configure terraform variables for azure monitoring workspace**

Start by defining the necessary variables in the `variables.tf` file to make your terraform configuration modular and reusable:

```hcl
variable "rg_prefix" {
  type        = string
  default     = "rg"
  description = "Prefix of the resource group name that's combined with name of the resource group."
}
variable "monitoring_rg_name" {
  description = "(Required) Specifies the name of the Resource Group where the Azure Monitor Workspace should exist. "
  type        = string
  default     = "monitoring"
}
variable "monitoring_rg_name" {
  description = "(Required) Specifies the name of the Resource Group where the Azure Monitor Workspace should exist. "
  type        = string
  default     = "monitoring"
}
variable "monitoring_rg_location" {
  description = "(Required) Specifies the Azure Region where the Azure Monitor Workspace should exist."
  type        = string
  default     = "eastus"
}
variable "monitoring_tags" {
  description = "(Optional) A mapping of tags which should be assigned to the Azure Monitor Workspace."
  type        = map(any)
  default     = {
    "Application" = "Monitoring"
  }
}
variable "azure_monitor_workspace_prefix" {
  type        = string
  default     = "amw"
  description = "Prefix of the Azure Monitor Workspace name that's combined with name of the Azure Monitor Workspace."
}
variable "azure_monitor_workspace_name" {
  description = "(Required) Specifies the name which should be used for this Azure Monitor Workspace."
  type        = string
  default     = "workspace"
}
```

## **Task-2: Create new resource group for azure monitoring workspace**

Create a new resource group to host the Azure Monitoring Workspace. Resource groups provide logical groupings for Azure resources, making management easier:

```hcl
# Create the resource group for monitoring
resource "azurerm_resource_group" "monitoring" {
  name     = lower("${var.rg_prefix}-${var.monitoring_rg_name}-${local.environment}")
  location = var.monitoring_rg_location
  tags     = merge(local.default_tags, var.monitoring_tags)
  lifecycle {
    ignore_changes = [
      # tags,
    ]
  }
}

```

## **Task-3: Create an azure monitoring workspace using terraform**

Provision an Azure Monitor workspace, which is the backbone of Azure's monitoring services:

```hcl
# Create the Azure Monitor Workspace.
resource "azurerm_monitor_workspace" "amw" {
  name                          = lower("${var.azure_monitor_workspace_prefix}-${var.azure_monitor_workspace_name}-${local.environment}")
  resource_group_name           = azurerm_resource_group.monitoring.name
  location                      = azurerm_resource_group.monitoring.location
  public_network_access_enabled = var.monitor_workspace_public_access
  tags                          = merge(local.default_tags, var.monitoring_tags)
  lifecycle {
    ignore_changes = [
      # tags,
    ]
  }
  depends_on = [
    azurerm_resource_group.monitoring
  ]
}
```

## **Task-4: Lock the resource group to prevent accidental deletions**

Add a resource lock to the resource group to prevent accidental deletions. This lock can be configured using Terraform:

```hcl
# Lock the resource group -rg-monitoring-<env>
resource "azurerm_management_lock" "monitoring" {
  name       = "CanNotDelete"
  scope      = azurerm_resource_group.monitoring.id
  lock_level = "CanNotDelete"
  notes      = "This resource group can not be deleted - lock set by Terraform"
  depends_on = [
    azurerm_resource_group.monitoring,
    azurerm_monitor_workspace.amw,    
  ]
}
```

## **Reference**

- [terraform Azure Provider Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/monitor_workspace)
- [Microsoft Azure Monitoring Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/essentials/azure-monitor-workspace-overview)
- [Azure Monitor Workspace](https://learn.microsoft.com/en-us/azure/azure-monitor/)

