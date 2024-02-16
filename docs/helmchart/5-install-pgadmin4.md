# Install Pgadmin4 in Azure Kubernetes Services (AKS) with Helmchart using Terraform

## Introduction


PostgreSQL is a powerful, open-source object-relational database management system (ORDBMS) that we use in most of our Microservices Architecture. Managing databases is always crucial in any development environment. PgAdmin4 plays a very important role here in managing the PostgreSQL database.


In this article, we'll explore how to set up pgAdmin4 on a Kubernetes cluster, specifically using Azure Kubernetes Service (AKS). We'll use tools like Helmcharts and Terraform to make the deployment process smooth and manageable. By the end, you'll have a clear understanding of how to deploy pgAdmin4 effectively using IaC tools, making database management in your Kubernetes environment. 


**pgAdmin**

pgAdmin is a free and open-source administration and management tool for the PostgreSQL database. It provides a graphical user interface (GUI) for managing and interacting with PostgreSQL databases, allowing users to perform tasks such as creating and modifying tables, managing users and permissions, and running SQL queries. 


**PostgreSQL**

PostgreSQL is a powerful, open-source object-relational database management system (ORDBMS) PostgreSQL is developed by a global community of contributors. It is available for multiple platforms such as Windows, Linux, and MacOS.

**pgAdmin vs. PostgreSQL**

While there are other ways to manage a PostgreSQL database, the simplest approach is to use a GUI tool to make interactions more visual and user-friendly. 

pgAdmin was created to help PostgreSQL users get the most out of their database. The purpose is to provide a graphical administration tool to make it easier to manipulate schema and data in PostgreSQL

## Objective

In this exercise we will accomplish & learn how to implement following:

- **Step 1:** Setup terraform configure for Pgadmin4 Install
- **Step 2.** Create a new namespace for Pgadmin4
- **Step 3.** Install Pgadmin4 helmchart using terraform
- **Step 4.** Setup SSO for Pgadmin4
- **Step 5.** Verify Pgadmin4 resources in AKS Cluster
- **Step 6.** Access Pgadmin4 locally - port forwarding
- **Step 7.** Pgadmin4 login with localhost

## Prerequisites

Before proceeding with the installation of Pgadmin4 in AKS using terraform, ensure you have the following prerequisites in place:

- Azure subscription - <https://azure.microsoft.com/en-us/free/>
- Install and configure Terraform - <https://www.terraform.io/downloads>
- Install azure CLI - <https://learn.microsoft.com/en-us/cli/azure/install-azure-cli>
- Install and set up kubectl - <https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/>
- AKS cluster - Ensure you have a running Kubernetes cluster available for Pgadmin4 deployment
<!-- - Setup Ingress controller in AKS
- Cert-Manager and Let's Encrypt Setup on AKS 
- A registered domain
- A DNS provider
- Public IP address -->

<!-- 
1. **Azure Subscription:** [Sign up for an Azure subscription](https://azure.microsoft.com/en-us/free/), if not already done.

2. **terraform:** [Install and configure terraform](https://www.terraform.io/downloads) on your local machine, including the Helm, Kubernetes, and Azure providers.

3. **Azure CLI:** [Install Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli) for Azure service interaction.

4. **Kubectl:** [Install and set up kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/) for managing Kubernetes clusters.

5. **Kubernetes Cluster:** Ensure you have a running Kubernetes cluster available for Pgadmin4 deployment.

6. **Git Repository:** Maintain a Git repository storing manifests for your applications, serving as Pgadmin4's deployment source. -->

## Implementation Details

Let's look into the step-by-step implementation details:

**login to Azure**

Verify that you are logged into the right Azure subscription before start anything in visual studio code

``` sh
# Login to Azure
az login 

# Shows current Azure subscription
az account show

# Lists all available Azure subscriptions
az account list

# Sets Azure subscription to desired subscription using ID
az account set -s "anji.keesari"
```

**Connect to Cluster**

Use the `az aks get-credentials` command to configure kubectl to connect to the AKS cluster.


``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get namespace -A
```

## Step-1: Setup terraform configure for Pgadmin4 Install

Launch Visual Studio Code and open your current terraform repository to begin working on your terraform configuration.

In order to install any Helmcharts using terraform configuration we need to have following terraform providers.

- helm provider
- Kubernetes provider
- Kubectl provider 

**Terraform Providers**

You can install the necessary providers by adding the following code in your Terraform configuration file:

Let's update our existing `provider.tf` file with new kubernetes, helm and kubectl providers:

``` tf title="provider.tf"
terraform {

  required_version = ">=0.12"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>2.0"
    }

    azuread = {
      version = ">= 2.26.0" // https://github.com/terraform-providers/terraform-provider-azuread/releases
    }
     kubernetes = {
      source  = "hashicorp/kubernetes"
      version = ">= 2.0.3"
    }
    helm = {
      source  = "hashicorp/helm"
      version = ">= 2.1.0"
    }
    
     kubectl = {
      source  = "gavinbunney/kubectl"
      version = ">= 1.7.0"
    }
  }
}

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



**Setting Up Pgadmin4 Locally Using Helm Charts**

Pgadmin4 can be installed using various methods. In this guide, we will walk through downloading the Pgadmin4 Helmchart source code from a Git repository and running it locally from Visual Studio Code to install it on an AKS cluster.

Downloading Pgadmin4 Helmcharts Source Code

1. Clone the Repository: Begin by cloning the Helmchart repository from the following Git repository: [rowanruseler/helm-charts](https://github.com/rowanruseler/helm-charts){:target="_blank"}. This repository contains the necessary Helm charts for deploying Pgadmin4.

2. Access Pgadmin4 Charts: Navigate to the `pgadmin4` folder within the Helmcharts repository. You can do this by accessing the following URL: [Pgadmin4 Helm Chart](https://github.com/rowanruseler/helm-charts/tree/master/charts/pgadmin4){:target="_blank"}.

3. Organize Your Project: For better management, create a new folder named `pgadmin4` within your Terraform project directory. This folder will contain all Pgadmin4 source code and configurations.

4. Configuration Files: Within the `pgadmin4` folder, either create a separate file named `pgadmin4.yaml` or use `values.yaml` to store configuration settings.

Customizing Pgadmin4 Configuration

Once the files are set up, customize Pgadmin4 according to your requirements:

Change-1: Configure Extra Secret Mounts

![Change-1 Configuration](image.png)

Change-2: Update Email & Password

![Change-2 Configuration](image.png)

Change-3: Update config_local.py File

Edit the `config_local.py` file to configure Single Sign-On (SSO) settings for later use:

```python
MASTER_PASSWORD_REQUIRED = True
AUTHENTICATION_SOURCES = ['oauth2', 'internal']
OAUTH2_AUTO_CREATE_USER = True
OAUTH2_CONFIG = [
  {
      'OAUTH2_NAME': 'microsoft',
      'OAUTH2_DISPLAY_NAME': 'Microsoft',
      'OAUTH2_CLIENT_ID': 'your-client-id-here',
      'OAUTH2_CLIENT_SECRET': 'your-client-secret-here',
      'OAUTH2_TOKEN_URL': 'https://login.microsoftonline.com/your-tenant-id-here/oauth2/v2.0/token',
      'OAUTH2_AUTHORIZATION_URL': 'https://login.microsoftonline.com/your-tenant-id-here/oauth2/v2.0/authorize',
      'OAUTH2_API_BASE_URL': 'https://graph.microsoft.com/oidc/',
      'OAUTH2_USERINFO_ENDPOINT': 'userinfo',
      'OAUTH2_ICON': 'fa-microsoft',
      'OAUTH2_BUTTON_COLOR': '#0000ff',
      'OAUTH2_SCOPE': 'openid email profile'
  }
]
```

Update Chart.yaml

If required, update the `appVersion` field in the `Chart.yaml` file to reflect any changes made to the application version.

Final folder structure

After completing the setup, your folder structure should resemble the following:

```
terraform_project/
└── pgadmin4/
    ├── pgadmin4.yaml
    ├── config_local.py
    ├── Chart.yaml
    └── ...
```


## Step-2: Create namespace for Pgadmin4

Create a separate namespace for Pgadmin4 where all pgadmin4 related resources will be created. let's create a new file called pgadmin4.tf and copy the following configuration.

``` tf title="pgadmin4.tf"
resource "kubernetes_namespace" "pgadmin4" {  
  metadata {
    name = "pgadmin4"
  }
}
```

run terraform validate & format

``` sh
terraform validate
terraform fmt
```

run terraform plan

``` sh
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
```
output

``` sh
Plan: 1 to add, 0 to change, 0 to destroy.
```
run terraform apply

```sh
terraform apply dev-plan
```

``` sh
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:
```

## Step-3: Install Pgadmin4 helmchart using terraform

Visit the official Pgadmin4 Helm chart on the ArtifactHUB website: [Pgadmin4 Helm Chart](https://artifacthub.io/packages/helm/runix/pgadmin4){:target="_blank"}.

Click on the "Install" button to retrieve the necessary details for the Pgadmin4 Helmchart installation.

``` sh
repository = "https://helm.runix.net"
chart      = "pgadmin4"
version    = "1.23.3"
```

``` tf title="pgadmin4.tf"
# Install pgadmin4 helm chart using terraform
resource "helm_release" "pgadmin4" {
  count      = var.pgadmin4
  name       = "pgadmin4"
  repository = "https://helm.runix.net"
  chart      = "pgadmin4"
  version    = "1.13.6"
  namespace  = kubernetes_namespace.pgadmin4[0].metadata.0.name
  values = [
    "${file("pgadmin4/pgadmin4.yaml")}"
  ]

  # set {
  #   name  = "env.email"
  #   value = "chart@domain.com"
  # }
  # set {
  #   name  = "env.password"
  #   value = data.azurerm_key_vault_secret.pgadmin4_admin_pwd.value
  # }
  depends_on = [
    kubernetes_namespace.pgadmin4
  ]
}
```

run terraform plan

```
terraform validate
terraform fmt
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
```
output
``` sh
Plan: 1 to add, 0 to change, 0 to destroy.
```

run terraform apply

```
terraform apply dev-plan
```
``` sh
helm_release.argocd: Creating...
helm_release.argocd: Still creating... [10s elapsed]
helm_release.argocd: Still creating... [20s elapsed]
helm_release.argocd: Still creating... [30s elapsed]
helm_release.argocd: Still creating... [40s elapsed]
helm_release.argocd: Still creating... [50s elapsed]
helm_release.argocd: Still creating... [1m0s elapsed]
helm_release.argocd: Creation complete after 1m5s [id=argocd]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs
```


## Step-4. Setup SSO for Pgadmin4

Create Pgadmin4 oauth2 YAML file for SSO

oauth2-config.yaml

```sh
#
# secrets.yaml
# To setup Google OAUTH
## https://support.google.com/googleapi/answer/6158849?hl=en#
# To setup Github OAUTH
## https://docs.github.com/en/developers/apps/building-oauth-apps/creating-an-oauth-app
# redirect|callback URI to set:
## https://pgadmin4.example.com/oauth2/authorize 
---
kind: Secret
apiVersion: v1
metadata:
  name: pgadmin4-config
type: Opaque
stringData:
  config_local.py: |-
    MASTER_PASSWORD_REQUIRED = True
    AUTHENTICATION_SOURCES = ['oauth2', 'internal']
    OAUTH2_AUTO_CREATE_USER = True
    OAUTH2_CONFIG = [
      {
          'OAUTH2_NAME': 'microsoft',
          'OAUTH2_DISPLAY_NAME': 'Microsoft',
          'OAUTH2_CLIENT_ID': 'your-client-id-here',
          'OAUTH2_CLIENT_SECRET': 'your-client-secret-here',
          'OAUTH2_TOKEN_URL': 'https://login.microsoftonline.com/ff93c4a0-f87d-46ad-b4be-3ee05cefec6b/oauth2/v2.0/token',
          'OAUTH2_AUTHORIZATION_URL': 'https://login.microsoftonline.com/ff93c4a0-f87d-46ad-b4be-3ee05cefec6b/oauth2/v2.0/authorize',
          'OAUTH2_API_BASE_URL': 'https://graph.microsoft.com/oidc/',
          'OAUTH2_USERINFO_ENDPOINT': 'userinfo',
          'OAUTH2_ICON': 'fa-microsoft',
          'OAUTH2_BUTTON_COLOR': '#0000ff',
          'OAUTH2_SCOPE': 'openid email profile'
      }
    ]
# kubectl apply -f .\pgadmin4\oauth2-config.yaml -n pgadmin4
```


Apply pgadmin4 oauth2 file:

terraform apply needs two parts

*data part*

```
data "kubectl_file_documents" "pgadmin4_oauth2" {
  content = file("pgadmin4/pgadmin4-config.yaml")
}
```
*resource part*

```
resource "kubectl_manifest" "pgadmin4_oauth2" {
  for_each  = data.kubectl_file_documents.pgadmin4_oauth2.manifests
  yaml_body = each.value
  depends_on = [
    data.kubectl_file_documents.pgadmin4_oauth2
  ]
}
```
terraform plan & apply at this stage

```
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```


## Step 5. Verify Pgadmin4 resources in AKS.

Validate to make sure all the deployments / services created and running as expected. 

Run the following `kubectl` commands to verify the pgadmin4 installation in the AKS cluster.

```sh
kubectl get all --namespace pgadmin4
# or
kubectl get all,configmaps,secrets -n pgadmin4
```

or

```sh
kubectl get namespace pgadmin4
kubectl get deployments -n pgadmin4
kubectl get pods -n pgadmin4
kubectl get services -n pgadmin4
```

expected output

```sh
NAME                            READY   STATUS    RESTARTS   AGE
pod/pgadmin4-65f9ff74f6-xbscn   1/1     Running   0          12d

NAME               TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE
service/pgadmin4   ClusterIP   10.25.181.231   <none>        80/TCP    18d

NAME                       READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/pgadmin4   1/1     1            1           18d

NAME                                  DESIRED   CURRENT   READY   AGE
replicaset.apps/pgadmin4-65f9ff74f6   1         1         1       18d

NAME                         DATA   AGE
configmap/kube-root-ca.crt   1      25d

NAME                                    TYPE                 DATA   AGE
secret/pgadmin4                         Opaque               1      18d
secret/pgadmin4-config                  Opaque               1      25d
secret/sh.helm.release.v1.pgadmin4.v1   helm.sh/release.v1   1      25d
secret/sh.helm.release.v1.pgadmin4.v2   helm.sh/release.v1   1      18d
secret/sh.helm.release.v1.pgadmin4.v3   helm.sh/release.v1   1      18d
secret/sh.helm.release.v1.pgadmin4.v4   helm.sh/release.v1   1      18d
secret/tls-secret                       kubernetes.io/tls    2      18d
```

```kubectl get configmaps -n argocd```

```
NAME                        DATA   AGE
argocd-cm                   6      3m6s
argocd-cmd-params-cm        29     3m6s
argocd-gpg-keys-cm          0      3m6s
argocd-notifications-cm     1      3m6s
argocd-rbac-cm              3      3m6s
argocd-ssh-known-hosts-cm   1      3m6s
argocd-tls-certs-cm         0      3m6s
kube-root-ca.crt            1      11m
```
```kubectl get secrets -n argocd```
```
NAME                           TYPE                 DATA   AGE
argocd-initial-admin-secret    Opaque               1      3m7s
argocd-notifications-secret    Opaque               0      3m23s
argocd-secret                  Opaque               5      3m23s
sh.helm.release.v1.argocd.v1   helm.sh/release.v1   1      3m24s
```

```kubectl get ingress -n argocd```

output

```
No resources found in argocd namespace.
```

## Step-6. Access Pgadmin4 locally - port forwarding


To perform local login testing for Pgadmin4, you can use port forwarding. Here are the steps:

1. Connect to your AKS cluster using kubectl
2. Forward the Pgadmin4 server port to your local machine:

```sh
kubectl port-forward service/pgadmin4 -n pgadmin4 8080:80
```
output

```sh
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
Handling connection for 8080
```
1. Access the Pgadmin4 web interface by visiting <https://localhost:8080> in your web browser.
!!! Note
    Use this URL for before patching - <https://localhost:8080>
    Use this URL for after patching - <http://localhost:8080/>

![image.jpg](images/image-2.jpg)

The port forwarding can be stopped by cancelling the command with CTRL + C.


login with following credentials

```
anjkeesari@gmail.com

StrongPassword@
```


**Trouble Shooting**


in case Login with Microsoft button is missing or workload not created then run the following from helmcharts repo.

```
helm upgrade pgadmin4 runix/pgadmin4 -f .\pgadmin4\pgadmin4.yaml --create-namespace -n pgadmin4
```

output
```sh

Release "pgadmin4" has been upgraded. Happy Helming!
NAME: pgadmin4
LAST DEPLOYED: Mon Dec  5 07:09:32 2022
NAMESPACE: pgadmin4
STATUS: deployed
REVISION: 11
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace pgadmin4 -l "app.kubernetes.io/name=pgadmin4,app.kubernetes.io/instance=pgadmin4" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:80
```



## Step-7. Create Ingress YAML file for pgadmin4 Url

```sh
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: pgadmin4
  annotations:
    kubernetes.io/ingress.class: azure/application-gateway
    appgw.ingress.kubernetes.io/health-probe-status-codes: "200-499"
    cert-manager.io/cluster-issuer: letsencrypt
    cert-manager.io/acme-challenge-type: http01
spec:
  tls:
  - hosts:
    - dev.pgadmin.mydomain.net
    secretName: tls-secret
  rules:
    - host: dev.pgadmin.mydomain.net
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: pgadmin4
                port:   
                  number: 80
status:
  loadBalancer:
    ingress:
      - ip: 20.125.213.106
      
# kubectl apply -f pgadmin4-ingress.yaml -n pgadmin4

```
terraform plan & apply 

```
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```


## Step-8. Deploy Ingress YAML file for pgadmin4 Url

```sh
# Create pgadmin4 ingress file
data "kubectl_file_documents" "pgadmin4_ingress" {
  content = file("pgadmin4/pgadmin4-ingress.yaml")
}

# Apply pgadmin4 ingress file
resource "kubectl_manifest" "pgadmin4_ingress" {
  for_each  = data.kubectl_file_documents.pgadmin4_ingress.manifests
  yaml_body = each.value
  depends_on = [
    data.kubectl_file_documents.pgadmin4_ingress
  ]
}
```

## Step-9:. Add new record set in public DNS Zone

![image.png](/.attachments/image-bc663684-fb87-46b4-bbc2-0d588214b221.png)

## Step-10: Test the pgadmin4 Url

Test the pgadmin4 Url with default credentials

https://dev.pgadmin.mydomain.net/login
```
chart@domain.com - default email address

SuperSecret - default password can be found in AKS->configuration->Secrets-pgadmin4 (opaque)
```
![image.png](/.attachments/image-f3d482ee-d839-487d-ab78-1a7cd63132c9.png)


**Single Sign on testing**

click on login with Microsoft->enter corp credentials->

**Redirect URIs**
Update the URL here for SSO

![image.png](/.attachments/image-b6fe5862-811a-4b48-9e20-ce36ab4885d9.png)


Helm Chart troubleshooting details


**helm get info cmd**

```
helm repo list
helm ls -aA
helm list --namespace pgadmin4
helm history pgadmin4 -n pgadmin4
```

**helm upgrade**

```
helm upgrade pgadmin4 runix/pgadmin4 -f .\pgadmin4\pgadmin4.yaml -n pgadmin4
```
output

```sh
Release "pgadmin4" has been upgraded. Happy Helming!
NAME: pgadmin4
LAST DEPLOYED: Mon Dec  5 07:09:32 2022
NAMESPACE: pgadmin4
STATUS: deployed
REVISION: 11
NOTES:
1. Get the application URL by running these commands:
  export POD_NAME=$(kubectl get pods --namespace pgadmin4 -l "app.kubernetes.io/name=pgadmin4,app.kubernetes.io/instance=pgadmin4" -o jsonpath="{.items[0].metadata.name}")
  echo "Visit http://127.0.0.1:8080 to use your application"
  kubectl port-forward $POD_NAME 8080:80
```
**helm rollback**

```sh
helm rollback pgadmin4 10 
```
output

```
PS C:\Source\Repos\helmcharts\appservices> helm rollback helm-release-ewm30 251
Rollback was a success! Happy Helming!
```
**helm uninstall**

```
helm uninstall pgadmin4 -n pgadmin4
```


## Reference

here is the list of all the resources used while working on this technical story 

- [pgAdmin home page](https://www.pgadmin.org/){:target="_blank"}
- [Helm Provider for Terraform](https://github.com/hashicorp/terraform-provider-helm){:target="_blank"}
<!-- 
- https://artifacthub.io/ - artifacthub for searching charts 
- https://artifacthub.io/packages/helm/runix/pgadmin4 - pgadmin4 Helm Chart
- https://github.com/rowanruseler/helm-charts/tree/master/charts/pgadmin4/examples - example for SSO
- https://www.pgadmin.org/docs/pgadmin4/6.15/index.html - documentation
- https://www.pgadmin.org/docs/pgadmin4/6.15/oauth2.html# - SSO
- https://github.com/rowanruseler/helm-charts/tree/master/charts/pgadmin4 - looks like this is the latest git repo
- https://www.postgresql.org/ftp/pgadmin/pgadmin4/v6.16/ -->