## Introduction

Managing and using secretes, certificates is something very important part of any kind of project.

Default Kubernetes secrete management will not be sufficient for securing secretes; they are just base64 encoded strings which can be easily decoded anyone. true security is, using Azure Key vault for storing secretes.

By using the `Azure Key Vault Provider for Secrets Store CSI Driver`, you can securely store secrets in Azure Key Vault and use them in your applications without having to manage the secrets directly in your Kubernetes cluster.

The `Azure Key Vault Provider for Secrets Store CSI Driver` is a Container Storage Interface (CSI) driver that allows you to use Azure Key Vault as a secrets store for your Kubernetes cluster. It provides a way for your pods to securely access secrets stored in Key Vault and use them in your applications. The driver acts as a bridge between your Kubernetes cluster and Azure Key Vault.

## Technical Scenario

As a `cloud engineer` you've been asked to integrate Azure Key Vault service with Azure Kubernetes Service(AKS)
so that all your application configuration is fully secured. we are going to use Azure Key Vault Provider for Secrets Store CSI Driver for achiving the goal.


## Prerequisites

Before delving into the implementation, ensure you have the following prerequisites:

- Azure subscription
- Terraform installed and configured
- Azure CLI, Kubectl, Helm tools installed
- AKS cluster
- Azure Key Vault


## Objective

In this exercise, we will cover the following steps to integrating Azure key vault with AKS using terraform:

1. **Step-1:** Install the Secrets Store CSI Driver and the Azure Keyvault Provider

2. **Step 2:** Grant AKS Access to Key Vault

3. **Step 3:** Create Secret Provider Class

4. **Step 4:** Update and Apply Deployment YAML File

5. **Step 5:** Create Secret in Azure Key Vault

6. **Step 6:** Validate the Secrets Store Deployment

## Architecture Diagram

This digram from MSDN documentation explains step by step workflow of the CSI driver with pod identity.


[Diagram Placeholder - Working on it...]
 
![Alt text](images\image-7.png)


**login to Azure**

Before you start this lab, ensure you are logged into the correct Azure subscription using Visual Studio Code:

``` sh
# Login to Azure
az login 

# Shows current Azure subscription
az account show --output table

# Lists all available Azure subscriptions
az account list --output table

# Sets Azure subscription to desired subscription using ID
az account set -s "anji.keesari"
```

**Connect to Cluster**

To interact with your Azure Kubernetes Service (AKS) cluster, you need to establish a connection. Depending on your role, you can use either the User or Admin credentials:

``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# verify the aks connection by running following commands
kubectl get no
kubectl get namespace -A
```

## Implementation Details

This guide will take you through the steps to configure and run the Azure Key Vault Provider for Secrets Store CSI driver on Kubernetes.


Utilize Terraform to deploy the Secrets Store CSI Driver Helm chart onto AKS.

## Step-1: Install the Secrets Store CSI Driver and the Azure Keyvault Provider

In order to install any kind of helm charts from terraform we need following three providers; 


``` tf title="provider.tf"

provider "kubernetes" {
  host                   = azurerm_kubernetes_cluster.aks.kube_admin_config.0.host
  client_certificate     = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_certificate)
  client_key             = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_key)
  cluster_ca_certificate = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.cluster_ca_certificate)
  #load_config_file       = false
}

provider "helm" {
  debug = true

  kubernetes {
    host                   = azurerm_kubernetes_cluster.aks.kube_admin_config.0.host
    client_certificate     = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_certificate)
    client_key             = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_key)
    cluster_ca_certificate = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.cluster_ca_certificate)

  }
}
provider "kubectl" {
  host                   = azurerm_kubernetes_cluster.aks.kube_admin_config.0.host
  client_certificate     = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_certificate)
  client_key             = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.client_key)
  cluster_ca_certificate = base64decode(azurerm_kubernetes_cluster.aks.kube_admin_config.0.cluster_ca_certificate)
  load_config_file       = false
}
```

Since we've added new providers in terraform project we need to run terraform init once again;

**Terraform init**

```sh
terraform init
```

**Install Helm Chart using terraform**

`Deploy the CSI Driver:` You need to deploy the Azure Key Vault Provider for Secrets Store CSI Driver in your Kubernetes cluster. Here we are going to use Helm chart to deploy the driver.

Running the this helm chart will install both the Secrets Store CSI Driver and Azure Key Vault provider.

``` tf title="aks_addon_csi_driver.tf"

locals {
  csi_driver_settings = {
    linux = {
      enabled = true
      resources = {
        requests = {
          cpu    = "100m"
          memory = "100Mi"
        }
        limits = {
          cpu    = "100m"
          memory = "100Mi"
        }
      }
    }
    secrets-store-csi-driver = {
      install = true
      linux = {
        enabled = true
      }
      logLevel = {
        debug = true
      }
      syncSecret = {
        enabled = true
      }
    }
  }
}

resource "helm_release" "csi_driver" {
  name  = "csi-secrets-store-provider-azure"
  chart = "csi-secrets-store-provider-azure"
  #   version      = "0.0.8"
  repository   = "https://azure.github.io/secrets-store-csi-driver-provider-azure/charts"
  namespace    = "kube-system"
  max_history  = 4
  atomic       = true
  reuse_values = false
  timeout      = 1800
  values       = [yamlencode(local.csi_driver_settings)]
  depends_on = [
    azurerm_kubernetes_cluster.aks
  ]
}
```

Verify the Secrets Store CSI Driver and Azure Key Vault provider are successfully installed

To verify the `Secrets Store CSI Driver` is installed, run this command:

```sh
kubectl get pods -l app=secrets-store-csi-driver -n kube-system

# You should see the driver pods running on each agent node:
NAME                             READY   STATUS    RESTARTS   AGE
secrets-store-csi-driver-spbfq   3/3     Running   0          3h52m
```

To verify the Azure `Key Vault provider` is installed, run this command:

```sh
kubectl get pods -l app=csi-secrets-store-provider-azure -n kube-system

# You should see the provider pods running on each agent node:
NAME                                         READY   STATUS    RESTARTS   AGE
csi-csi-secrets-store-provider-azure-tpb4j   1/1     Running   0          3h52m
```

## Step-2: Grant AKS Access to Key Vault

Establish the necessary permissions to allow AKS access to Azure Key Vault securely.

## Step-3: Create Secret Provider Class

Define a Secret Provider Class to facilitate seamless integration between Kubernetes and Azure Key Vault.

## Step-4: Update and Apply Deployment YAML File

Update and apply the deployment YAML file to ensure the correct configuration of the Secrets Store CSI Driver.

## Step-5: Create Secret in Azure Key Vault

Add a secret to Azure Key Vault for secure storage.

## Step-6: Validate the Secrets Store Deployment

Verify the successful deployment of the Secrets Store CSI Driver.

## Challenges and Considerations

Integrating Azure Key Vault with Azure Kubernetes Service can be intricate and time-consuming, particularly in the presence of implementation issues. Given the diverse approaches outlined in the documentation, a step-by-step guide for complete automation using Terraform is crucial for a true Infrastructure as Code (IaC) approach.

## Architecture Diagram

Refer to the MSDN documentation for a detailed architecture diagram illustrating the step-by-step workflow of the CSI driver with pod identity.

---

Feel free to adjust this draft according to your preferences and specific details of your implementation.