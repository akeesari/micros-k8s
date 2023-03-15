One of my goal is to simplify the development, deployment, and scaling of complex applications and to bring the full power of Kubernetes to all projects. 

One of the key tools we use from the Kubernetes ecosystem is `Helm.`

`Helm` is the package manager for Azure Kubernetes Cluster (AKS) deployments. Helm uses a packaging format called charts. A `chart` is a collection of manifest files that describe a related set of AKS Cluster Kubernetes resources.


Helm charts deployment make perfect sense for the complex workload deployments.


## Objective

In this exercise we will accomplish & learn following:

- What is Helm Chart?
- How helm chart works?
- When do we need to use helm chart?
- Key Features of Helm
- Explain Helm Architecture

## What is Helm Chart?

Helm is a package manager for Kubernetes that allows you to manage and install software in your cluster. A Helm chart is a package that contains all of the necessary configuration files and resources to deploy an application in a Kubernetes cluster.

A Helm chart includes information such as the resources to be deployed (e.g., pods, services, and ingress), the container images to be used, and the configuration values for the deployment. Charts can be stored in a central repository, making it easy to share and reuse applications and components across multiple clusters and teams.

Using Helm, you can install and manage complex applications with a single command, simplifying the process of deploying and managing applications in your cluster. Helm charts also make it easier to manage versioning and upgrades of your applications, as well as rollback to previous versions if necessary.

## When do we need to use helm chart?

You might need to use Helm charts when deploying and managing applications in a Kubernetes cluster. Some of the key reasons to use Helm charts include:

- **Simplified application deployment:** Helm charts make it easier to deploy and manage complex applications, as you can define the resources and configuration in a single package, and then deploy and upgrade the application with a single command.
- **Reusable components:** Helm charts can be stored in a central repository, making it easy to share and reuse components across multiple clusters and teams.
- **Versioning and upgrades:** Helm charts provide versioning and upgrade capabilities, making it easier to manage the lifecycle of your applications and rollback to previous versions if necessary.
- **Customization:** Helm charts provide a flexible and customizable way to manage deployments, as you can define variables and configuration values that can be modified during installation.
- **Dependency management:** Helm charts can manage dependencies between different components, making it easier to deploy and manage complex applications that consist of multiple components.
- **Scalability:** Helm charts are designed to be scalable, making it easy to manage deployments across multiple clusters and teams.

Overall, Helm charts are a powerful tool for automating and simplifying the process of deploying and managing applications in a Kubernetes cluster. If you need to deploy and manage applications in a large-scale or complex environment, using Helm charts might be a good option to consider.

## key features of Helm

- **​Templates**: - allows easily customize deployments for different environments.​

- **Dependencies**: making it easy to manage and deploy complex, multi-tier applications​

- **Versioning**: makes it easy to roll back to a previous version.​

- **Declarative Deployments**: define the desired state of a deployment.​

- **Rollbacks**: allow to rollback deployments to the previous version.​

- **Namespaces**: enables easy manage resource allocation and security within the cluster. This is very helpful for multitenant systems ​

## How it works?

![image.png](images/image-1.png)

A chart is organized as a collection of files inside of a directory called `Helm-Charts`.  Created separate folder for each deployment or application in side the templates folder and organized files for each resource separately.

A chart typically includes the following files:

- **Chart.yaml**: contains metadata about the chart, such as its name, version, and description.
- **values.yaml**: contains default configuration values for the chart.
- **templates directory**: contains Kubernetes manifests (in YAML format) for the resources that the chart will create.
- **charts directory**: contains any dependent charts that the chart requires.

When you run the `helm install` command, it installs the chart in the Kubernetes cluster by creating resources defined in the templates, based on the configuration values specified in the `values.yaml` file. You can also pass additional values to the command line, which will override the defaults from the `values.yaml` file.


Once the application is deployed, Helm monitors the state of the resources and can perform updates or rollbacks if necessary. You can also use the Helm CLI to upgrade or delete the application, as well as manage the dependencies between different charts.

In this way, Helm charts provide a flexible and scalable way to manage and deploy applications in your Kubernetes cluster, making it easier to automate and simplify the process of application management.

##  Explain Helm Architecture

The architecture of Helm consists of the following components:

- **Helm CLI**: The Helm CLI is a command-line tool that allows you to manage and install Helm charts. The Helm CLI communicates with the Helm Tiller server to deploy charts in your cluster.

- **Helm Tiller server**: The Helm Tiller server is a component that runs in your Kubernetes cluster and is responsible for managing the installation and lifecycle of Helm charts. The Tiller server receives requests from the Helm CLI and communicates with the Kubernetes API to create, update, and manage the resources defined in the chart.

- **Chart repository**: The chart repository is a centralized storage location for Helm charts. It can be a local or remote repository, and it provides a way to share and distribute charts across multiple teams and clusters.

- **Chart**: A Helm chart is a package that contains all of the necessary configuration files and resources to deploy an application in a Kubernetes cluster. Charts can be stored in a chart repository and can be installed in your cluster using the Helm CLI.

- **Template:** The templates in a Helm chart are written in YAML format and use the Go Templating Language (Golang) to define variables and control structures. When you install a chart, the Helm Tiller server evaluates the templates and generates the necessary Kubernetes resources based on the configuration values defined in the chart.

The architecture of Helm provides a simple and flexible way to manage and deploy applications in your Kubernetes cluster, making it easier to automate and simplify the process of application management. The Helm CLI provides a user-friendly interface for interacting with the Tiller server, while the Tiller server communicates with the Kubernetes API to manage the resources in the cluster. The chart repository provides a centralized storage location for charts, making it easy to share and distribute applications across multiple teams and clusters.


## References
- <https://helm.sh/docs/topics/charts/>

<!--
# Reference


Introduction to Helm | Kubernetes Tutorial | Beginners Guide - That DevOps Guy - Practical

- https://www.youtube.com/watch?v=5_J7RWLLVeQ 

What is Helm? | Helm Concepts Explained | KodeKloud, Theory 

- https://www.youtube.com/watch?v=kJscDZfHXrQ

video from Nana - Theory

- https://www.youtube.com/watch?v=-ykwb1d0DXU

Create Your First Helm Chart | Helm 3 for beginners - Practicals

- https://www.youtube.com/watch?v=eNqjoX20BH4

----------

Cheat-Sheet

- https://github.com/RehanSaeed/Helm-Cheat-Sheet
- https://phoenixnap.com/kb/helm-commands-cheat-sheet

Github - Source code, Microsevices samples; 

- https://github.com/microservices-demo/microservices-demo/tree/master/deploy/kubernetes/helm-chart

Getting Started with Helm Chart

- https://jhooq.com/getting-start-with-helm-chart/

Convert Kubernetes deployment YAML into Helm Chart YAML, these references might be helpful

- https://jhooq.com/convert-kubernetes-yaml-into-helm/
- https://www.youtube.com/watch?v=ZZVXXEyEzAs&t=29s
- https://phoenixnap.com/kb/helm-delete-deployment-namespace - helm delete

helm website
- https://helm.sh/docs/helm/

- https://www.visualstudiogeeks.com/devops/helm/deploying-helm-chart-with-azdo
- https://docs.microsoft.com/en-us/azure/container-registry/container-registry-helm-repos
- https://docs.microsoft.com/en-us/azure/aks/kubernetes-helm
- https://docs.microsoft.com/en-us/azure/aks/quickstart-helm?tabs=azure-cli
-----

deploying micro services application on kubernetes using helm charts
- https://www.youtube.com/watch?v=-dyxS2XD_ME


(1073) Helm 3 for beginners - YouTube
- https://www.youtube.com/playlist?list=PLLYW3zEOaqlKYku0piyzzLFGpR9VpPvXR

Package and Deploy Helm Charts task
- https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/deploy/helm-deploy?view=azure-devops

HELM Chart Deployment to Kubernetes using Azure DevOps CICD
- https://www.youtube.com/watch?v=NT_vMuzpXuY

Creating a Helm chart for an ASP.NET Core app

- https://andrewlock.net/deploying-asp-net-core-applications-to-kubernetes-part-4-creating-a-helm-chart-for-an-aspnetcore-app/
- https://github.com/saharsh-samples/dotnet-k8s-helm-cicd/blob/develop/deployment/helm-k8s/templates/deployment.yaml

Deploying Helm Charts with Azure DevOps
- https://www.youtube.com/watch?v=1bC-fZEFodU

Replace Helm Chart Variables in your CI/CD Pipeline with Tokenizer
- https://www.programmingwithwolfgang.com/replace-helm-variables-tokenizer/ -->