## Introduction

Managing and using secretes, certificates is something very important part of any kind of project.

Default Kubernetes secrete management will not be sufficient for securing secretes; they are just base64 encoded strings which can be easily decoded anyone. true security is, using Azure Key vault for storing secretes.

By using the `Azure Key Vault Provider for Secrets Store CSI Driver`, you can securely store secrets in Azure Key Vault and use them in your applications without having to manage the secrets directly in your Kubernetes cluster.

The `Azure Key Vault Provider for Secrets Store CSI Driver` is a Container Storage Interface (CSI) driver that allows you to use Azure Key Vault as a secrets store for your Kubernetes cluster. It provides a way for your pods to securely access secrets stored in Key Vault and use them in your applications. The driver acts as a bridge between your Kubernetes cluster and Azure Key Vault.

## Technical Scenario

As a `cloud engineer` you've been asked to integrate Azure Key Vault service with Azure Kubernetes Service(AKS)
so that all your application configuration is fully secured. we are going to use Azure Key Vault Provider for Secrets Store CSI Driver for achiving the goal.


## Prerequisites

Before start the implementation, ensure you have the following prerequisites:

- Azure subscription
- Terraform installed and configured
- Azure CLI, Kubectl, Helm tools installed
- AKS cluster
- Azure Key Vault


## Objective

In this exercise, we will cover the following steps to integrating Azure key vault with AKS using terraform:

1. **Step-1:** Install the Secrets Store CSI Driver and the Azure Keyvault Provider

2. **Step 2:** Craete access policy in Azure Key Vault for AKS cluster

3. **Step 3:** Create Secret Provider Class

4. **Step 4:** Update and Apply Deployment YAML File

5. **Step 5:** Create Secret in Azure Key Vault

6. **Step 6:** Validate the Secrets Store Deployment

## Architecture Diagram

This digram explains components used in this lab like CSI driver, Keyvault Provider & AKS VMSS access in Key vault.
 
![Alt text](images\image-32.png)

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

This guide will take you through the steps to configure and run the Azure Key Vault Provider for Secrets Store CSI driver on Kubernetes for Integrating Azure Key Vault with AKS cluster.

Utilize Terraform to deploy the Secrets Store CSI Driver Helm chart onto AKS.

## Step-1: Install the Secrets Store CSI Driver and the Azure Keyvault Provider

To install Helm charts using Terraform, you'll need to add the necessary Terraform providers. Here's an example of how to include them in your `provider.tf` file:

Let's add following terraform providers in `provider.tf` file

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

``` tf title="aks-kv-csi-driver.tf"

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

**Validation**

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
csi-secrets-store-provider-azure-tpb4j   1/1     Running   0          3h52m
```

!!! note
    I recently observed that the latest version of AKS clusters now includes these pods by default. Therefore, if you have created an AKS cluster with the latest Kubernetes version, you can safely skip step-1.

## Step-2: Craete access policy in Azure Key Vault for AKS cluster

Granting the necessary permissions for the Kubernetes cluster to access secrets in the Key Vault is a crucial step in securing your applications. There are multiple approaches to achieve this, such as using Helm charts, Azure Command-Line Interface (az) commands, or Azure Active Directory (AAD) pod identity. In this guide, we'll focus on granting access to the Azure Kubernetes Service (AKS) Virtual Machine Scale Set (VMSS) identity, when I tried with AAD pod identity I've encountered couple of issues therefore I am selecting VMSS identity here.


Manual Steps: Enable Managed Service Identity (MSI) for the AKS VMSS.

1. Navigate to the AKS MC_ resource group in the Azure portal, typically named in the format `MC_rg-rgname-dev_aks-aksname-dev_region`.

2. Select the AKS Virtual Machine Scale Set (VMSS) - often named something like `aks-agentpool-12345678-vmss`.

3. In the left navigation pane, click on "Identity," and set the Status to "On" in the "System Assigned" tab. This action creates a new MSI. Copy the generated ID; you'll need it later.

Update the Terraform script with the AKS VMSS identity.

Once the AKS VMSS identity is enabled, proceed to update the Terraform script to incorporate this identity. Below are the steps:

- In the Terraform script, create a new file (e.g., `role_assignment.tf`) and add the following code:

``` tf title="role_assignment.tf"
  resource "azurerm_key_vault_access_policy" "aks_vmss_access_policy" {
    key_vault_id = azurerm_key_vault.kv.id
    tenant_id    = data.azurerm_subscription.current.tenant_id
    object_id    = var.aks_vmss_identity // Replace with the Object (principal) ID of the VMSS

    secret_permissions = [
      "Get",
    ]

    depends_on = [
      azurerm_key_vault.kv
    ]
  }
```

- Declare a variable for AKS VMSS identity in your Terraform script:

``` tf title="variables.tf"
  variable "aks_vmss_identity" {
    description = "Manually enable the AKS VMSS MSI before running this."
    type        = string
  }
```

- Copy the Object (principal) ID of the VMSS generated during the manual step and update the variable in the Terraform script:

``` tf title="variables.tf"
  aks_vmss_identity = "ddfd8a57-316e-44ba-b5ea-dc1824d9b480"
```

**Validate Key Vault Access Policy**

The Terraform code above creates a new Key Vault access policy, granting the specified permissions to the AKS VMSS identity. Note that the `object_id` should be dynamically retrieved; however, the solution to achieve dynamic reading is pending.

Ensure to automate the manual step in the future for a fully streamlined process.


## Step-3: Create Secret Provider Class

To effectively utilize and configure the Secrets Store CSI Driver for your Azure Kubernetes Service (AKS) cluster, you'll need to create a `SecretProviderClass` custom resource. This is an essential step as it defines how the Secrets Store CSI Driver interacts with your Azure Key Vault and synchronizes secrets to your Kubernetes cluster.


The creation of the `SecretProviderClass` can be seamlessly integrated into your Helm charts or YAML manifests. Below is an example YAML file (secretproviderclass.yaml) illustrating a SecretProviderClass setup using a system-assigned identity to access the Azure Key Vault:


``` yaml title="secretproviderclass.yaml"

# This is a SecretProviderClass example using system-assigned identity to access with key vault
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretProviderClass
metadata:
  name: keyvault-secret-provider
  namespace: sample
spec:
  provider: azure
  secretObjects:                              # [OPTIONAL] SecretObjects defines the desired state of synced Kubernetes secret objects
  - secretName: secret-provider-dev                   # name of the Kubernetes secret object
    type: Opaque                              # type of Kubernetes secret object (for example, Opaque, kubernetes.io/tls)
    data:
    - objectName: sample-secret               # name of the mounted content to sync; this could be the object name or the object alias
      key: sample-secret                      # data field to populate
    - objectName: connectionstring-dbname     # name of the mounted content to sync; this could be the object name or the object alias
      key: connectionstring-dbname            # data field to populate
    
  parameters:
    usePodIdentity: "false"
    useVMManagedIdentity: "true"    # Set to true for using managed identity
    userAssignedIdentityID: ""      # If empty, then defaults to use the system assigned identity on the VM
    keyvaultName: kv-kvname-dev      # azure key vault service name
    cloudName: "AzurePublicCloud"   # [OPTIONAL for Azure] if not provided, the Azure environment defaults to AzurePublicCloud
    objects:  |
      array:
        - |
          objectName: sample-secret
          objectType: secret        # object types: secret, key, or cert
          objectVersion: ""         # [OPTIONAL] object versions, default to latest if empty
        - |
          objectName: connectionstring-dbname
          objectType: secret        # object types: secret, key, or cert
          objectVersion: ""         # [OPTIONAL] object versions, default to latest if empty
    tenantId: ee93c4a0-f87d-46ad-b4be-3ee05cefecws  # The tenant ID of the key vault
```

Explanation:

- provider: Specifies the provider, in this case, "azure."
- secretObjects: Defines the desired state of synced Kubernetes secret objects.
- parameters: Configuration parameters for the Secrets Store CSI Driver.
- objects: Specifies the array of objects to sync from Azure Key Vault to the Kubernetes cluster.

**kubectl apply** 

Execute the following kubectl apply command to apply the configuration:

```sh
kubectl apply -f secretproviderclass.yaml

# output 

# secretproviderclass.secrets-store.csi.x-k8s.io/azure-kvname-system-msi created
```
**validation**

```sh
kubectl get secretproviderclass -n sample

# output

NAME                       AGE
keyvault-secret-provider   4m12s
```

This confirms that the `SecretProviderClass` named `keyvault-secret-provider` has been successfully created in the `sample` namespace.

For more details, you can refer to the official [Microsoft documentation](https://learn.microsoft.com/en-us/azure/aks/hybrid/secrets-store-csi-driver)



## Step-4: Update and apply deployment YAML file

To seamlessly integrate the Secrets Store CSI Driver into your Azure Kubernetes Service (AKS) cluster, you must update your deployment YAML file to reference the newly created `SecretProviderClass`. This ensures that your applications can securely access and utilize the secrets stored in Azure Key Vault.


``` yaml title="aspnet-api.yalm"

apiVersion: apps/v1
kind: Deployment
metadata:
  name: aspnet-api
  namespace: tenant1
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
        - name: aspnet-api-image
          image: acrname.azurecr.io/aspnet-api:20221006.2
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          volumeMounts:
          - name: secrets-store01-inline
            mountPath: "/mnt/secrets-store"
            readOnly: true

          env:
            - name: ASPNETCORE_ENVIRONMENT
              value: "Development"
            - name: ConnectionStrings__CoreDB
              valueFrom:
                secretKeyRef:
                  name: secret-provider-dev
                  key: connectionstring-coredb
      volumes:
        - name: secrets-store01-inline
          csi:
            driver: secrets-store.csi.k8s.io
            readOnly: true
            volumeAttributes:
              secretProviderClass: "keyvault-secret-provider"

```

Explanation:
- volumeMounts: Mounts the secrets store volume to the specified path in your container.
- volumes: Specifies the volume configuration, including the CSI driver and the associated SecretProviderClass.

Then, apply the updated deployment YAML file to the cluster:

Once you've updated your deployment YAML file, apply the changes to the cluster using the following kubectl apply command:

```sh
kubectl apply -f aspnet-api.yaml 
```

This action ensures that your deployment now incorporates the Secrets Store CSI Driver, and your application pods will securely access the secrets from Azure Key Vault.

## Step-5: Create Secret in Azure Key Vault

Azure Key Vault can store keys, secrets, and certificates. In the following example, a plain-text secret called ExampleSecret is configured.

```sh
az keyvault secret set --vault-name <keyvault-name> -n ExampleSecret --value MyAKSHCIExampleSecret
```

## Step-6: Validate the Secrets Store deployment

To show the secrets that are held in secrets-store, run the following command:

```sh
kubectl exec busybox-secrets-store-inline --namespace kube-system -- ls /mnt/secrets-store/

# output

ExampleSecret
```

To show the test secret held in secrets-store, run the following command:

```sh
kubectl exec busybox-secrets-store-inline --namespace kube-system -- cat /mnt/secrets-store/ExampleSecret

#output

MyAKSHCIExampleSecret
```


## References

Here is a comprehensive list of resources that were used in the development of this technical guide:

1. [Use Azure Key Vault Provider for Kubernetes Secrets Store CSI Driver with AKS hybrid](https://learn.microsoft.com/en-us/azure/aks/hybrid/secrets-store-csi-driver) - Microsoft Azure's official documentation on utilizing the Azure Key Vault Provider for Kubernetes Secrets Store CSI Driver with AKS.

2. [MSDN Documentation](https://docs.microsoft.com/en-us/azure/aks/hybrid/secrets-store-csi-driver) - Refer to the MSDN documentation for detailed insights into how secrets are pulled from the Key Vault service.

3. [secrets-store-csi-driver-provider-azure GitHub Repo](https://github.com/Azure/secrets-store-csi-driver-provider-azure) - Explore the GitHub repository for the Azure provider of the Secrets Store CSI Driver.

4. [Azure Key Vault Provider for Secrets Store CSI Driver Documentation](https://secrets-store-csi-driver.sigs.k8s.io/providers/azure/) - The official documentation for the Azure Key Vault Provider for Secrets Store CSI Driver.

5. [Helm Chart Installation details](https://secrets-store-csi-driver.sigs.k8s.io/getting-started/installation.html#helm-installation) - Get detailed information on installing Helm Charts for the Secrets Store CSI Driver.
<!-- 
6. [How-To: Deploy Microservice Application with Secrets Store CSI Driver Using Helm Chart - MSDN link](https://docs.microsoft.com/en-us/azure/aks/hybrid/how-to-secrets-store-csi-driver-app) - A step-by-step guide on deploying microservices with the Secrets Store CSI Driver using Helm.

7. [HOW-TO: Deploy AKS with POD Managed Identity and CSI using Terraform and Azure Pipeline - MSDN link](https://docs.microsoft.com/en-us/azure/aks/hybrid/how-to-secrets-store-csi-driver-akv) - Learn how to deploy AKS with pod-managed identity and CSI using Terraform and Azure Pipeline.

8. [addon-kv-csi-driver Helm chart deployment using Terraform](https://secrets-store-csi-driver.sigs.k8s.io/howto/deployment/helmaddon.html) - A resource for deploying the CSI driver Helm chart using Terraform.

9. [AAD Pod Identity - Helm Chart deployment using Terraform](https://secrets-store-csi-driver.sigs.k8s.io/howto/deployment/aadpodid.html) - Explore Helm chart deployment using Terraform with AAD Pod Identity. -->

**Troubleshooting Resources**

10. [Troubleshooting: Secret is not creating in AKS after fetching it with CSI driver](https://stackoverflow.com/questions/70154936/secret-is-not-creating-in-aks-after-fetching-it-with-csi-driver) - Stack Overflow discussion on troubleshooting issues with secret creation in AKS.

11. [AKS with Azure Key Vault: Env variables don't load](https://serverfault.com/questions/1075149/aks-with-azure-key-vault-env-variables-dont-load) - Server Fault thread addressing environment variable loading issues with AKS and Azure Key Vault.

12. [Azure Key Vault integration with AKS - Not clear what to do if not working](https://stackoverflow.com/questions/66296659/finally-got-key-vault-integrated-with-aks-but-not-clear-what-i-need-to-do-if) - Stack Overflow discussion on integration issues with AKS and Key Vault.

13. [GitHub Issue: Azure Key Vault integration with AKS](https://github.com/kubernetes-sigs/secrets-store-csi-driver/issues/236) - GitHub issue addressing problems related to Azure Key Vault integration.

14. [AKS and Azure Key Vault Integration](https://stackoverflow.com/questions/66308097/azure-key-vault-integration-with-aks-works-for-nginx-tutorial-pod-but-not-actua/66341260#66341260) - Stack Overflow discussion on integrating Azure Key Vault with AKS.

15. [Reloading an env variable synced to a mounted Azure Key Vault secret](https://roykim.ca/2022/11/13/how-to-reload-an-env-variable-syncd-to-a-mounted-azure-key-vault-secret/) - A guide on reloading environment variables synced with Azure Key Vault secrets.

16. [Known Limitations of Secrets Store CSI Driver](https://secrets-store-csi-driver.sigs.k8s.io/known-limitations.html#mounted-content-and-kubernetes-secret-not-updated) - Understand the known limitations of the Secrets Store CSI Driver.

17. [Enable and Disable Autorotation](https://learn.microsoft.com/en-us/azure/aks/csi-secrets-store-driver#enable-and-disable-autorotation) - Learn about enabling and disabling autorotation with the CSI Secrets Store Driver.

Feel free to explore these resources for a more in-depth understanding and troubleshooting of the Secrets Store CSI Driver integration with Azure Kubernetes Service.
