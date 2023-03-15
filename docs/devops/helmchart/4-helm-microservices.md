## Introduction

In our previous labs we've already created basic helm chart, in this lab we are going to extend the same helm chart and make it for Microservices architecture that means we are going to create helm chart which runs for multiple applications as a single package.  

## Technical Scenario

As a DevOps Engineer, you've been asked to create a Helm chart for Microservices Architecture so that you can deploy Microservices applications to Azure Kubernetes Service (AKS) cluster in a consistent and repeatable manner.

## Prerequisites

- **A Kubernetes cluster:** You will need a Kubernetes cluster to deploy your Helm chart to. 
- **Basic helm chart:**  We've already created this in our previous lab
- **Microservice applications to deploy:** You will need Microservice applications that you want to deploy to your AKS cluster. This could be a containerized application, a set of microservices, or any other application that can run on Kubernetes.
- **Azure Container Registry (ACR)** for storing the application image: You will need to have an Azure Container Registry (ACR) where you can store the container image for your application.

  
## Implementation details

In this exercise we will accomplish & learn how to implement following:

- Task-1: Update basic Helm chart for Microservices
- Task-2: Validate Helm chart
- Task-3: Install Helm chart in AKS
- Task-4: UnInstall Helm chart

## Task-1: Update basic helm chart for Microservices

To create a new chart, you can use the `helm create` command. to create a chart named `microservices-chart`, you can run the following command:

Here is the tree structure of our files & folders from previous lab.

```
C:.
│   .helmignore
│   Chart.yaml
│   values.yaml
│
├───charts
└───templates
        deployment.yaml
        service.yaml
```

**values.yaml:** 

This file contains default values for the chart's templates. These values can be overridden when the chart is installed using the --set or --values flags.

Use the `values.yaml` file for default values of helm chart but if you want to install same helm chart for multiple environments we need to maintain separate values.yaml file for each environment; Here is how you can create separate values.yaml file for each environment.

```
values-{environment}.yaml
```

For example 

- values-dev.yaml  - use this for dev environment
- values-test.yaml  - use this for qa/test environment
- values-uat.yaml  - use this for uat/staging environment
- values-prod.yaml  - use this for production environment


By using a separate `values.yaml` file for each environment, you can easily manage configuration for multiple environment and use the same manifest yaml or helm chart for each environment.

Let's take a look inside the each values.yaml files 

dev environment
``` yaml title="values-dev.yaml"

replicaCount: 1

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
```
test environment

``` yaml title="values-dev.yaml"

replicaCount: 2

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
```
prod environment

``` yaml title="values-dev.yaml"

replicaCount: 3

image:
  repository: nginx
  pullPolicy: IfNotPresent
  tag: ""
```

if you notice each files has different configuration as per the specific environment need.

**templates/:** 

This directory contains the Kubernetes manifests that make up the chart. Each file defines a different Kubernetes resource, such as a deployment, service, or ConfigMap.

**deployment.yaml:** 

This file defines a Kubernetes deployment that runs the application. It includes information about the container image to use, the number of replicas to run, and other relevant details.

**service.yaml:** 

This file defines a Kubernetes service that exposes the application to the network. It includes information about the type of service (ClusterIP, NodePort, etc.), the port to use, and other relevant details.

Now let's look at some of our Microservices manifest yaml files created in our previous labs which we are going to include here in our helm chart and deploy as package together with helm install.

**aspnet-api** yaml manifest files 

deployment

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
service

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

**aspnet-app** yaml manifest files 

deployment 
```
deployment yaml
```

service
```
service yaml
```

ingress
```
service yaml 
```

Here is the new tree structure after adding few microservices yaml manifest files in template folder.

```
C:.
│   .helmignore
│   Chart.yaml
│   values.yaml
│
├───charts
└───templates
    ├───aspnet-api
    │       deployment.yaml
    │       service.yaml
    │
    ├───aspnet-app
    │       deployment.yaml
    │       service.yaml
    │
    └───ingress
            ingress-no-pathprefix.yaml
            ingress-pathprefix.yaml
```

Together, these components make up a Helm chart that can be used to deploy an application to a Kubernetes cluster. When the chart is installed, Helm will use the templates in the templates/ directory to generate Kubernetes manifests that define the resources needed to run the application.

## Task-2: Validate Helm chart

**Helm Lint**

```
helm lint microservices-chart
```

output
```
==> Linting microservices-chart
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
```

**Helm template**

This command will generate Kubernetes YAML manifests from the Helm chart

``` sh
# generate with environment values.yaml file
helm template 'microservices-chart' './microservices-chart' --namespace='sample' --values 'microservices-chart/values-dev.yaml' > microservices-manifests.yaml
```

You will notice new file `microservices-manifests.yaml`

## Task-3: Install Helm chart in AKS

```
helm install 'microservices-release' './microservices-chart' --namespace 'sample'
```
output

```
NAME: microservices-release
LAST DEPLOYED: Tue Mar 14 17:10:36 2023
NAMESPACE: sample
STATUS: deployed
REVISION: 1
TEST SUITE: None
```

```
helm ls -n sample
```
output

```
NAME                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                           APP VERSION
microservices-release   sample          1               2023-03-14 17:10:36.1926652 -0700 PDT   deployed        microservices-chart-0.1.0       1.16.0
```
## Task-4: UnInstall Helm chart

```
helm upgrade my-release my-chart/
```
