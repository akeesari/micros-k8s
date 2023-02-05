One of my goal is to simplify the development, deployment, and scaling of complex applications and to bring the full power of Kubernetes to all projects. DevOps Pipeline provides a platform which allows all projects to develop, deploy and scale container-based applications, highly productive, yet flexible environment for developers; One of the key tools we use from the Kubernetes ecosystem is Helm.

Helm is the package manager for Azure Kubernetes Cluster (AKS) deployments. Helm uses a packaging format called charts. A chart is a collection of manifest files that describe a related set of AKS Cluster Kubernetes resources.


Helm charts deployment make perfect sense for the complex workload deployments.

## key features

- ​Templates: - allows easily customize deployments for different environments.​

- Dependencies: making it easy to manage and deploy complex, multi-tier applications​

- Versioning: makes it easy to roll back to a previous version.​

- Declarative Deployments: define the desired state of a deployment.​

- Rollbacks: allow to rollback deployments to the previous version.​

- Namespaces: enables easy manage resource allocation and security within the cluster. This is very helpful for multitenant systems ​

## How it works?

![image.png](images/image-1.png)

A chart is organized as a collection of files inside of a directory called `Helm-Charts`.  Created separate folder for each deployment or application in side the templates folder and organized files for each resource separately.

A chart typically includes the following files:

- Chart.yaml: contains metadata about the chart, such as its name, version, and description.
- values.yaml: contains default configuration values for the chart.
- templates directory: contains Kubernetes manifests (in YAML format) for the resources that the chart will create.
- charts directory: contains any dependent charts that the chart requires.

When you run the `helm install` command, it installs the chart in the Kubernetes cluster by creating resources defined in the templates, based on the configuration values specified in the `values.yaml` file. You can also pass additional values to the command line, which will override the defaults from the `values.yaml` file.

For example:

- application
    - deployment.yaml
    - service.yaml
    - kustomization.yaml
    - secretes.yaml
    - ingress.yaml
    - configmap.yaml


<!-- 
## Install helm 

Pick the command as per your os version:

more information - https://helm.sh/docs/intro/install/

    # https://community.chocolatey.org/packages?q=helm

    choco install kubernetes-helm

## Verify the Installation

    helm version

Start with basic concepts from the first few vides from References:

artifacthub.io - use this website for required charts

https://artifacthub.io/packages/search?kind=0

## Update list of Helm charts from repositories

    helm repo update
    
## Searching Helm Charts
List all installed charts

    helm search

Search for a chart

    helm search foo

## Showing Installed Helm Charts
List all installed Helm charts

    helm ls

List all deleted Helm charts

    helm ls --deleted

List installed and deleted Helm charts

    helm ls --all

## Installing/Deleting Helm charts
Inspect the variables in a chart

    helm inspect values stable/mysql

Install a Helm chart

To install a new package, use the helm install command. At its simplest, it takes two arguments: 
A release name that you pick, 
and the name of the chart you want to install.

    helm install happy-panda bitnami/wordpress

    helm install --name foo stable/mysql
    helm install --name path/to/foo
    helm install --name foo bar-1.2.3.tgz
    helm install --name foo https://example.com/charts/bar-1.2.3.tgz

Install a Helm chart and override variables

    helm install --name foo --values config.yaml --timeout 300 --wait stable/mysql

Show status of Helm chart being installed

    helm status foo

## Delete a Helm chart

helm delete --purge foo
## Upgrading Helm Charts
Return the variables for a release

    helm get values foo

Upgrade the chart or variables in a release

    helm upgrade --values config.yaml foo stable/mysql

List release numbers

    helm history foo

Rollback to a previous release number

    helm rollback foo 1

## Creating Helm Charts
Create a blank chart

    helm create foo

Lint the chart

    helm lint foo

Package the chart into foo.tgz

    helm package foo

Install chart dependencies

    helm dependency update

## Release:
Uninstall a release

    helm uninstall [release]
 
 upgrade
 
    helm upgrade [release] [chart]

nstruct Helm to rollback changes if the upgrade fails:

    helm upgrade [release] [chart] --atomic

Upgrade a release. If it does not exist on the system, install it:

    helm upgrade [release] [chart] --install

Upgrade to a specified version:

    helm upgrade [release] [chart] --version [version-number]

Roll back a release:

    helm rollback [release] [revision]

Download Release Information
The helm get command lets you download information about a release.

## Download all the release information:

    helm get all [release]

Download all hooks:

    helm get hooks [release]

Download the manifest:

    helm get manifest [release]

Download the notes:

    helm get notes [release]

Download the values file:

    helm get values [release]

Fetch release history:

    helm history [release] 

## List and Search Repositories
Use the helm repo and helm search commands to list and search Helm repositories. helm search also enables you to find apps and repositories in Helm Hub.

List chart repositories:

    helm repo list

Generate an index file containing charts found in the current directory:

    helm repo index

Search charts for a keyword:

    helm search [keyword]

Search repositories for a keyword:

    helm search repo [keyword]

Search Helm Hub:

    helm search hub [keyword]

## Release Monitoring
The helm list command enables listing releases in a Kubernetes cluster according to several criteria, including using regular (Pearl compatible) expressions to filter results. Commands such as helm status and helm history provide more details about releases.

List all available releases in the current namespace:

    helm list

List all available releases across all namespaces:

    helm list --all-namespaces

List all releases in a specific namespace:

    helm list --namespace [namespace]

List all releases in a specific output format:

    helm list --output [format]

Apply a filter to the list of releases using regular expressions:

    helm list --filter '[expression]'

See the status of a specific release:

    helm status [release]

Display the release history:

    helm history [release]

See information about the Helm client environment:

    helm env

## Delete Helm Deployment
Deleting a Helm deployment removes the components without deleting the namespace.
reference: https://phoenixnap.com/kb/helm-delete-deployment-namespace

## List Helm Deployments
List Helm deployments in the current namespace with:

    helm list

To list deployments in a specific namespace, use:

    helm list --namespace <namespace_name>

List all Helm deployments in all namespaces by running:

    helm list --all-namespaces

## Delete Helm Deployment
To remove an installed Helm deployment, run:

    helm uninstall <deployment name> --namespace <namespace_name>
    helm uninstall sample-apps --namespace tenant1

Alternatively, use the alias:

    helm delete <deployment name> --namespace <namespace_name>

The terminal outputs a confirmation of removal. For example, the command below removes a deployment named phoenix-chart on the namespace other:

    helm uninstall phoenix-chart --namespace other



## To uninstall a release, use:

    helm uninstall <release-name> -n <namespace>
    You can also use --no-hooks to skip running hooks for the command:
    $ helm uninstall <release-name> -n <namespace> --no-hooks
   
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