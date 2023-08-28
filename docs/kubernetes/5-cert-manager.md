

## Introduction


**Cert-Manager**

Cert-Manager is an open-source tool that automates the management and issuance of TLS certificates in Kubernetes. It is used to manage and automate the process of obtaining, renewing, and revoking X.509 certificates from various certificate authorities. Cert-Manager integrates with Kubernetes by creating custom resource definitions (CRDs) to define and manage certificate issuance, renewal, and revocation processes. This helps simplify and automate certificate management, reducing the chances of manual errors and security vulnerabilities. Cert-Manager is compatible with various certificate authorities, including Let's Encrypt, which provides free and open-source certificates.

**Let's Encrypt**

Let's Encrypt is a free, automated, and open certificate authority (CA). It provides domain-validated X.509 certificates for Transport Layer Security (TLS) encryption, which is used to secure web traffic. These certificates can be obtained and managed automatically, making it easier and more cost-effective for website owners to encrypt their sites. Let's Encrypt is a non-profit organization and its goal is to make encryption accessible and affordable for all websites. By providing free, easy-to-use certificates, Let's Encrypt aims to encourage widespread adoption of encryption, helping to create a safer and more secure internet. Certificates issued by Let's Encrypt are trusted by all major browsers and are renewable every 90 days.

**Key Features**

1. Automated issuance and renewal of certificates to secure Ingress with TLS.
2. Fully integrated issuers from recognized public and private Certificate Authorities.
3. Secure pod-to-pod communication with mTLS using private PKI issuers.
4. Supports certificate use cases for web-facing and internal workloads.
5. Open-source add-ons for enhanced cloud-native service mesh security.
6. Backed by major cloud service providers and distributions.

## Technical Scenario

As a `Cloud Engineer`  you have been asked to provision certificates for your applications deployed in Kubernetes, another requirement is that it should be Cloud native certificate management. Here we are going to use cert-Manager as a Kubernetes operator, that can provision certificates from certificate authorities like Let's Encrypt automatically.

**Install Cert-Manager helm chart using terraform**

Another requirement here is to make sure that installation of Cert-Manager in AKS cluster is completely automated, to fulfill this requirement we are going to use terraform configuration to install the Cert-Manager in our AKS.

## Prerequisites

Before proceeding with this exercise, ensure you have the following in place:

- A functioning Kubernetes cluster
- An active Azure subscription [Link](https://azure.microsoft.com/en-us/free/)
- Terraform installed and configured [Guide](https://www.terraform.io/downloads)
- Defined Terraform providers for Helm installation, including:
    - Helm provider
    - Kubernetes provider
    - Kubectl provider
- Azure CLI installed [Guide](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)
- Kubectl installed and set up [Guide](https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/)
- Helm client installed
- An AKS (Azure Kubernetes Service) cluster

## Implementation Details

In this exercise, we will cover the following steps to implement Cert-Manager in your AKS cluster:

1. **Step-1:** Define Terraform Providers for Helm Install (Setup Terraform)

2. **Step 2:** Create a New Namespace for Cert-Manager

3. **Step 3:** Install Cert-Manager Helm Chart Using Terraform

4. **Step 4:** Verify Cert-Manager Resources in AKS

5. **Step 5:** Create ClusterIssuer YAML File

      - Create ClusterIssuer using Terraform
      - Initialize Terraform
      - Create a Terraform execution plan
      - Apply a Terraform execution plan

6. **Step 6:** Test the Application with HTTPS URL

## Architecture Diagram

Below is a high-level architecture diagram depicting the components involved in Cert-Manager:

[Diagram Placeholder - Working on it...]
 
![Alt text](images\image-7.png)


**login to Azure**

Verify that you are logged into the right Azure subscription before start anything in visual studio code

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
``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get namespace -A
```


These steps will guide you through the process of setting up and using Cert-Manager in your AKS cluster. By the end of this exercise, you'll have a functional environment for automating the management of TLS certificates within Kubernetes.

## Step-1: Configure Terraform providers 


Launch Visual Studio Code and open your current Terraform repository to start working on your Terraform configuration.

To install Helm charts using Terraform configuration, we need the following Terraform providers:

   - Helm provider
   - Kubernetes provider
   - Kubectl provider

The Helm Provider allows you to manage your Helm charts and releases as part of your Terraform-managed infrastructure. It lets you define your charts as Terraform resources, simplifying their installation and updates.

With Terraform, you can handle the installation, upgrades, and removal of your Helm charts in a consistent, version-controlled way. This helps streamline infrastructure management, ensuring uniformity and reducing the chance of errors.

**Terraform Providers**

You can install these providers by adding the following code to your Terraform configuration file. Update the existing provider.tf file with these new providers:


``` tf title="provider.tf"
terraform {

  required_version = ">=0.12"

  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~>3.69.0" //"~>2.0"
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

provider "random" {}

provider "azurerm" {
  features {}
  skip_provider_registration = true
  subscription_id            = var.sp-subscription-id
  client_id                  = var.sp-client-id
  client_secret              = var.sp-client-secret
  tenant_id                  = var.sp-tenant-id
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

You can find more information about these providers by referring to their respective documentation:

- Helm Provider: [More info](https://registry.terraform.io/providers/hashicorp/helm/latest/docs)
- Kubernetes Provider: [More info](https://registry.terraform.io/providers/hashicorp/kubernetes/latest/docs)
- Kubectl Provider: [More info](https://registry.terraform.io/providers/gavinbunney/kubectl/latest/docs)

**Run terraform init**

After adding these new providers to the Terraform providers list, you need to run `terraform init` to initialize them:

```bash
terraform init
```

output
``` sh
Initializing the backend...

Initializing provider plugins...
- Reusing previous version of hashicorp/azurerm from the dependency lock file
- Reusing previous version of hashicorp/azuread from the dependency lock file
- Finding hashicorp/kubernetes versions matching ">= 2.0.3"...
- Reusing previous version of hashicorp/random from the dependency lock file
- Finding hashicorp/helm versions matching ">= 2.1.0"...
- Finding gavinbunney/kubectl versions matching ">= 1.7.0"...
- Installing hashicorp/kubernetes v2.18.1...
- Installed hashicorp/kubernetes v2.18.1 (signed by HashiCorp)
- Using previously-installed hashicorp/random v3.4.3
- Installing hashicorp/helm v2.9.0...
- Installed hashicorp/helm v2.9.0 (signed by HashiCorp)
- Installing gavinbunney/kubectl v1.14.0...
- Installed gavinbunney/kubectl v1.14.0 (self-signed, key ID AD64217B5ADD572F)
- Using previously-installed hashicorp/azurerm v3.31.0
- Using previously-installed hashicorp/azuread v2.33.0


Terraform has been successfully initialized!
```

This command initializes the provider plugins and ensures they are ready for use in your Terraform project.

Once the initialization process is complete, your Terraform project will be set up with the necessary providers for managing Helm charts and Kubernetes resources.


## Step-2: Create a new namespace for Cert Mananger

Creating a separate namespace for cert-manager is important to keep all cert-manager related AKS resources organized in a single space, logically distinct from other Kubernetes resources. Here's how to achieve that:

**Create the Namespace**

Let's start by creating a file named cert-manager.tf and paste the following Terraform configuration:

``` tf title="cert-manager.tf"
# create namespace for cert mananger
resource "kubernetes_namespace" "cert_manager" {
  metadata {
    labels = {
      "name" = "cert-manager"
    }
    name = "cert-manager"
  }
}
```
**Validate and Format**

Run these commands to validate and format your Terraform configuration:

``` sh
terraform validate
terraform fmt
```

**Plan the Changes**

``` sh
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
```
The output will list the changes Terraform is going to make, including creating the cert-manager namespace:

``` sh
 + create

Terraform will perform the following actions:

# kubernetes_namespace.cert_manager will be created
+ resource "kubernetes_namespace" "cert_manager" {
    + id                               = (known after apply)
    + wait_for_default_service_account = false

    + metadata {
        + generation       = (known after apply)
        + labels           = {
            + "name" = "cert-manager"
          }
        + name             = "cert-manager"
        + resource_version = (known after apply)
        + uid              = (known after apply)
      }
  }

Plan: 1 to add, 0 to change, 0 to destroy.
```
**Apply the Changes**

Apply the planned changes using the following command:

```
terraform apply dev-plan
```
``` sh
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:
```

After applying, you'll see that Terraform has added the cert-manager namespace. This isolation helps keep your cert-manager resources organized and distinguishable from others within your Kubernetes cluster.

## Step-3: Install cert-manager helm chart using terraform

Now, let's dive into installing the cert-manager Helm chart into your AKS cluster using Terraform. Here's the step-by-step process:

**1. Add the Helm repository**

First, you need to add the Helm repository for cert-manager. The official source for cert-manager charts is the Helm repository provided by Jetstack. Adding the repository is essential for accessing the chart securely:

``` sh
helm repo add jetstack https://charts.jetstack.io
```

**2. Update your local Helm chart repository cache:**

Make sure to update your local Helm chart repository cache to get the latest information.

```sh
helm repo update
```

**3. Install the Cert-Manager Helm Chart**

To install the cert-manager Helm chart, you'll need its details. These details can be found on the ArtifactHUB website. Navigate to the cert-manager chart page and click the Install button to get the required information.

<https://artifacthub.io/packages/helm/cert-manager/cert-manager> - click on Install button and get following helm-chart details

``` sh
repository = "https://charts.jetstack.io"
chart      = "cert-manager"
version    = "1.12.3"
```

In your Terraform configuration, create a new resource using the `helm_release` block:

``` tf title="cert-manager.tf"
# Install cert-manager helm chart using terraform
resource "helm_release" "cert_manager" {
  name       = "cert-manager"
  repository = "https://charts.jetstack.io"
  chart      = "cert-manager"
  version    = "v1.12.3"
  namespace  = kubernetes_namespace.cert_manager.metadata.0.name

  set {
    name  = "installCRDs"
    value = "true"
  }
  depends_on = [
    kubernetes_namespace.cert_manager    
  ]
}
```

**Plan and Apply**

Run the following commands to validate, plan, and apply the installation of the cert-manager Helm chart using Terraform:

```sh
terraform validate
terraform plan -out=dev-plan -var-file="./environments/dev-variables.tfvars"
terraform apply dev-plan
```
output
``` sh
 + create

Terraform will perform the following actions:

# helm_release.cert_manager will be created
+ resource "helm_release" "cert_manager" {
    + atomic                     = false
    + chart                      = "cert-manager"
    + cleanup_on_fail            = false
    + create_namespace           = false
    + dependency_update          = false
    + disable_crd_hooks          = false
    + disable_openapi_validation = false
    + disable_webhooks           = false
    + force_update               = false
    + id                         = (known after apply)
    + lint                       = false
    + manifest                   = (known after apply)
    + max_history                = 0
    + metadata                   = (known after apply)
    + name                       = "cert-manager"
    + namespace                  = "cert-manager"
    + pass_credentials           = false
    + recreate_pods              = false
    + render_subchart_notes      = true
    + replace                    = false
    + repository                 = "https://charts.jetstack.io"
    + reset_values               = false
    + reuse_values               = false
    + skip_crds                  = false
    + status                     = "deployed"
    + timeout                    = 300
    + verify                     = false
    + version                    = "v1.12.3"
    + wait                       = true
    + wait_for_jobs              = false

    + set {
        + name  = "installCRDs"
        + value = "true"
      }
  }

Plan: 1 to add, 0 to change, 0 to destroy.
```

Check the plan output to ensure everything is as expected.

Apply the plan to actually install the cert-manager Helm chart:

```
terraform apply dev-plan
```
``` sh
helm_release.cert_manager: Creating...
helm_release.cert_manager: Still creating... [10s elapsed]
helm_release.cert_manager: Still creating... [20s elapsed]
helm_release.cert_manager: Still creating... [30s elapsed]
helm_release.cert_manager: Still creating... [40s elapsed]
helm_release.cert_manager: Still creating... [50s elapsed]
helm_release.cert_manager: Creation complete after 59s [id=cert-manager]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:
```

Terraform will take care of installing the cert-manager Helm chart with the specified version into your AKS cluster. This process will provide the necessary infrastructure to manage and issue TLS certificates within your Kubernetes environment.

## Step 4. Verify cert-manager resources in AKS.

After successfully installing the cert-manager Helm chart in your AKS cluster, the next step is to verify the installation. This involves checking the deployed resources and ensuring everything is running as expected. Here's how you can verify the cert-manager installation:

**Connect to AKS cluster**

Use the Azure CLI to connect to your AKS cluster:

``` sh
# azure CLI

# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin
```

**Get Cert-Manager Resources**

Run the following commands to get information about the cert-manager resources:

``` sh
kubectl get namespace cert-manager
kubectl get deployments -n cert-manager
kubectl get pods -n cert-manager
kubectl get services -n cert-manager
kubectl get configmaps -n cert-manager
kubectl get secrets -n cert-manager
```

**Get all** resources related to cert-manager.

Run the following command to get a comprehensive list of all resources related to cert-manager:

```sh
kubectl get all -n cert-manager
```
```
NAME                                           READY   STATUS    RESTARTS   AGE
pod/cert-manager-5674b9b755-vgpxw              1/1     Running   0          15m
pod/cert-manager-cainjector-557c547f54-n748g   1/1     Running   0          15m
pod/cert-manager-webhook-86868b95db-lgg8d      1/1     Running   0          15m

NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/cert-manager           ClusterIP   10.25.117.167   <none>        9402/TCP   16m
service/cert-manager-webhook   ClusterIP   10.25.63.104    <none>        443/TCP    16m

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cert-manager              1/1     1            1           16m
deployment.apps/cert-manager-cainjector   1/1     1            1           16m
deployment.apps/cert-manager-webhook      1/1     1            1           16m

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/cert-manager-5674b9b755              1         1         1       16m
replicaset.apps/cert-manager-cainjector-557c547f54   1         1         1       16m
replicaset.apps/cert-manager-webhook-86868b95db      1         1         1       16m
```

**Get Configmaps Details**

```
kubectl get configmaps -n cert-manager
```

```
NAME                   DATA   AGE
cert-manager-webhook   0      16m
kube-root-ca.crt       1      30m
```

**Get Secrets**

```
kubectl get secrets -n cert-manager
```
```
NAME                                 TYPE                 DATA   AGE
cert-manager-webhook-ca              Opaque               3      17m
sh.helm.release.v1.cert-manager.v1   helm.sh/release.v1   1      17m
```

**Get Ingress**

```
kubectl get ingress -n cert-manager
```

output

```
No resources found in cert-manager namespace.
```

**get CRDs**

``` sh
kubectl get clusterrole -n cert-manager
kubectl get clusterrolebinding -n cert-manager
kubectl get CustomResourceDefinition -n cert-manager

or
kubectl get crd -l app.kubernetes.io/name=cert-manager
```

```sh
NAME                                  CREATED AT
certificaterequests.cert-manager.io   2023-08-27T15:11:41Z
certificates.cert-manager.io          2023-08-27T15:11:41Z
challenges.acme.cert-manager.io       2023-08-27T15:11:41Z
clusterissuers.cert-manager.io        2023-08-27T15:11:41Z
issuers.cert-manager.io               2023-08-27T15:11:42Z
orders.acme.cert-manager.io           2023-08-27T15:11:41Z
```

The output from these commands will provide you with a detailed overview of the cert-manager resources that are deployed in your AKS cluster. If all the resources are shown as "Running," "Ready," or "Available," it indicates that the cert-manager installation was successful. This verification step ensures that cert-manager is ready for use, and you can proceed with deploying applications in your AKS cluster while enjoying the benefits of automatic certificate management.



## Step-5: Create clusterissuer YAML file

The purpose of a ClusterIssuer in AKS (Azure Kubernetes Service) is to manage the issuance and renewal of TLS certificates for an AKS cluster. It defines the certificate authority that will be used to issue and sign the certificates, such as Let's Encrypt, and it can be used to configure additional properties such as the email address for certificate expiration notifications. By using a ClusterIssuer, you can automate the process of obtaining and renewing certificates for your AKS cluster, ensuring that your applications continue to use valid certificates without manual intervention.

``` yaml title="clusterissuer.yaml"
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: anjkeesari@gmail.com
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
          podTemplate:
            spec:
              nodeSelector:
                "kubernetes.io/os": linux

#kubectl apply -f cert-manager/clusterissuer-nginx.yaml -n sample
```

loading cluster issuer file using terraform

``` tf title="cert_manager.tf"
locals {
  clusterissuer = "cert-manager/clusterissuer-nginx.yaml"
}

# Create clusterissuer for nginx YAML file
data "kubectl_file_documents" "clusterissuer" {
  content = file(local.clusterissuer)
}
```

Create ClusterIssuer using terraform

``` tf title="cert_manager.tf"
# Apply clusterissuer for nginx YAML file
resource "kubectl_manifest" "clusterissuer" {
  for_each  = data.kubectl_file_documents.clusterissuer.manifests
  yaml_body = each.value
  depends_on = [
    data.kubectl_file_documents.clusterissuer
  ]
}
```
**Testing**

describe clusterissuer 

```
kubectl describe clusterissuer letsencrypt
```

Output

``` sh
Name:         letsencrypt
Namespace:
Labels:       <none>
Annotations:  <none>
API Version:  cert-manager.io/v1
Kind:         ClusterIssuer
Metadata:
 .
 .
 .
Status:
  Acme:
    Last Private Key Hash:  XyQWbNfKPyMk77tcC5HPrjudqf/Fnl7YbHCPqniQz0Y=
    Last Registered Email:  anjkeesari@gmail.com
    Uri:                    https://acme-v02.api.letsencrypt.org/acme/acct/1278195356
  Conditions:
    Last Transition Time:  2023-08-27T19:25:54Z
    Message:               The ACME account was registered with the ACME server
    Observed Generation:   1
    Reason:                ACMEAccountRegistered
    Status:                True
    Type:                  Ready
Events:                    <none>
```


## Step-6: Test the application with https URLs

In this step, we'll test our application using HTTPS URLs to ensure that the certificate management provided by cert-manager and Let's Encrypt is functioning correctly. Follow these steps to test the application:

Use the `deployment.yaml` file for your aspnet-api Microservice deployment. Apply it to the 'sample' namespace:

``` yaml title="deployment.yaml"
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
          image: acr1dev.azurecr.io/sample/aspnet-api:20230226.1
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP

# kubectl apply -f deployment.yaml -n sample
```

Modify the `service.yaml` file for the aspnet-api Microservice by changing the `ClusterIP` to `LoadBalancer`. Apply the changes to the 'sample' namespace:



``` yaml title="service.yaml"
apiVersion: v1
kind: Service
metadata:
  name: aspnet-api
  namespace: sample
  labels: {}
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
      name: http
  selector: 
    app: aspnet-api

# kubectl apply -f service.yaml -n sample
```
After applying the modified service.yaml file, you'll be able to access the Swagger API URL from the internet. Currently, this URL is not secure (HTTP). We'll make this URL secure (HTTPS) using cert-manager and Let's Encrypt.

![Alt text](images/image-8.png)


!!! Note

    I'm skipping the DNS configuration in this lab, assuming you've already configured the custom domain in your DNS settings, and it's ready to be used for this exercise.
    
Add the following annotations to the ingress.yaml file for the sample Ingress resource:

**annotations**

```
  cert-manager.io/cluster-issuer: letsencrypt
  cert-manager.io/acme-challenge-type: http01
```
**spec->tls**

```
  tls:
  - hosts:
    - dev1.tenant1.assetmark.net
    secretName: tls-secret
```

Here is the complete ingress YAML file

``` yaml title="ingress.yaml"
# This ingress used for api calls.
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: sample
  annotations:
    kubernetes.io/ingress.class: nginx
    # appgw.ingress.kubernetes.io/backend-path-prefix: "/"
    cert-manager.io/cluster-issuer: letsencrypt
    appgw.ingress.kubernetes.io/ssl-redirect: "true"
    cert-manager.io/acme-challenge-type: http01
    appgw.ingress.kubernetes.io/health-probe-status-codes: "200-499"
spec:
  tls:
  - hosts:
    - dev.k8s.anjikeesari.com
    secretName: tls-secret
  rules:
    - host: dev.k8s.anjikeesari.com
      http:
        paths:
          - path: /aspnet/api/* # aspnet svc rest api calls
            pathType: Prefix
            backend:
              service:
                name: aspnet-api
                port:
                  number: 80
# status:
#   loadBalancer:
#     ingress:
#       - ip: 20.221.242.197
# kubectl apply -f ingress.yaml -n sample
```

Test the application using custom domain URLs with the HTTPS protocol. Access the following URL in your browser:

https://dev.k8s.anjikeesari.com/swagger/index.html

You'll notice that the certificate used is provided by Let's Encrypt. This indicates that the cert-manager and Let's Encrypt integration is working correctly.

By following these steps, you've successfully configured your application to use HTTPS URLs and demonstrated that the certificate issuance and management process with cert-manager and Let's Encrypt is functioning as intended.


![Alt text](images\image-9.png)


## Reference:

Here is a list of resources that were used as references during the development of this technical scenario:

1. [Cert-Manager Documentation - Installing with Helm](https://cert-manager.io/docs/installation/helm/#option-2-install-crds-as-part-of-the-helm-release)
2. [Artifact Hub - Cert-Manager Helm Chart](https://artifacthub.io/packages/helm/cert-manager/cert-manager)
3. [Azure AKS Ingress TLS Documentation](https://learn.microsoft.com/en-us/azure/aks/ingress-tls?tabs=azure-cli)
4. [Setup Let's Encrypt ClusterIssuer with Terraform](https://stackoverflow.com/questions/68511476/setup-letsencrypt-clusterissuer-with-terraform)
