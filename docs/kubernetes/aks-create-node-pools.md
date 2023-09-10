<!-- Here are step-by-step instructions on creating a new user node pool in AKS using Terraform, along with an introduction to taints and tolerations: -->

# Create a new user node pool in AKS using terraform

## Introduction

In Kubernetes, **taints** and **tolerations** are essential mechanisms for controlling pod placement on nodes within a cluster. Taints are applied to nodes, specifying constraints on which pods can be scheduled there, while tolerations are set on pods, allowing them to tolerate specific node taints. 

This guide will walk you through creating a new user node pool in Azure Kubernetes Service (AKS) using Terraform and implementing taints and tolerations.

Define Taints and Tolerations

- **Taints**: Taints are labels applied to nodes that indicate a restriction or preference for the types of pods that can be scheduled on the node. Taints consist of a key, value, and effect. The effect can be "NoSchedule," "PreferNoSchedule," or "NoExecute."

- **Tolerations**: Tolerations are set on pods and allow them to tolerate specific taints on nodes. A toleration specifies the key, operator, value, and effect of the taint it tolerates.

Use Cases for Taints and Tolerations

- **Node Isolation**: Taints can be used to isolate nodes for specific workloads, ensuring that only designated pods are scheduled on those nodes.

- **Node Affinity**: Tolerations can be used to specify that pods with particular requirements are scheduled on nodes with matching taints.

## Technical Scenario

In this scenario, we will create a new user node pool in an existing AKS cluster using Terraform. We'll apply a taint to this node pool. Then, we'll deploy an application with tolerations to ensure it runs on nodes with the corresponding taint.

## Objective

- Create a new user node pool in AKS.
- Apply a taint to the new node pool.
- Deploy an application with tolerations to run on nodes with the taint.
- Verify the application's status.

<!-- ## Architecture Diagram

![Architecture Diagram](insert_link_to_diagram_here) -->

## Prerequisites:

1. **Terraform Installed**: Ensure you have Terraform installed on your local machine.

2. **Azure CLI**: Install the Azure Command-Line Interface (CLI) for authentication.

3. **Terraform Configuration**: Make sure you have an existing Terraform configuration for your AKS cluster.

## Implementation Details

Here are step-by-step instructions on creating a new user node pool in AKS using Terraform.

login to Azure

Verify that you are logged into the right Azure subscription before start anything in visual studio code

``` sh
# Login to Azure
az login 

# Set Azure subscription
az account set -s "anji.keesari"
```

Connect to Cluster

Use the following command to connect to your AKS cluster.

``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get namespace -A
```

## Step 1: Declare & Define Variables

In your Terraform configuration file, declare variables for AKS cluster details, and node pool configuration.

**Declare Variables**
``` tf title="variables.tf"
// ========================== Azure Kubernetes services (AKS)- User Node Pool ==========================

variable "user_node_pool_enabled" {
  description = "(Optional) Should the User Node Pool enabled? Defaults to false."
  type        = bool
  default     = false
}


variable "user_node_pool_vm_size" {
  description = "Specifies the vm size of the user node pool"
  default     = "Standard_B4ms"
  type        = string
}

variable "user_node_pool_availability_zones" {
  description = "Specifies the availability zones of the user node pool"
  default     = ["1", "2", "3"]
  type        = list(string)
}

variable "user_node_pool_name" {
  description = "Specifies the name of the user node pool"
  default     = "agentpool"
  type        = string
}

variable "user_node_pool_subnet_name" {
  description = "Specifies the name of the subnet that hosts the user node pool"
  default     = "SystemSubnet"
  type        = string
}

variable "user_node_pool_subnet_address_prefix" {
  description = "Specifies the address prefix of the subnet that hosts the user node pool"
  default     = ["10.0.0.0/20"]
  type        = list(string)
}

variable "user_node_pool_enable_auto_scaling" {
  description = "(Optional) Whether to enable auto-scaler. Defaults to false."
  type        = bool
  default     = false
}

variable "user_node_pool_enable_host_encryption" {
  description = "(Optional) Should the nodes in this Node Pool have host encryption enabled? Defaults to false."
  type        = bool
  default     = false
}

variable "user_node_pool_enable_node_public_ip" {
  description = "(Optional) Should each node have a Public IP Address? Defaults to false. Changing this forces a new resource to be created."
  type        = bool
  default     = false
}

variable "user_node_pool_max_pods" {
  description = "(Optional) The maximum number of pods that can run on each agent. Changing this forces a new resource to be created."
  type        = number
  default     = 30
}

variable "user_node_pool_node_labels" {
  description = "(Optional) A list of Kubernetes taints which should be applied to nodes in the agent pool (e.g key=value:NoSchedule). Changing this forces a new resource to be created."
  type        = map(any)
  default     = { "kubernetes.azure.com/scalesetpriority" = "spot" }
}

variable "user_node_pool_node_taints" {
  description = "(Optional) A map of Kubernetes labels which should be applied to nodes in this Node Pool. Changing this forces a new resource to be created."
  type        = list(string)
  default     = ["kubernetes.azure.com/scalesetpriority=spot:NoSchedule"]
}

variable "user_node_pool_os_disk_type" {
  description = "(Optional) The type of disk which should be used for the Operating System. Possible values are Ephemeral and Managed. Defaults to Managed. Changing this forces a new resource to be created."
  type        = string
  default     = "Managed"
}

variable "user_node_pool_os_type" {
  description = "(Optional) The type of Operating System. The operating system used on each Node in this Node Pool."
  type        = string
  default     = "Linux"
}
variable "user_node_pool_priority" {
  description = "(Optional) The priority of the Virtual Machines in the Virtual Machine Scale Set backing this Node Pool."
  type        = string
  default     = "Regular"
}


variable "user_node_pool_max_count" {
  description = "(Required) The maximum number of nodes which should exist within this Node Pool. Valid values are between 0 and 1000 and must be greater than or equal to min_count."
  type        = number
  default     = 5
}

variable "user_node_pool_min_count" {
  description = "(Required) The minimum number of nodes which should exist within this Node Pool. Valid values are between 0 and 1000 and must be less than or equal to max_count."
  type        = number
  default     = 1
}

variable "user_node_pool_node_count" {
  description = "(Optional) The initial number of nodes which should exist within this Node Pool. Valid values are between 0 and 1000 and must be a value in the range min_count - max_count."
  type        = number
  default     = 2
}
variable "user_node_pool_os_disk_size_gb" {
  description = "(Optional) The size of the OS Disk on each Node in this Node Pool."
  type        = number
  default     = 128
}
```

**Define variables**

`dev-variables.tfvar` - update this existing file for AKS values for development environment.

``` tf title="dev-variables.tfvar"
# user_node_pool
user_node_pool_enabled              = true
user_node_pool_enable_auto_scaling  = true
user_node_pool_max_count            = 5
user_node_pool_min_count            = 1
user_node_pool_max_pods             = 110
user_node_pool_node_count           = 1
user_node_pool_node_labels          = {"TenantName" = "tenant1"}
user_node_pool_node_taints          = ["TenantName=tenant1:NoSchedule"]
user_node_pool_name                 = "usernodepool" #"sysnodepool"
user_node_pool_os_disk_size_gb      = 128
user_node_pool_vm_size              = "Standard_B8ms"
user_node_pool_availability_zones   = ["1", "2", "3"]
```

## Step 2: Create a New User Node Pool in AKS Using Terraform

Define the new user node pool in your Terraform configuration. Ensure that you specify the desired taint on the node pool.

``` tf title="aks.tf"

resource "azurerm_kubernetes_cluster_node_pool" "user" {
  count                 = var.user_node_pool_enabled ? 1 : 0
  zones                 = var.user_node_pool_availability_zones
  enable_auto_scaling   = var.user_node_pool_enable_auto_scaling
  os_disk_type          = var.user_node_pool_os_disk_type
  os_type               = var.user_node_pool_os_type
  priority              = var.user_node_pool_priority
  os_disk_size_gb       = var.user_node_pool_os_disk_size_gb
  vm_size               = var.user_node_pool_vm_size
  kubernetes_cluster_id = azurerm_kubernetes_cluster.aks.id
  max_count             = var.user_node_pool_max_count
  min_count             = var.user_node_pool_min_count
  max_pods              = var.user_node_pool_max_pods
  node_count            = var.user_node_pool_node_count
  node_labels           = var.user_node_pool_node_labels
  node_taints           = var.user_node_pool_node_taints
  mode                  = "User"
  name                  = var.user_node_pool_name
  # orchestrator_version  = data.azurerm_kubernetes_service_versions.current.latest_version
  vnet_subnet_id = azurerm_subnet.aks.id


  tags = merge(local.default_tags, var.aks_tags)
  lifecycle {
    ignore_changes = [
      tags,
    ]
  }
}
```

**Terraform validate**

```sh
terraform validate
# output
Success! The configuration is valid.
```

run terraform plan & apply again here.


**Terraform plan **

```sh
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"

```


**terraform apply**

```sh
terraform apply dev-plan


# output
azurerm_kubernetes_cluster_node_pool.user["1"]: Creating...
.
.
.

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:
```

## Step 3: Verify the New User Node Pool Taint

Use the Azure CLI or Kubernetes `kubectl` to verify that the taint has been applied to the new user node pool.

```bash
az aks nodepool show -g <resource-group-name> -n <new-node-pool-name> --cluster-name <aks-cluster-name> --query "taints" --output table
```


Node list status before and after 

**Before**

```bash
kubectl get nodes -o wide

# output
NAME                                   STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-agentpool-25316841-vmss000000      Ready    agent   27h   v1.26.6   10.64.4.4     <none>        Ubuntu 22.04.3 LTS   5.15.0-1041-azure   containerd://1.7.1+azure-1
aks-agentpool-25316841-vmss000001      Ready    agent   26h   v1.26.6   10.64.4.113   <none>        Ubuntu 22.04.3 LTS   5.15.0-1041-azure   containerd://1.7.1+azure-1
```

**After**

```bash
kubectl get nodes -o wide

# output
NAME                                   STATUS   ROLES   AGE   VERSION   INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION      CONTAINER-RUNTIME
aks-agentpool-25316841-vmss000000      Ready    agent   27h   v1.26.6   10.64.4.4     <none>        Ubuntu 22.04.3 LTS   5.15.0-1041-azure   containerd://1.7.1+azure-1
aks-agentpool-25316841-vmss000001      Ready    agent   26h   v1.26.6   10.64.4.113   <none>        Ubuntu 22.04.3 LTS   5.15.0-1041-azure   containerd://1.7.1+azure-1
aks-usernodepool-19872531-vmss000000   Ready    agent   21m   v1.26.6   10.64.4.222   <none>        Ubuntu 22.04.3 LTS   5.15.0-1041-azure   containerd://1.7.1+azure-1
```

## Step 4: Deploy an Application with Tolerations and Affinity

Create a Kubernetes manifest for your application with tolerations that match the taint on the node pool.


```bash
      tolerations:
        - key: "TenantName"
          operator: "Equal"
          value: "tenant1"
          effect: "NoSchedule"  # This pod tolerates the taint
```


```yaml title="deployment.yaml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aspnet-api
  namespace: sample
spec:
  replicas: 1
  selector: 
    matchLabels:
      app: aspnet-api
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5 
  template:
    metadata:
      labels:
        app: aspnet-api
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      serviceAccountName: default
      containers:
        - name: aspnet-api
          image: acr1dev.azurecr.io/sample/aspnet-api:20230323.15
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
      tolerations:
        - key: "TenantName"
          operator: "Equal"
          value: "tenant1"
          effect: "NoSchedule"  # This pod tolerates the taint
# kubectl apply -f deployment.yaml -n sample
```

## Step 5: Deploy the Application

Apply the Kubernetes manifest to deploy your application to the AKS cluster.

```bash
kubectl apply -f deployment.yaml
```

## Step 6: Verify Your Application Status

Check the status of your deployed application pods using `kubectl`.


Pod status before and after 
Before

```bash
# output
sample              aspnet-api-7d96f84c56-88dnz                         1/1     Running   0          3m31s   10.64.4.225   aks-agentpool-25316841-vmss000001   <none>           <none>
sample              aspnet-app-c5c847d44-tfhw6                          1/1     Running   0          7s      10.64.4.248   aks-agentpool-25316841-vmss000001   <none>           <none>
```

After

```bash
kubectl get pods -o wide -A

# output
sample              aspnet-api-7d96f84c56-88dnz                         1/1     Running   0          3m31s   10.64.4.225   aks-usernodepool-19872531-vmss000000   <none>           <none>
sample              aspnet-app-c5c847d44-tfhw6                          1/1     Running   0          7s      10.64.4.248   aks-usernodepool-19872531-vmss000000   <none>           <none>
```

You should see your application pods running on nodes with the corresponding taint.

You've successfully created a new user node pool in AKS using Terraform, applied a taint to it, and deployed an application with tolerations to ensure it runs on nodes with the taint.