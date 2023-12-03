## Introduction

Azure Front Door is a modern cloud content delivery network (CDN) service that delivers high performance, scalability, and secure user experiences for your content and applications. Azure Front Door offers a range of security, acceleration, global load balancing, and caching solutions operating at the network edge. 

In this lab, I will walk through the steps to create an Azure Front Door & CDN profile using Terraform. We'll also configure frontend URL of Azure Front Door and backend origin group and origin with route and custome domain.  I'll also configure diagnostic settings to monitor its performance effectively. Finally, we'll validate these resources within the Azure portal to confirm that everything is functioning as expected.

## Technical Scenario

As a `Cloud Architect`, your mission is to architect a solution for optimizing the caching of static and dynamic web content globally, elevating user experiences, and ensuring the creation of a high-performance, globally accessible, and secure website. Leveraging the capabilities of Azure Front Door and CDN profile is key to achieving these goals efficiently.

## Objective

In this exercise we will accomplish & learn how to implement following:

- **Task-1:** Define and Declare Azure Front Door & CDN variables.
- **Task-2:** Create a Front Door CDN profile using Terraform.
- **Task-3:** Create a Front Door Endpoint using using Terraform - (frontend).
- **Task-4:** Create a Front Door Origin Group using Terraform - (backend).
- **Task-5:** Create a Front Door Origin using Terraform - (backend).
- **Task-6:** Create Custom Domains for the Front Door using Terraform.
- **Task-7:** Create a Front Door Route using Terraform.
- **Task-8:** Create a DNS TXT (temporary) record in DNS Zone
- **Task-9:** Create DNS CNAME records in DNS Zone
- **Task-10:** Configure diagnostic settings for CDN profile
- **Task-10:** Apply lock on Front Door Profile
<!-- - **Task-7:** Create an App Service plan using Terraform.
- **Task-8:** Create an App Service app using Terraform. -->

## Architecture diagram

The following diagram illustrates the high level architecture of Azure Front Door & CDN:

<!-- ![Alt text](images/image-51.png) -->

## Prerequisites

Before proceeding with this lab, make sure you have the following prerequisites in place:

- Download and Install Terraform.
- Download and Install Azure CLI.
- Azure subscription.
- Visual Studio Code.
- Log Analytics workspace - for configuring diagnostic settings.
- Basic knowledge of Terraform and Azure concepts.

## Implementation details

Step-by-step implementation details will be covered here.


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

## Task-1: Define and Declare Azure Front Door & CDN variables.

In this task, we will define and declare the necessary variables for creating the Azure Front Door & CDN resources. These variables will be used to specify the Azure Front Door & CDN resources settings and customize the values according to our requirements in each environment.

This table presents the variables along with their descriptions, data types, and default values:


| Variable Name                                      | Description                                                                                                      | Type         | Default Value | 
| --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------ | ------------- |
| `cdn_frontdoor_prefix`                             | Prefix of the Front Door & CDN name.                                                                              | `string`     | `"afd"`       |
| `cdn_frontdoor_tags`                               | Specifies a mapping of tags for the Front Door Endpoint.                                                          | `map(any)`   | `{}`          |
| `cdn_frontdoor_profile_name`                        | Specifies the name of the Front Door Profile.                                                                     | `string`     | `"afd-cdn_frontdoor1-dev"` |
| `cdn_frontdoor_profile_sku_name`                    | The SKU for the Front Door profile. Possible values include: `Standard_AzureFrontDoor`, `Premium_AzureFrontDoor`.  | `string`     | `"Standard_AzureFrontDoor"` |
| `cdn_frontdoor_endpoint_name`                       | The name for the Front Door Origin. Changing this forces a new Front Door Origin to be created.                   | `string`     | `"endpoint1"` |
| `cdn_frontdoor_endpoint_enabled`                    | Specifies if this Front Door Endpoint is enabled. Defaults to `true`.                                             | `bool`       | `true`        |
| `cdn_frontdoor_origin_group_name`                  | The name for the Front Door Origin Group.                                                                         | `string`     | `"origingroup1"` |
| `session_affinity_enabled`                          | Specifies whether session affinity should be enabled on this host. Defaults to `true`.                            | `bool`       | `true`        |
| `restore_traffic_time_to_healed_or_new_endpoint_in_minutes` | Time before shifting traffic to another endpoint when a healthy endpoint becomes unhealthy or a new endpoint is added. | `number` | `10` |
| `cdn_frontdoor_origin_name`                        | The name for the Front Door Origin.                                                                               | `string`     | `"origin1"`   |
| `cdn_frontdoor_origin_host_ip`                     | The IPv4 address, IPv6 address, or Domain name of the Origin.                                                     | `string`     | `"20.125.213.106"` |
| `cdn_frontdoor_origin_host_name`                   | The host name of the domain. The `host_name` field must be the FQDN of your domain.                              | `string`     | `"sitename.mydomain.com"` |
| `cdn_frontdoor_origin_host_header`                 | The host header value which is sent to the origin with each request.                                               | `string`     | `"sitename.mydomain.com"` |
| `cdn_frontdoor_route_name`                         | The name for the Front Door Route. Valid values must begin with a letter or number and may only contain letters, numbers, and hyphens with a maximum length of 90 characters. | `string` | `"route1"` |
| `content_types_to_compress`                        | Specifies the address space of the hub virtual virtual network.                                                     | `list(string)` | See default values in the code |
| `public_dns_zone_name`                             | The name of the DNS Zone. Must be a valid domain name.                                                             | `string`     | `"mydomain.com"` |
| `public_dns_zone_rg_name`                          | Specifies the resource group where the resource exists.                                                           | `string`     | `"rg-mydomains-dev"` |
| `cdn_frontdoor_custom_domain_name`                 | The name for the Front Door Custom Domain. Must be between 2 and 260 characters in length.                         | `string`     | `"sitename"` |
| `dns_txt_record`                                   | The name of the DNS TXT Record.                                                                                    | `string`     | `"sitename"` |
| `dns_cname_record`                                 | The name of the DNS CNAME Record.                                                                                  | `string`     | `"sitename"` |

*Variable declaration:*

```tf title="variables.tf"
// ========================== Azure Front Door & CDN profile ==========================
# Task-1: Define and Declare Azure Front Door & CDN variables.
# cdn_profile
variable "cdn_frontdoor_prefix" {
  type        = string
  default     = "afd"
  description = "Prefix of the Front Door & CDN name."
}
variable "cdn_frontdoor_tags" {
  description = "(Optional) Specifies a mapping of tags which should be assigned to the Front Door Endpoint."
  type        = map(any)
  default     = {}
}
variable "cdn_frontdoor_profile_name" {
  type        = string
  default     = "afd-cdn_frontdoor1-dev"
  description = "(Required) Specifies the name of the Front Door Profile."
}
variable "cdn_frontdoor_profile_sku_name" {
  type        = string
  description = "The SKU for the Front Door profile. Possible values include: Standard_AzureFrontDoor, Premium_AzureFrontDoor"
  default     = "Standard_AzureFrontDoor"
  validation {
    condition     = contains(["Standard_AzureFrontDoor", "Premium_AzureFrontDoor"], var.cdn_frontdoor_profile_sku_name)
    error_message = "The SKU value must be one of the following: Standard_AzureFrontDoor, Premium_AzureFrontDoor."
  }
}
# endpoint
variable "cdn_frontdoor_endpoint_name" {
  type        = string
  default     = "endpoint1"
  description = "(Required) The name which should be used for this Front Door Origin. Changing this forces a new Front Door Origin to be created."
}
variable "cdn_frontdoor_endpoint_enabled" {
  description = " (Optional) Specifies if this Front Door Endpoint is enabled? Defaults to true."
  default     = true
  type        = bool
}
# origin_group
variable "cdn_frontdoor_origin_group_name" {
  type        = string
  default     = "origingroup1"
  description = "(Required) The name which should be used for this Front Door Origin Group."
}
variable "session_affinity_enabled" {
  description = "(Optional) Specifies whether session affinity should be enabled on this host. Defaults to true."
  default     = true
  type        = bool
}
variable "restore_traffic_time_to_healed_or_new_endpoint_in_minutes" {
  description = "(Optional) Specifies the amount of time which should elapse before shifting traffic to another endpoint when a healthy endpoint becomes unhealthy or a new endpoint is added. Possible values are between 0 and 50 minutes (inclusive). Default is 10 minutes."
  default     = 10
  type        = number
}
# origin
variable "cdn_frontdoor_origin_name" {
  type        = string
  default     = "origin1"
  description = "(Required) The name which should be used for this Front Door Origin."
}
variable "cdn_frontdoor_origin_host_ip" {
  type        = string
  default     = "20.125.213.106"
  description = "(Required) The IPv4 address, IPv6 address or Domain name of the Origin."
}
variable "cdn_frontdoor_origin_host_name" {
  type        = string
  default     = "sitename.mydomain.com"
  description = "(Required) The host name of the domain. The host_name field must be the FQDN of your domain(e.g. contoso.fabrikam.com)."
}
variable "cdn_frontdoor_origin_host_header" {
  type        = string
  default     = "sitename.mydomain.com"
  description = "(Optional) The host header value (an IPv4 address, IPv6 address or Domain name) which is sent to the origin with each request."
}
# route
variable "cdn_frontdoor_route_name" {
  type        = string
  default     = "route1"
  description = "(Required) The name which should be used for this Front Door Route. Valid values must begin with a letter or number, end with a letter or number and may only contain letters, numbers and hyphens with a maximum length of 90 characters."
}
variable "content_types_to_compress" {
  description = "Specifies the address space of the hub virtual virtual network"
  type        = list(string)
  default = [
    "application/eot",
    "application/font",
    "application/font-sfnt",
    "application/javascript",
    "application/json",
    "application/opentype",
    "application/otf",
    "application/pkcs7-mime",
    "application/truetype",
    "application/ttf",
    "application/vnd.ms-fontobject",
    "application/x-font-opentype",
    "application/x-font-truetype",
    "application/x-font-ttf",
    "application/x-httpd-cgi",
    "application/x-javascript",
    "application/x-mpegurl",
    "application/x-opentype",
    "application/x-otf",
    "application/x-perl",
    "application/x-ttf",
    "application/xhtml+xml",
    "application/xml",
    "application/xml+rss",
    "font/eot",
    "font/opentype",
    "font/otf",
    "font/ttf",
    "image/svg+xml",
    "text/css",
    "text/csv",
    "text/html",
    "text/javascript",
    "text/js",
    "text/plain",
    "text/richtext",
    "text/tab-separated-values",
    "text/x-component",
    "text/x-java-source",
    "text/x-script",
    "text/xml",
  ]
}
# custom_domain
variable "public_dns_zone_name" {
  type        = string
  default     = "mydomain.com"
  description = "(Required) The name of the DNS Zone. Must be a valid domain name. "
}
variable "public_dns_zone_rg_name" {
  type        = string
  default     = "rg-mydomains-dev"
  description = "(Required) Specifies the resource group where the resource exists."
}
variable "cdn_frontdoor_custom_domain_name" {
  type        = string
  default     = "sitename"
  description = "(Required) The name which should be used for this Front Door Custom Domain. Possible values must be between 2 and 260 characters in length, must begin with a letter or number, end with a letter or number and contain only letters, numbers and hyphens."
}
variable "dns_txt_record" {
  type        = string
  default     = "sitename"
  description = "(Required) The name of the DNS TXT Record."
}
variable "dns_cname_record" {
  type        = string
  default     = "sitename"
  description = " (Required) The name of the DNS CNAME Record."
}
```

*Variable Definition:*

```tf title="dev-variables.tfvars"
# Azure Front Door & CDN profile
cdn_frontdoor_profile_name          = "frontdoor1"
cdn_frontdoor_profile_sku_name      = "Standard_AzureFrontDoor"
```

## Task-2: Create a Front Door CDN profile using Terraform.

In this task, you will use Terraform to create an Azure CDN profile. The provided Terraform code demonstrates how to configure the CDN profile, including its name, resource group, SKU, and tags. This profile is the foundation of your content delivery network.

CDN profiles are commonly used to optimize the delivery of static and dynamic content, accelerate web performance, and reduce the load on your web servers by distributing cached content from the edge servers to users.


```tf title="cdn-frontdoor.tf"
# Task-2: Create a Front Door CDN profile using Terraform.
resource "azurerm_cdn_frontdoor_profile" "cdn_frontdoor_profile" {
  name                = lower("${var.cdn_frontdoor_prefix}-${var.cdn_frontdoor_profile_name}-${local.environment}")
  resource_group_name = azurerm_resource_group.rg.name
  sku_name            = var.cdn_frontdoor_profile_sku_name
  tags                = merge(local.default_tags)
  depends_on = [
    azurerm_resource_group.rg,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
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
Azure Front Door and CDN profile - Overview blade 

![Alt text](images/image-80.png)

## Task-3: Create a Front Door Endpoint using using Terraform.

**Azure Front Door manager**

Before creating CDN endpoints let's try to understand the `Azure Front Door manager` first here.

The Front Door manager in Azure Front Door Standard and Premium provides an overview of endpoints you've configured for your Azure Front Door profile. With Front Door manager, you can manage your collection of endpoints. You can also configure routings rules along with their domains and origin groups, and security policies you want to apply to protect your web application.

<https://learn.microsoft.com/en-us/azure/frontdoor/media/manager/manager-expanded.png#lightbox>

for more information - <https://learn.microsoft.com/en-us/azure/frontdoor/manager>

**Endpoint**

In Azure Front Door, an endpoint is a logical grouping of one or more routes that are associated with domain names. Each endpoint is assigned a domain name by Front Door, and you can associate your own custom domains by using routes.

A Front Door profile can contain multiple endpoints. However, in many situations you might only need a single endpoint.

For example, suppose you have created an endpoint named myendpoint. The endpoint domain name might be myendpoint-mdjf2jfgjf82mnzx.z01.azurefd.net.

for more information - <https://learn.microsoft.com/en-us/azure/frontdoor/endpoint?tabs=azurecli> 


```tf title="cdn-frontdoor.tf"
# Task-3: Create a Front Door endpoint using using Terraform. - (frontend) 
resource "azurerm_cdn_frontdoor_endpoint" "cdn_frontdoor_endpoint" {
  name                     = var.cdn_frontdoor_endpoint_name
  cdn_frontdoor_profile_id = azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile.id
  enabled                  = var.cdn_frontdoor_endpoint_enabled
  tags                     = merge(local.default_tags, var.cdn_frontdoor_tags)
  depends_on = [
    azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
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

![Alt text](images/image-81.png)


## Task-4: Create a Front Door Origin Group using Terraform.


An Origin Group, also known as a Backend Pool, is a logical grouping of multiple backend resources (usually web servers or application instances) that serve the same content or application but may be distributed across different geographic locations or data centers. The primary purpose of an Origin Group is to ensure high availability and load balancing.


```tf title="cdn-frontdoor.tf"

# Task-4: Create a Front Door origin group using Terraform. (backend)
resource "azurerm_cdn_frontdoor_origin_group" "cdn_frontdoor_origin_group" {
  name                                                      = var.cdn_frontdoor_origin_group_name
  cdn_frontdoor_profile_id                                  = azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile.id
  session_affinity_enabled                                  = var.session_affinity_enabled
  restore_traffic_time_to_healed_or_new_endpoint_in_minutes = var.restore_traffic_time_to_healed_or_new_endpoint_in_minutes

  load_balancing {
    additional_latency_in_milliseconds = 50
    sample_size                        = 4
    successful_samples_required        = 3
  }

  # health_probe {
  #   path                = "/"
  #   request_type        = "HEAD"
  #   protocol            = "Http"
  #   interval_in_seconds = 100
  # }
  depends_on = [
    azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
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

![Alt text](images/image-82.png)


## Task-5: Create a Front Door Origin using Terraform.

Origin
An origin refers to the application deployment that Azure Front Door retrieves contents from when caching isn't enabled or when a cache gets missed. Azure Front Door supports origins hosted in Azure and applications hosted in your on-premises datacenter or with another cloud provider. An origin shouldn't be confused with your database tier or storage tier. The origin should be viewed as the endpoint for your application backend. When you add an origin to an origin group in the Front Door configuration, you must also configure the following settings:
Origin type: 

```tf title="cdn-frontdoor.tf"
# Task-5: Create a Front Door origin using Terraform. (backend)
resource "azurerm_cdn_frontdoor_origin" "cdn_frontdoor_origin" {
  name                          = var.cdn_frontdoor_origin_name
  cdn_frontdoor_origin_group_id = azurerm_cdn_frontdoor_origin_group.cdn_frontdoor_origin_group.id

  enabled                        = true
  host_name                      = "20.125.213.106" # azurerm_public_ip.appgtw_pip.ip_address
  http_port                      = 80
  https_port                     = 443
  origin_host_header             = "sitename.mydomain.com" # azurerm_public_ip.appgtw_pip.ip_address
  priority                       = 1
  weight                         = 1000
  certificate_name_check_enabled = false

  depends_on = [
    azurerm_cdn_frontdoor_origin_group.cdn_frontdoor_origin_group,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
  }
}
```
In Azure Front Door, an "Origin" refers to the source of content that you want to distribute or deliver using the Front Door service. The term "Origin" typically represents a backend service or web application that hosts the content you want to serve to users. Origins are part of Origin Groups in Azure Front Door.

Here's a bit more detail:

- **Origin Group:** An Origin Group is a logical grouping of multiple backend Origins. It allows you to specify a collection of backend endpoints, often geographically distributed, that host the same content or service. These endpoints are used to serve traffic for your application. An Origin Group can be used for redundancy, load balancing, or traffic distribution based on your configuration.

2. **Origin:** An Origin, within an Origin Group, is a specific backend endpoint or server that hosts the content you want to serve. You might have multiple Origins within an Origin Group to provide high availability and fault tolerance. Azure Front Door automatically balances and routes traffic to these Origins.

For example, if you have a web application hosted in different data centers or regions, you can define each data center or region as an Origin within an Origin Group. Azure Front Door will then intelligently route traffic to the closest or healthiest Origin based on the routing rules you've configured.

By using Origin Groups and Origins, Azure Front Door can ensure high availability, low latency, and redundancy for your applications and content delivery.


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

![Alt text](images/image-83.png)


## Task-6: Create custom domains for the Front Door using Terraform.

This Terraform configuration is used to define and create a custom domain for an Azure Front Door profile, associating it with the provided Azure CDN Front Door profile and specifying details related to TLS for secure communication. The custom domain allows you to map your own domain name to the Front Door, enabling users to access your services via a user-friendly domain, such as www.example.com, while benefiting from the performance and security features of Azure Front Door.


```tf title="cdn-frontdoor.tf"

data "azurerm_dns_zone" "dns_zone" {
  name                = var.public_dns_zone_name
  resource_group_name = var.public_dns_zone_rg_name
}

# Task-6: Create custom domains for the Front Door
resource "azurerm_cdn_frontdoor_custom_domain" "cdn_frontdoor_custom_domain" {
  name                     = var.cdn_frontdoor_custom_domain_name
  cdn_frontdoor_profile_id = azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile.id
  dns_zone_id              = data.azurerm_dns_zone.dns_zone.id
  host_name                = var.cdn_frontdoor_origin_host_name
  tls {
    certificate_type    = "ManagedCertificate"
    minimum_tls_version = "TLS12"
  }
  depends_on = [
    azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile,
    data.azurerm_dns_zone.dns_zone,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
  }
}
```

<https://learn.microsoft.com/en-us/azure/frontdoor/domain>

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

<!-- ![Alt text](images/image-83.png) -->

## Task-7: Create a Front Door Route using Terraform

```tf title="cdn-frontdoor.tf"
# Task-7: Create Route in Origin of the Origin Group
resource "azurerm_cdn_frontdoor_route" "cdn_frontdoor_route" {
  name                            = var.cdn_frontdoor_route_name
  cdn_frontdoor_endpoint_id       = azurerm_cdn_frontdoor_endpoint.cdn_frontdoor_endpoint.id
  cdn_frontdoor_origin_group_id   = azurerm_cdn_frontdoor_origin_group.cdn_frontdoor_origin_group.id
  cdn_frontdoor_origin_ids        = [azurerm_cdn_frontdoor_origin.cdn_frontdoor_origin.id]
  cdn_frontdoor_custom_domain_ids = [azurerm_cdn_frontdoor_custom_domain.cdn_frontdoor_custom_domain.id]

  supported_protocols = ["Http", "Https"]
  patterns_to_match   = ["/*", "/"]
  forwarding_protocol    = "MatchRequest"
  link_to_default_domain = true
  https_redirect_enabled = true
  cache {
    query_string_caching_behavior = "UseQueryString"
    # Content won't be compressed when the requested content is smaller than 1 KB or larger than 8 MB(inclusive).
    compression_enabled       = true
    content_types_to_compress = var.content_types_to_compress
  }
  depends_on = [
    azurerm_cdn_frontdoor_endpoint.cdn_frontdoor_endpoint,
    azurerm_cdn_frontdoor_origin_group.cdn_frontdoor_origin_group,
    azurerm_cdn_frontdoor_origin.cdn_frontdoor_origin,
    azurerm_cdn_frontdoor_custom_domain.cdn_frontdoor_custom_domain,
  ]
  lifecycle {
    ignore_changes = [
      # tags
    ]
  }
}

```


In the context of Azure Front Door and similar content delivery or load balancing services, a "Route" typically refers to the configuration or rule that defines how incoming requests are directed to specific backend Origins or Origin Groups.

Here's a breakdown:

- **Route Configuration:** A "Route" configuration specifies the criteria for routing requests based on factors like the request's path, host, or other HTTP attributes. It defines what should happen when a request matches certain conditions.

2. **Origin Group Routing:** In Azure Front Door, you can define routing rules that determine which Origin Group should serve traffic based on certain conditions. For example, you might configure routing rules to direct requests for images to one Origin Group and requests for dynamic content to another. This allows for intelligent load balancing and content delivery.

3. **Routing Decisions:** Routes are used to make routing decisions. When a request is received by the Front Door service, it examines the request attributes and matches them against the defined routing rules. Once a match is found, the request is directed to the associated Origin or Origin Group.

4. **Path-Based Routing:** Path-based routing is a common use of routes. For instance, you can route requests that match the path "/images/*" to one Origin Group for image content and requests matching "/api/*" to another Origin Group for API requests.

In summary, "Route" in this context is a configuration element that enables the intelligent and dynamic routing of incoming requests to the appropriate backend Origin or Origin Group based on specific criteria. This is crucial for scenarios where you need to load balance, distribute, or segregate traffic effectively, whether for content delivery or backend service access. The exact terminology and configuration may vary depending on the service or platform you're using, but the fundamental concept of routing based on rules remains consistent.



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

<!-- ![Alt text](images/image-83.png) -->
## DNS configuration

When you add a domain to your Azure Front Door profile, you configure two records in your DNS server:
A DNS TXT record, which is required to validate ownership of your domain name. For more information on the DNS TXT records, see Domain validation.
A DNS CNAME record, which controls the flow of internet traffic to Azure Front Door.

## Task-8: Create a DNS TXT (temporary) record in DNS Zone

The provided Terraform configuration is used to create a DNS TXT record in an Azure DNS zone. The primary purpose of this DNS TXT record is to validate ownership and control of a custom domain for Azure Front Door. Let's break down the key components of this configuration:


```tf title="cdn-frontdoor.tf"
# Task-8: Create a DNS TXT (temporary) record in DNS Zone
resource "azurerm_dns_txt_record" "dns_txt_record_validation" {
  name                = join(".", ["_dnsauth", var.dns_txt_record])
  zone_name           = data.azurerm_dns_zone.dns_zone.name
  resource_group_name = data.azurerm_dns_zone.dns_zone.resource_group_name
  ttl                 = 3600

  record {
    value = azurerm_cdn_frontdoor_custom_domain.cdn_frontdoor_custom_domain.validation_token
  }
  depends_on = [
    azurerm_cdn_frontdoor_custom_domain.cdn_frontdoor_custom_domain,
    data.azurerm_dns_zone.dns_zone
  ]
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

<!-- ![Alt text](images/image-83.png) -->
In summary, this Terraform configuration creates a DNS TXT record in an Azure DNS zone with specific content, and it associates it with Azure Front Door for domain ownership validation. It's an essential step when setting up a custom domain to point to Azure Front Door, as it proves that you have control over the domain you're configuring.

## Task-9: Create DNS CNAME records in DNS Zone


```tf title="cdn-frontdoor.tf"
# Task-9: Create DNS CNAME records in DNS Zone
resource "azurerm_dns_cname_record" "dns_cname_record" {
  name                = var.dns_cname_record
  zone_name           = data.azurerm_dns_zone.dns_zone.name
  resource_group_name = data.azurerm_dns_zone.dns_zone.resource_group_name
  ttl                 = 3600
  record              = azurerm_cdn_frontdoor_endpoint.cdn_frontdoor_endpoint.host_name

  depends_on = [
    data.azurerm_dns_zone.dns_zone,
    azurerm_cdn_frontdoor_endpoint.cdn_frontdoor_endpoint,
    # azurerm_cdn_frontdoor_security_policy.example
  ]
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

<!-- ![Alt text](images/image-83.png) -->
## Task-10: Configure diagnostic settings for CDN profile

By configuring diagnostic settings, we can monitor and analyze the performance and behavior of the Azure Front Door & CDN profile instance.

``` bash title="redis_cache.tf"
# Task-10: Create Diagnostic Settings for Azure Front Door
resource "azurerm_monitor_diagnostic_setting" "cdn_frontdoor_diag" {  
  name                       = "DiagnosticsSettings"
  target_resource_id         = azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile.id
  log_analytics_workspace_id = azurerm_log_analytics_workspace.workspace.id

  log {
    category_group = "allLogs"
  }

  log {
    category_group = "audit"
  }

  metric {
    category = "AllMetrics"
  }
  lifecycle {
    ignore_changes = [
      log_analytics_destination_type,
    ]
  }
  depends_on = [
    azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile,
    azurerm_log_analytics_workspace.workspace
  ]
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

## Task-11: Apply lock on Front Door Profile

The purpose of applying a lock to an Azure resource, in this case, the Azure Front Door profile, is to prevent accidental deletion. By setting a "CanNotDelete" lock, you are ensuring that the resource remains in place and operational, which is particularly useful for critical resources in production environments to avoid data loss or service disruption caused by accidental deletion. It provides an additional layer of protection for important resources.

```tf title="cdn-frontdoor.tf"
# Task-10: Apply lock on Front Door Profile
resource "azurerm_management_lock" "cdn_frontdoor_lock" {
  name       = "afd-profile"
  scope      = azurerm_cdn_frontdoor_profile.cdn_frontdoor_profile.id
  lock_level = "CanNotDelete"
  notes      = "This resource can not be deleted - lock set by Terraform"
  depends_on = [
    azurerm_cdn_frontdoor_endpoint.cdn_frontdoor_endpoint,
    azurerm_cdn_frontdoor_origin_group.cdn_frontdoor_origin_group,
    azurerm_cdn_frontdoor_origin.cdn_frontdoor_origin,
    azurerm_cdn_frontdoor_custom_domain.cdn_frontdoor_custom_domain,
    azurerm_cdn_frontdoor_route.cdn_frontdoor_route
  ]
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

<!-- ![Alt text](images/image-83.png) -->

## Reference

Here are some references related to Azure Front Door and CDN:

- [Azure Front Door and CDN documentation](https://learn.microsoft.com/en-us/azure/frontdoor/)
- [What is Azure Front Door?](https://learn.microsoft.com/en-us/azure/frontdoor/front-door-overview)
- [Create an Azure Front Door Standard/Premium profile using Terraform](https://learn.microsoft.com/en-us/azure/frontdoor/create-front-door-terraform)
- [Decision tree for load balancing in Azure](https://learn.microsoft.com/en-us/azure/architecture/guide/technology-choices/load-balancing-overview)
- [azurerm_cdn_frontdoor_profile](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_profile)
- [azurerm_cdn_frontdoor_endpoint](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_endpoint)
- [azurerm_cdn_frontdoor_origin_group](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_origin_group)
- [azurerm_cdn_frontdoor_origin](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_origin)
- [azurerm_cdn_frontdoor_route](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_route)
- [azurerm_cdn_frontdoor_custom_domain](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/cdn_frontdoor_custom_domain)
- [azurerm_dns_txt_record](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_txt_record)
- [azurerm_dns_cname_record](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/resources/dns_cname_record)

<!-- - https://intellipaat.com/blog/azure-front-door/ -->