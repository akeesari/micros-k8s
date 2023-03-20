## Introduction

In our previous labs we've already created deployment, service and ingress YAML manifest files for all of our Microservice and applied some of those manifests using `kubectl` commands, this is not ideal way to deploy applications to kubernetes cluster especially for microservice architecture, As part of this lab we are going to use those YAML manifest files and create Helm chart for  AKS deployment so that we are deploying as package instead of individual manifest files. Helm charts are a package manager for Kubernetes that make it easier to deploy, manage, and upgrade applications on a Kubernetes cluster.


## Technical Scenario

As a DevOps Engineer, you've been asked to create a basic Helm chart so that you can deploy your applications to Azure Kubernetes Service (AKS) cluster in a consistent and repeatable manner.

## Prerequisites

- **A Kubernetes cluster:** You will need a Kubernetes cluster to deploy your Helm chart to. 
- **Helm installed:** You will need to have Helm installed on your local machine. 
- **Azure CLI installed:** You will need to have the Azure CLI installed on your local machine. 
- **A basic understanding of Kubernetes:** You should have a basic understanding of Kubernetes concepts such as Pods, Deployments, Services, ConfigMaps, and Secrets.
- **An application to deploy:** You will need an application that you want to deploy to your AKS cluster. This could be a containerized application, a set of microservices, or any other application that can run on Kubernetes.
- **Azure Container Registry (ACR)** for storing the application image: You will need to have an Azure Container Registry (ACR) where you can store the container image for your application.

  
## Implementation details

In this exercise we will accomplish & learn how to implement following:

- Step-1: Install Helm locally
- Step-2: Create a new Helm chart
- Step-3: Validate Helm chart
- Step-4: Install Helm chart in AKS
- Step-5: UnInstall Helm chart


## Step-1: Install Helm locally

You need to install Helm on your local machine. You can find the installation instructions in the Helm documentation: <https://helm.sh/docs/intro/install/>


**Windows OS users**

To install Helm on Windows using Chocolatey package manager, follow these steps:

- Install Chocolatey by following the instructions on the Chocolatey website: <https://chocolatey.org/install>
- Once Chocolatey is installed, open a Command Prompt or PowerShell window with administrator privileges.
- Run the following command to install Helm using Chocolatey:
```
choco install kubernetes-helm
```

**Mac OS users**

Install Helm using Homebrew package manager by running the following command in a terminal window:

```
brew install helm
```

**Verify the Helm installation**


After the installation is complete, run the `helm version` command to verify that Helm is installed and working properly.

```
helm version
```

your output will look like below:
```
version.BuildInfo{Version:"v3.8.2", GitCommit:"6e3701edea09e5d55a8ca2aae03a68917630e91b", GitTreeState:"clean", GoVersion:"go1.17.5"}
```

## Step-2: Create a new Helm chart

To create a new chart, you can use the `helm create` command. to create a chart named `microservices-chart`, you can run the following command:

```
helm create microservices-chart
```

Let's quickly view the list of files and folders created here by running following command in command prompt.

```
tree /F
```
output

```
C:.
│   .helmignore
│   Chart.yaml
│   values.yaml
├───charts
└───templates
    │   deployment.yaml
    │   hpa.yaml
    │   ingress.yaml
    │   NOTES.txt
    │   service.yaml
    │   serviceaccount.yaml
    │   _helpers.tpl
    └───tests
            test-connection.yaml
```

We don’t need all the files here, let me cleanup some files which we are not going use it immediately and keep only files we need it for now to continue our lab work.

Here is the new tree structure of our files & folders after the cleanup

```
C:.
│   .helmignore
│   Chart.yaml
│   values.yaml
├───charts
└───templates
        deployment.yaml
        service.yaml
```

Now let's discuss the purpose of each file and folders used here and update with new contents.

**.helmignore:** 

The `.helmignore` file is used by the Helm package manager to specify files and directories that should be excluded from a chart when it is packaged. The syntax of the .helmignore file is similar to that of a .gitignore file. 

We will continue using this file without any changes.

``` yaml title=".helmignore"
# Patterns to ignore when building packages.
# This supports shell glob matching, relative path matching, and
# negation (prefixed with !). Only one pattern per line.
.DS_Store
# Common VCS dirs
.git/
.gitignore
.bzr/
.bzrignore
.hg/
.hgignore
.svn/
# Common backup files
*.swp
*.bak
*.tmp
*.orig
*~
# Various IDEs
.project
.idea/
*.tmproj
.vscode/

```

**Chart.yaml:** 

This file contains the chart's metadata, including its name, version, and description. Again we don't need to make any changes here for now.

``` yaml title="Chart.yaml"
apiVersion: v2
name: microservices-chart
description: A Helm chart for Kubernetes
.
.
access the the git repo for entire source code ...
```

**values.yaml:** 

This file contains default values for the chart's templates. These values can be overridden when the chart is installed using the --set or --values flags.

Let's replace the contents of values.yaml file with following:

``` yaml title="values.yaml"
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

**templates/:** 

This directory contains the Kubernetes manifests that make up the chart. Each file defines a different Kubernetes resource, such as a deployment, service, or ConfigMap.

**deployment.yaml:** 

This file defines a Kubernetes deployment that runs the application. It includes information about the container image to use, the number of replicas to run, and other relevant details.

Let's replace the `deployment.yaml` file contents with one of our Microservice deployment.yaml file and make few changes here to read values from values.yaml file. 

If you notice we've reading the namespace, replicas & image values from values.yaml files; that's how helm syntax works.

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



**service.yaml:** 

This file defines a Kubernetes service that exposes the application to the network. It includes information about the type of service (ClusterIP, NodePort, etc.), the port to use, and other relevant details.

Let's replace the `service.yaml` file contents with one of our microservices service.yaml file and make few changes here to read values from values.yaml file. 

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
That's it! Together, these components make up a Helm chart that can be used to deploy an application to a Kubernetes cluster. When the chart is installed, Helm will use the templates in the templates/ directory to generate Kubernetes manifests that define the resources needed to run the application.

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
helm template 'microservices-chart' './microservices-chart' --namespace='sample' > microservices-manifests.yaml
```

!!! Note
    you will notice a new file `microservices-manifests.yaml` created in your folder for your review, after the reveiw you can delete this file so that you don't commit this file in your git repo.


## Step-3: Installing Helm Chart

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

When you run the `helm install` command to install a Helm chart, the new release is placed in your Kubernetes cluster. The release is installed as a set of Kubernetes resources, which can include Deployments, Services, ConfigMaps, and more, depending on the contents of your Helm chart.

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

Overall, Helm simplifies the process of installing and managing Kubernetes applications and provides an easy-to-use command-line interface for managing charts and releases.