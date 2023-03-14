## Introduction

In our previous labs we've already created deployment, service and ingress YAML manifest files for all of our Microservice and applied those manifests using kubectl commands, this is not ideal way to deploy applications to AKS, As part of this lab we are going to use those YAML manifest files and create Helm chart out of those files for AKS deployment so that we are deploying as package instead of individual manifest files.


## Technical Scenario

As a DevOps Engineer, you've been asked to create a Helm chart for Microservices Architecture so that you can deploy Microservices applications to Azure Kubernetes Service (AKS) cluster in a consistent and repeatable manner.

## Prerequisites

- **A Kubernetes cluster:** You will need a Kubernetes cluster to deploy your Helm chart to. 
- **Helm installed:** You will need to have Helm installed on your local machine. 
- **Azure CLI installed:** You will need to have the Azure CLI installed on your local machine. 
- **A basic understanding of Kubernetes:** You should have a basic understanding of Kubernetes concepts such as Pods, Deployments, Services, ConfigMaps, and Secrets.
- **An application to deploy:** You will need an application that you want to deploy to your AKS cluster. This could be a containerized application, a set of microservices, or any other application that can run on Kubernetes.
- **Azure Container Registry (ACR)** for storing the application image: You will need to have an Azure Container Registry (ACR) where you can store the container image for your application.

  
## Implementation details

In this exercise we will accomplish & learn how to implement following:

- Step-1: Install Helm
- Step-2: Create a new Helm chart
- Step-3: Validate Helm chart
- Task-3: Create template from Helm chart
- Task-4: Install Helm chart to AKS


## Step-1: Install Helm

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

## Task-2: Create a new Helm chart

To create a new chart, you can use the `helm create` command. to create a chart named `microservices-chart`, you can run the following command:

```
helm create microservices-chart
```

IMAGE

We don’t need hpa, ingress, serviceaccount, etc. I am going to delete all these files which we are not going to use and keep following files which are need to install the helm.

```
microservices-chart/
  Chart.yaml          # Contains the chart's metadata
  values.yaml         # Contains default values for the chart
  templates/          # Contains Kubernetes manifests
    deployment.yaml   # Defines a Kubernetes deployment
    service.yaml      # Defines a Kubernetes service

```

- **Chart.yaml:** This file contains the chart's metadata, including its name, version, and description. It also includes information about the chart's dependencies, maintainers, and other relevant details.
- **values.yaml:** This file contains default values for the chart's templates. These values can be overridden when the chart is installed using the --set or --values flags.
- **templates/:** This directory contains the Kubernetes manifests that make up the chart. Each file defines a different Kubernetes resource, such as a deployment, service, or ConfigMap.
- **deployment.yaml:** This file defines a Kubernetes deployment that runs the application. It includes information about the container image to use, the number of replicas to run, and other relevant details.
- **service.yaml:** This file defines a Kubernetes service that exposes the application to the network. It includes information about the type of service (ClusterIP, NodePort, etc.), the port to use, and other relevant details.

Together, these components make up a Helm chart that can be used to deploy an application to a Kubernetes cluster. When the chart is installed, Helm will use the templates in the templates/ directory to generate Kubernetes manifests that define the resources needed to run the application.

Let's take a look inside each files for more details:

**Chart.yaml**

Chart.yaml contains the information about the Chart such as name, version, description, etc.

```
Chart.yaml goes here
```


**values.yaml**


Using the `values.yaml` file for each environment allows you to define different configurations for your application based on the environment it's deployed in. Here's an example of how you can use the values.yaml file for each environment:

Create a separate values.yaml file for each environment you want to deploy your application to, for example 

- values.dev.yaml
- values.test.yaml
- values.staging.yaml
- values.prod.yaml


By using a separate `values.yaml` file for each environment, you can easily manage configuration for multiple environment and use the same manifest yaml or helm chart.

```
values.yaml file code
```


**passing values to yaml manifest**

You can pass these values in the deployment and service manifest files as below.

Microservice-1 manifest files 

```
deployment yaml 
```

```
service yaml 
```
Microfrontend-1 manifest files 

```
deployment yaml 
```

```
service yaml 
```


## Helm Lint - 

`Helm lint` is like your C# source code compilation. It is always recommended to run `helm lint` before installing helm chart, `Helm Lint` is a tool that checks the syntax and structure of a Helm chart and ensures that it follows best practices and conventions

Helm Lint helps you catch errors and issues with your Helm charts before you deploy them to a Kubernetes cluster, which can save you time and avoid potential problems down the line.

When you run helm lint on a Helm chart, it checks for the following:

- Valid YAML syntax
- Valid Helm chart structure
- Properly formatted values files
- Best practices for Helm chart development, such as using templates, labels, and annotations correctly

Helm Lint can be run on a Helm chart directory like this:

```
helm lint microservices-chart
```

If Helm Lint finds any issues with the Helm chart, it will output them to the console along with suggestions for how to fix them.

## Installing a Local Helm Chart

To install a local Helm chart, you can use the `helm install` command followed by the path to the chart directory. Here are the steps to follow:
- Navigate to the directory containing your Helm chart.
- Run the helm install command and specify a release name for the chart. For example:
```
helm install microservices-chart ./microservices-chart
```
This command installs the chart from the current directory (./microservices-chart) and gives it a release name of `microservices-chart`
-If you want to specify values for the chart, you can use the --values or -f flag followed by the path to a values.yaml file. For example:
```
helm install mychart ./mychart --values ./myvalues.yaml
```
After running the helm install command, Helm will deploy the chart to your Kubernetes cluster and create a new release. You can verify that the installation was successful by running helm ls, which will show a list of installed releases.

When you run the `helm install` command to install a Helm chart, the new release is placed in your Kubernetes cluster. The release is installed as a set of Kubernetes resources, which can include Deployments, Services, ConfigMaps, and more, depending on the contents of your Helm chart.

By default, Helm installs the release in the `default` namespace of your Kubernetes cluster. However, you can specify a different namespace by using the --namespace flag followed by the name of the namespace. For example:

```
helm install mychart ./mychart --namespace mynamespace
```
To view the Kubernetes resources that were created as part of the release, you can use the kubectl get command. For example, to see all the Deployments in the release, you can run:
```
kubectl get deployments -l app=mychart
```
This command lists all the Deployments with the app=mychart label, which is automatically applied to all resources in the release.

**--dry-run**

The `--dry-run` flag is used to perform a simulated installation of a chart using the helm install command without actually installing the chart or creating a new release. When this flag is used, Helm will generate the Kubernetes manifest files that would be applied to the cluster during the installation process, but it will not actually make any changes to the cluster.

The purpose of performing a dry run is to preview the changes that would be made to the cluster by the installation process before actually applying those changes. This can be useful for testing and debugging purposes, as well as for verifying that the desired configuration settings are being applied correctly.

By using the --dry-run flag, you can identify any potential issues or conflicts that may arise during the installation process before making any changes to the cluster. This can help you to avoid making unintended changes or causing downtime for your applications.

```
helm install mychart ./mychart --dry-run  
```

## Packaging the Helm chart

Use the helm package command to create the chart package. This command takes the name of the chart directory as its argument and creates a compressed archive file with the extension .tgz.

```
helm package mychart/
```
This will create a file named mychart-<version>.tgz in the current directory.

You can use the --version flag to specify the version number for the chart package. 
```
helm package --version 1.0.0 mychart/

```

Once you have created a Helm chart package, you can share it with others by hosting it in a public or private chart repository. Other users can then install and manage your application or service in their Kubernetes clusters using the helm install command and the name of your chart package.

## Upgrade helm chart

Run the `helm upgrade` command followed by the `release name` and `chart name`. For example, to upgrade a release named my-release with a chart named my-chart, run the following command:

```
helm upgrade my-release my-chart/
```

You can specify the namespace in which the release is deployed by using the --namespace flag. For example, to upgrade a release named my-release in the my-namespace namespace with a chart named my-chart, run the following command:

```
helm upgrade my-release my-chart/ --namespace my-namespace
```
After the upgrade is complete, check the status of the release using the helm status command to ensure that it has been updated successfully.
lua

```
helm status my-release
```

Note: If you encounter any issues during the upgrade process, you can roll back the release to the previous version using the helm rollback command. For example, to roll back the my-release release to version 1, run the following command:

```
helm rollback my-release 1

```
This will revert the release to the previous version and restore the previous configuration.

## helm chart history

