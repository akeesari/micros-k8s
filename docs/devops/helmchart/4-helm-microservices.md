## Introduction

As part of [Create your first Helm chart](2-create-helm.md) Lab we've already created a basic helm chart, in this lab we are going to extend the same helm chart and make it for Microservices architecture, that means we are going to create helm chart which runs for multiple applications as a single package manager.  

## Technical Scenario

As a DevOps Engineer, you've been asked to create a Helm chart for Microservices Architecture so that you can deploy Microservices applications to Azure Kubernetes Service (AKS) cluster in a consistent and repeatable manner.

## Prerequisites

- **A Kubernetes cluster:** You will need a Kubernetes cluster to deploy your Helm chart to. 
- **Basic helm chart:**  We've already created this in our previous lab
- **Microservice applications yaml manifests:** You will need Microservice applications YAML manifests that you want to deploy to your AKS cluster. These Microservices are containerized and ready AKS deployment.
- **Azure Container Registry (ACR)** for storing the application images: You will need to have an Azure Container Registry (ACR) where you can store the container images for your Microservice applications.

  
## Implementation details

In this exercise we will accomplish & learn how to implement following:

- Task-1: Update basic Helm chart for Microservices
- Task-2: Validate Helm chart
- Task-3: Install Helm chart in AKS
- Task-4: UnInstall Helm chart

## Task-1: Update basic helm chart for Microservices

Before updating the existing Helm chart, let's review the tree structure of our files & folders from previous lab.

```
C:
│   .helmignore
│   Chart.yaml
│   values.yaml
├───charts
└───templates
        deployment.yaml
        service.yaml
```

Let's discuss about the new changes in each file in the existing Helm chart to support Microservice applications deployment. 

**.helmignore** - no changes in this file

**Chart.yaml** - no changes in this file

**values.yaml:** 

One of the key features of Helm Chart is the ability to use `values.yaml` files to parameterize your deployment configuration. as we know that `values.yaml` contains the default values for the chart's templates, These values can be overridden when the chart is installed.

Now let's consider the real scenario and use the `values.yaml` for multiple environments. Here we want to install same helm chart for multiple environments; in this case we need to maintain separate `values.yaml` file for each environment; Here is how you can create separate values.yaml file for each environment.


``` sh
# file naming convention 
values-{environment}.yaml
```

For example 

```
- values-dev.yaml  - use this for dev environment
- values-test.yaml  - use this for qa/test environment
- values-uat.yaml  - use this for uat/staging environment
- values-prod.yaml  - use this for production environment
```

By using a separate `values.yaml` file for each environment, you can easily manage configuration for multiple environment and use the same manifest yaml or helm chart for each environment.

Let's take a look inside the each values.yaml files 

dev environment
``` yaml title="values-dev.yaml"
global:
  environment: dev
  namespace: sample
  registryPrefix: acr1dev.azurecr.io

microservices:
  aspnetapi:
    replicaCount: 1    
  aspnetapp:
    replicaCount: 1
```
test environment

``` yaml title="values-test.yaml"
global:
  environment: test
  namespace: sample
  registryPrefix: acr1test.azurecr.io

microservices:
  aspnetapi:
    replicaCount: 2  
  aspnetapp:
    replicaCount: 2
```
prod environment

``` yaml title="values-prod.yaml"
global:
  environment: prod
  namespace: sample
  registryPrefix: acr1prod.azurecr.io

microservices:
  aspnetapi:
    replicaCount: 3
  aspnetapp:
    replicaCount: 3
```

if you notice each files has different configuration as per the specific environment need.

**templates/:** 

As we know this directory contains the Kubernetes manifests, we are going to create separate folder for each application inside this template folder and create the deployment, service, or ConfigMap manifest files for specific to single application. 

Here is how the folder structure for single application inside the template folder

``` sh
template:
└───aspnet-app
    |__ deployment.yaml
    |__ service.yaml
    |__ ConfigMap.yaml - # create only if it is required
   aspnet-api
    |__ deployment.yaml
    |__ service.yaml
    aspnet-db
    |__ deployment.yaml
    |__ service.yaml
```

Now let's look at some of our Microservices manifest yaml files created in our previous labs and package them in the template folder.

**aspnet-api** - create this new folder inside template folder

`deployment.yaml` - create this new file inside the `aspnet-api` folder and copy this yaml manifest.

``` yaml title="deployment.yaml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aspnet-api
  namespace: {{ .Values.global.namespace }}
spec:
  replicas: {{ .Values.microservices.aspnetapi.replicaCount }}
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
          image: {{ .Values.global.registryPrefix }}/sample/aspnet-api:20230302.5
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          env:
            - name: KEY
              value: value
```

`service.yaml` - create this new file inside the `aspnet-api` folder and copy this yaml manifest.


``` yaml title="service.yaml"
apiVersion: v1
kind: Service
metadata:
  name: aspnet-api
  namespace: {{ .Values.global.namespace }}
  labels: {}
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
      name: http
  selector: 
    app: aspnet-api

```

repeat above step for each service, Here is the example for aspnet-app.

**aspnet-app** - create this new folder inside template folder

`deployment.yaml` - create this new file inside the `aspnet-app` folder and copy this yaml manifest.

``` yaml title="deployment.yaml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: aspnet-app
  namespace: {{ .Values.global.namespace }}
spec:
  replicas: {{ .Values.microservices.aspnetapp.replicaCount }}
  selector: 
    matchLabels:
      app: aspnet-app
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5 
  template:
    metadata:
      labels:
        app: aspnet-app
    spec:
      nodeSelector:
        "kubernetes.io/os": linux
      serviceAccountName: default
      containers:
        - name: aspnet-app-image
          image: {{ .Values.global.registryPrefix }}/sample/aspnet-app:20230302.5
          imagePullPolicy: Always
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          env:
            - name: KEY
              value: value
```

`service.yaml` - create this new file inside the `aspnet-app` folder and copy this yaml manifest.


``` yaml title="service.yaml"
apiVersion: v1
kind: Service
metadata:
  name: aspnet-app
  namespace: {{ .Values.global.namespace }}
  labels: {}
spec:
  type: ClusterIP
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
      name: http
  selector: 
    app: aspnet-app

```

We'll stop here with 2 applications but same goes for the rest of the containers. Now let's talk about the ingress files.

**Ingress** - Create a new folder called `ingress`, we are going to create separate folder for managing all ingress files used in our microservices applications.

We are going to create separate ingress file for each scenario, for example 

`ingress-no-pathprefix.yaml` - This file contains the ingress routing rules to support the API swagger URL rules


``` yaml title="ingress-no-pathprefix.yaml"

replace yaml file here

```

`ingress-pathprefix.yaml` - This file contains the ingress routing rules to support the Web Application URL rules

``` yaml title="ingress-pathprefix.yaml"
replace yaml file here

```

Here is how the new tree structure looks like after adding few microservices YAML manifest files in template folder.

``` sh
C:.
│   .helmignore
│   Chart.yaml
│   values.yaml
├───charts
└───templates
    ├───aspnet-api
    │       deployment.yaml
    │       service.yaml
    ├───aspnet-app
    │       deployment.yaml
    │       service.yaml
    └───ingress
            ingress-no-pathprefix.yaml
            ingress-pathprefix.yaml
```

Together, these components make up a Helm chart that can be used to deploy an application to a Kubernetes cluster. When the chart is installed, Helm will use the templates in the templates/ directory to generate Kubernetes manifests that define the resources needed to run the application.

## Task-2: Validate Helm chart


Before we install this Helm chart, we are going to validate helm chart to make sure that no errors in the chart. the first command we are going to use is `helm lint`

**Helm Lint**

It is always recommended to run `helm lint` before installing helm chart, `Helm Lint` is a tool that checks the syntax and structure of a Helm chart and ensures that it follows best practices and conventions

``` sh
helm lint microservices-chart
```
example of the output without any errors

``` sh
==> Linting microservices-chart
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
```

**Helm template**

After the `helm lint`, it is also recommended to generate the yaml manifest from helm chart and review the yaml manifest to make sure all the values are replaced correctly or not, for that we are going to use `helm template` command.

`helm template` command does not install the chart or make any changes to the Kubernetes cluster. It simply generates YAML manifests based on the chart and the values provided. This can be useful for reviewing the manifests before applying them to a Kubernetes cluster.

``` sh
# generate with environment values.yaml file
helm template 'microservices-chart' './microservices-chart' --namespace='sample' --values 'microservices-chart/values-dev.yaml' > microservices-manifests.yaml
```

You will notice new file `microservices-manifests.yaml`

## Task-3: Install Helm chart in AKS

To install a local Helm chart, you can use the `helm install` command.

``` sh
helm install 'microservices-release' './microservices-chart' --namespace 'sample'
```

 - `microservices-release`  - release name for the chart.   
 - `./microservices-chart`  - directory where chart located.
 - `sample` - namespace of AKS

output

``` sh
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 17:10:36 2023
NAMESPACE: sample
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

This command installs the chart from the current directory (./microservices-chart) and gives it a release name of `microservices-release`


**verify the installation**

You can verify that the installation was successful by running helm ls, which will show a list of installed releases.

``` sh
helm ls -n sample
```
output

``` sh
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
microservices-release   sample          1               2023-03-14 17:10:36.1926652 -0700 PDT   deployed        microservices-chart-0.1.0       1.16.0
```

**helm status**

Check the status of the release using the `helm status` command to ensure that it has been installed successfully.

``` sh
helm status 'microservices-release' -n 'sample'
```
output

``` sh
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 17:10:36 2023
NAMESPACE: sample
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

## Step-4: Upgrade Helm chart

If you've any changes in the application code or configuration then you need to use the `helm upgrade` command to promote the new changes in the existing helm release.

Run the `helm upgrade` command followed by the `release name` and `chart name`.

``` sh
helm upgrade 'microservices-release' 'microservices-chart' --namespace 'sample'
```

output
``` sh
Release "microservices-release" has been upgraded. Happy Helming!
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 21:34:46 2023
NAMESPACE: sample
STATUS: deployed
REVISION: 2
TEST SUITE: None
```

**helm status**

After the upgrade is complete, check the status of the release using the helm status command to ensure that it has been updated successfully.

``` sh
helm status 'microservices-release' -n 'sample'
```
output
``` sh
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 21:34:46 2023
NAMESPACE: sample
STATUS: deployed
REVISION: 2
TEST SUITE: None
```

**release history**

Display the release history

``` sh
helm history 'microservices-release'
```
output
``` sh
REVISION        UPDATED                         STATUS          CHART                           APP VERSION     DESCRIPTION     
1               Tue Mar 14 17:10:36 2023        superseded      microservices-chart-0.1.0       1.16.0          Install complete
2               Tue Mar 14 21:34:46 2023        deployed        microservices-chart-0.1.0       1.16.0          Upgrade complete
```

## Step-5: UnInstall Helm chart

To uninstall a Helm chart, you can use the helm uninstall command followed by the name of the release you wish to delete. 

``` sh
# list helm release
helm ls -n 'sample'
helm uninstall 'microservices-release' -n 'sample'
```

output
``` sh
release "microservices-release" uninstalled
```

Overall, Helm simplifies the process of installing and managing Kubernetes microservices applications and provides an easy-to-use command-line interface for managing charts and releases.