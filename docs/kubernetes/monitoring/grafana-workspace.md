# **Create an Azure Managed Grafana Using Terraform**

## **Introduction**

Azure Managed Grafana is a fully managed service that provides powerful, and flexible tool for visualizing and analyzing data. It allows seamless integration with various Azure services, including Azure Monitor and Log Analytics, making it a great choice for monitoring applications running on Azure Kubernetes Service (AKS).

In this article, we will explore how to create and configure an Azure Managed Grafana workspace using Terraform.

## **Technical Scenario**

**As a** cloud infrastructure engineer,  
**I want to** set up Azure Managed Grafana for monitoring workloads running on Azure Kubernetes Service (AKS),  
**So that** we can visualize real-time metrics, logs, and health of our AKS clusters and applications.

**Acceptance Criteria:**

1.  A dedicated Azure Managed Grafana workspace should be provisioned.
2.  The workspace should be securely connected to an Azure Monitor and Log Analytics workspace to fetch telemetry data.
3.  Diagnostic settings for the Grafana workspace should be enabled to capture logs and metrics for troubleshooting.
4.  Private endpoint access should be configured to ensure secure connectivity.
5.  Role-based access control (RBAC) should be implemented for Grafana access, with specific permissions granted to users and groups.
6.  The setup should be automated using terraform to ensure consistency across environments and to simplify maintenance.

**Why Azure Managed Grafana?**

*   **Seamless Integration**: Azure Managed Grafana integrates effortlessly with Azure services such as Azure Monitor, Log Analytics, and Azure Metrics, offering a smooth experience for visualizing metrics from AKS and other cloud-native applications.
*   **Scalability**: The service automatically scales with your workload, so as your AKS clusters grow, your monitoring infrastructure can scale with it.
*   **Security**: The service provides built-in security features such as RBAC, private endpoints, and audit logging, making it a suitable choice for regulated environments.

## **Prerequisites**

Before proceeding, ensure the following prerequisites are met:

- An active Azure subscription with sufficient permissions (Contributor or Owner role).
- Terraform installed and configured locally.
- Familiarity with terraform syntax and Azure resources.
- Azure CLI installed for authentication purposes.
- Predefined Azure AD groups for managing role assignments.
- Basic knowledge of Azure Monitor and its resources.
- Azure Log Analytics Workspace: You need an existing Azure Log Analytics workspace for setting up diagnostics

## **Implementation Details**

The following steps detail the implementation process:

## **Task-1: Configure terraform variables for Azure Managed Grafana**

Start by defining the necessary variables in the `variables.tf` file to make your terraform configuration modular and reusable:

```hcl

# Configure terraform variables for Azure Managed Grafana
variable "grafana_name_prefix" {
  type        = string
  default     = "amg"
  description = "Prefix of the  Azure Managed Grafana name that's combined with name of the  Azure Managed Grafana."
}
variable "grafana_name" {
  description = "(Required) Specifies the name which should be used for this Dashboard Grafana"
  type        = string
  default     = "grafana"
}
variable "api_key_enabled" {
  description = " (Optional) Whether to enable the api key setting of the Grafana instance. Defaults to false."
  type        = bool
  default     = false
}
variable "grafana_major_version" {
  description = "(Optional) Which major version of Grafana to deploy. Defaults to 9. Possible values are 9, 10"
  type        = number
  default     = 10
}
variable "deterministic_outbound_ip_enabled" {
  description = "(Optional) Whether to enable the Grafana instance to use deterministic outbound IPs. Defaults to false."
  type        = bool
  default     = false
}
variable "grafana_public_access" {
  description = "(Optional) Whether to enable traffic over the public interface. Defaults to true."
  type        = bool
  default     = true
}
variable "monitoring_tags" {
  description = "(Optional) A mapping of tags which should be assigned to the Azure Monitor Workspace."
  type        = map(any)
  default     = {
    "Application" = "Monitoring"
  }
}
variable "diagnostic_setting_name" {
  description = "The name of this Diagnostic Setting."
  type        = string
  default     = "diag-grafana"
}
variable "diagnostic_setting_enabled_log_categories" {
  description = "A list of log categories to be enabled for this diagnostic setting."
  type        = list(string)
  default     = ["GrafanaLoginEvents"]
}
variable "grafana_rbac" {}
```

## **Task-2: Create an Azure Managed Grafana using terraform**

Provision an Azure Managed Grafana, which is the backbone of Azure's monitoring services:

```hcl
# Step-1: Create the Azure Managed Grafana
resource "azurerm_dashboard_grafana" "grafana" {
  name                              = "${var.grafana_name_prefix}-${var.grafana_name}-${local.environment}"
  resource_group_name               = azurerm_resource_group.monitoring.name
  location                          = azurerm_resource_group.monitoring.location
  sku                               = "Standard"
  public_network_access_enabled     = var.grafana_public_access
  api_key_enabled                   = var.api_key_enabled
  deterministic_outbound_ip_enabled = var.deterministic_outbound_ip_enabled
  grafana_major_version             = var.grafana_major_version
  tags                              = merge(local.default_tags, var.monitoring_tags)
  lifecycle {
    ignore_changes = [
      # tags,
    ]
  }
  identity {
    type = "SystemAssigned"
  }

  azure_monitor_workspace_integrations {
    resource_id = azurerm_monitor_workspace.amw.id
  }
  depends_on = [
    azurerm_resource_group.monitoring,
    azurerm_monitor_workspace.amw
  ]
}
```

This code will create a Grafana workspace linked to a Log Analytics workspace for monitoring.

## **Task-4: Configure diagnostic settings for Azure Managed Grafana**

Diagnostic settings allow you to monitor and troubleshoot the performance of your Azure Managed Grafana workspace.

```hcl
# Step-2: Configure diagnostic settings for Azure Managed Grafana
resource "azurerm_monitor_diagnostic_setting" "grafana" {
  name                       = var.diagnostic_setting_name
  target_resource_id         = azurerm_dashboard_grafana.grafana.id
  log_analytics_workspace_id = data.azurerm_log_analytics_workspace.workspace.id

  dynamic "enabled_log" {
    for_each = toset(var.diagnostic_setting_enabled_log_categories)

    content {
      category = enabled_log.value
    }
  }
  metric {
    category = "AllMetrics"
    enabled  = false
  }
  depends_on = [
    azurerm_dashboard_grafana.grafana,
    data.azurerm_log_analytics_workspace.workspace
  ]
}
```

## **Step 4: Create a private endpoint for Azure Managed Grafana**

To ensure secure access to the Azure Managed Grafana workspace, create a private endpoint.

```hcl
# Step-3 Create a private endpoint for Azure Managed Grafana
resource "azurerm_private_endpoint" "pe_grafana" {
  name                = lower("${var.private_endpoint_prefix}-${azurerm_dashboard_grafana.grafana.name}")
  location            = var.location # azurerm_dashboard_grafana.grafana.location
  resource_group_name = azurerm_dashboard_grafana.grafana.resource_group_name
  subnet_id           = data.azurerm_subnet.jumpserver.id
  tags                = merge(local.default_tags)

  private_service_connection {
    name                           = "pe-${azurerm_dashboard_grafana.grafana.name}"
    private_connection_resource_id = azurerm_dashboard_grafana.grafana.id
    is_manual_connection           = false
    subresource_names              = var.pe_grafana_subresource_names
    request_message                = try(var.request_message, null)
  }

  private_dns_zone_group {
    name                 = "default"
    private_dns_zone_ids = [data.azurerm_private_dns_zone.pdz_grafana_core_sub.id]
  }

  lifecycle {
    ignore_changes = [
      custom_network_interface_name,
      # location
    ]
  }

  depends_on = [
    data.azurerm_subnet.jumpserver,
    azurerm_dashboard_grafana.grafana,
    data.azurerm_private_dns_zone.pdz_grafana_core_sub
  ]
}   
```

## **Step 5: Create role assignment over resource group containing the azure log analytics workspace**
To enable access for the Grafana workspace to the Log Analytics workspace, create a role assignment.

```hcl
# Step-4: Add required role assignment over resource group containing the Azure Monitor Workspace
resource "azurerm_role_assignment" "grafana" {
  scope                = azurerm_resource_group.monitoring.id
  role_definition_name = "Monitoring Reader"
  principal_id         = azurerm_dashboard_grafana.grafana.identity[0].principal_id
  depends_on = [
    azurerm_resource_group.monitoring,
    azurerm_monitor_workspace.amw,
    azurerm_dashboard_grafana.grafana
  ]
}    
```

## **Step 6: Create role assignment over resource group containing the Azure Monitor Workspace**

Similarly, create role assignments for Azure Monitor.

```hcl
# Step-5: Add required role assignment over resource group containing the Azure Log Analytics Workspace for AKS logs access
resource "azurerm_role_assignment" "grafana_law" {
  scope                = data.azurerm_resource_group.workspace.id
  role_definition_name = "Monitoring Reader"
  principal_id         = azurerm_dashboard_grafana.grafana.identity[0].principal_id
  depends_on = [
    azurerm_resource_group.monitoring,
    data.azurerm_resource_group.workspace
  ]
}
```

## **Step 7: Create role assignment over resource group containing the AKS**

Grant the necessary permissions to AKS.

```hcl
# Step-6: Add required role assignment over ewm30 resource group access for AKS
resource "azurerm_role_assignment" "monitoring_reader_ewm30_rg" {
  scope                = data.azurerm_resource_group.ewm30_rg.id
  role_definition_name = "Monitoring Reader"
  principal_id         = azurerm_dashboard_grafana.grafana.identity[0].principal_id
  depends_on = [
    azurerm_resource_group.monitoring,
    data.azurerm_resource_group.workspace
  ]
}
```

## **Step 8: Create azure AD groups for Grafana Access**

Define Azure AD groups to assign users to Grafana.

```hcl
# Step-7: Create azure ad groups for Grafana access
resource "azuread_group" "grafana_admin" {
  for_each         = toset(["admin", "editor", "viewer"])
  display_name     = lower("${local.ad_group_prefix}-grafana-${each.key}-ewm30-${local.environment}")
  owners           = [data.azurerm_client_config.current.object_id]
  security_enabled = true

  lifecycle {
    ignore_changes = [owners]
  }
}    
```

## **Step 9: Setup RBAC for Grafana Access**

Configure RBAC to grant users specific access levels to Grafana dashboards.

```hcl
# Step-8: RBAC setup for Grafana instance access
resource "azurerm_role_assignment" "grafana_rbac" {
  for_each             = var.grafana_rbac
  principal_id         = each.value
  role_definition_name = each.key
  scope                = azurerm_dashboard_grafana.grafana.id
  depends_on = [
    azurerm_dashboard_grafana.grafana
  ]
}   
```

<!-- ## **Conclusion**

Implementing an Azure Monitoring Workspace using terraform not only simplifies resource creation but also ensures best practices for infrastructure management. By following the detailed steps outlined above, you can establish a scalable, secure, and efficient monitoring solution for your Azure resources, including AKS clusters. -->

## **Reference**

- [terraform Azure Provider Documentation](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dashboard_grafana)
- [Microsoft Azure Managed Grafana workspace](https://learn.microsoft.com/en-us/azure/managed-grafana/quickstart-managed-grafana-portal)

