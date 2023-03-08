
## Introduction

Helm is a Kubernetes package manager for deploying helm charts.

This page contains a list of commonly used helm commands.

Note: make sure that you login into azure and connect to AKS cluster before running these helm commands

## Azure login

``` sh
az login
az account list --output table
```
## Select the subscription

``` sh
az account set -s "anji.keesari"
az account show --output table
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

## Available Commands

```
completion  generate autocompletion scripts for the specified shell
create      create a new chart with the given name
dependency  manage a chart's dependencies
env         helm client environment information
get         download extended information of a named release
help        Help about any command
history     fetch release history
install     install a chart
lint        examine a chart for possible issues
list        list releases
package     package a chart directory into a chart archive
plugin      install, list, or uninstall Helm plugins
pull        download a chart from a repository and (optionally) unpack it in local directory
push        push a chart to remote
registry    login to or logout from a registry
repo        add, list, remove, update, and index chart repositories
rollback    roll back a release to a previous revision
search      search for a keyword in charts
show        show information of a chart
status      display the status of the named release
template    locally render templates
test        run tests for a release
uninstall   uninstall a release
upgrade     upgrade a release
verify      verify that a chart at the given path has been signed and is valid
version     print the client version information
```

## Version

See the installed version of Helm

```
helm version
```

## Helm --help

Display the general help output for Helm

```
helm --help
helm [command] --help 
helm repo --help
helm repo list --help
```


## Add a repository 

Add a repository from the internet

```
helm repo add [repository-name] [url]
helm repo add bitnami https://charts.bitnami.com/bitnami
```
## Remove a repository 

Remove a repository from your system:

```
helm repo remove [repository-name]
helm repo remove bitnami
```

output
```
"bitnami" has been removed from your repositories
```

## List Repositories

helm repo list

```
helm repo list
```
output

```
NAME                    URL
bitnami                 https://charts.bitnami.com/bitnami
runix                   https://helm.runix.net
ingress-nginx           https://kubernetes.github.io/ingress-nginx
jetstack                https://charts.jetstack.io
prometheus-community    https://prometheus-community.github.io/helm-charts
apache-solr             https://solr.apache.org/charts
azure-marketplace       https://marketplace.azurecr.io/helm/v1/repo
bitnami-azure           https://marketplace.azurecr.io/helm/v1/repo
```

## Update repositories

```
helm repo update
```

## Search helm repo

search command help

```
helm search helm
```

Search repositories for a keyword:
helm search repo searches the repositories that you have added to your local helm client (with helm repo add)

```
helm search repo [repo keyword]
helm search repo bitnami
helm search repo azure-marketplace
helm search repo wordpress
```

## Search Helm Hub
helm search hub searches the Artifact Hub

```
helm search hub [hub keyword]
helm search hub hub
helm search hub bitnami
helm search hub microsoft
helm search hub wordpress 
helm search hub prometheus

```

## Install and Uninstall Apps

`Install` an app:

```
helm install [app-name] [chart]

helm install -name app1 helmchart1
```

Install an app in a `specific namespace`:

```
helm install [app-name] [chart] --namespace [namespace]
helm install -name helm-release-name helm-charts --namespace sample
```

Override the `default values` with those specified in a file of your choice:
```
helm install [app-name] [chart] --values [yaml-file/url]
helm install -name helm-release-name helm-charts --namespace sample --values helm-charts/values-dev.yaml
helm install -name helm-release-name helm-charts --namespace sample --values helm-charts/values-test.yaml
helm install -name helm-release-name helm-charts --namespace sample --values helm-charts/values-prod.yaml
```

Run a `test installation` to validate and verify the chart:

```
helm install [app-name] --dry-run --debug
```
Uninstall a release:

```
helm uninstall [release]

helm uninstall helm-release-name -n sample
```



## App Upgrade and Rollback

Upgrade an app:

```
helm upgrade [release] [chart]
helm upgrade -name helm-release-name helm-charts --namespace sample
```

Instruct Helm to rollback changes if the upgrade fails:

```
helm upgrade [release] [chart] --atomic
```

Upgrade a release. If it does not exist on the system, install it:

```
helm upgrade [release] [chart] --install
```

Upgrade to a specified version:

```
helm upgrade [release] [chart] --version [version-number]

# first get the info
helm list --namespace sample

helm upgrade -name helm-release-name helm-charts --version 1.0.0 --namespace sample

```
Roll back a release:
```
helm rollback [release] [revision]
helm list --namespace sample
helm rollback -name helm-release-name 18 --namespace sample
```


## List Installed Helm Charts

List Installed Helm Charts in default namespace

```
helm ls
```

output

```
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
my-minio        default         1               2022-12-05 16:45:31.6927425 -0800 PST   deployed        minio-11.10.16  2022.11.11
```

List Installed Helm Charts from all namespace

```
helm ls -aA
```

output

```
NAME                                    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                                   APP VERSION
cert-manager                            cert-manager    1               2022-11-09 17:54:00.8093352 -0800 PST   deployed        cert-manager-v1.10.0                    v1.10.0
csi-secrets-store-provider-azure        kube-system     1               2022-12-09 16:37:01.010911 -0800 PST    deployed        csi-secrets-store-provider-azure-1.3.0  1.3.0
```

List Installed Helm Charts in specific namespace

```
helm ls -n cert-manager
```

output

```
NAME            NAMESPACE       REVISION        UPDATED                                 STATUS          CHART                   APP VERSION
cert-manager    cert-manager    1               2022-11-09 17:54:00.8093352 -0800 PST   deployed        cert-manager-v1.10.0    v1.10.0
```


## Release Monitoring

The helm list command enables listing releases in a Kubernetes cluster
**List** all available releases in the current namespace:

```
helm list

```

List all available releases across `all namespaces`:

```
helm ls -aA
helm list --all-namespaces
```
List all releases in a `specific namespace`:

```
helm list --namespace [namespace]
helm list --namespace sample
```
## helm release status

See the `status` of a specific release:

```
helm status [release]
helm status argocd --namespace argocd
```

output

```
NAME: argocd
LAST DEPLOYED: Fri Jan 20 20:18:33 2023
NAMESPACE: argocd
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
In order to access the server UI you have the following options:

1. kubectl port-forward service/argocd-server -n argocd 8080:443

    and then open the browser on http://localhost:8080 and accept the certificate

2. enable ingress in the values file `server.ingress.enabled` and either
      - Add the annotation for ssl passthrough: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-1-ssl-passthrough
      - Set the `configs.params."server.insecure"` in the values file and terminate SSL at your ingress: https://argo-cd.readthedocs.io/en/stable/operator-manual/ingress/#option-2-multiple-ingress-objects-and-hosts


After reaching the UI the first time you can login with username: admin and the random password generated during the installation. You can find the password by running:

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## release history

Display the release `history`

```
helm history [release]
helm history argocd --namespace argocd
```

output 
```
REVISION        UPDATED                         STATUS    CHART                                                                                                                                                   APP VERSION      DESCRIPTION
1               Fri Jan 20 20:18:33 2023        deployed  argo-cd-5.13.7                                                                                                                                          v2.5.2           Install complete
```

## Helm client environment

See information about the Helm client environment:

```
helm env
```

output
```
HELM_BIN="C:\ProgramData\chocolatey\lib\kubernetes-helm\tools\windows-amd64\helm.exe"
HELM_CACHE_HOME="C:\Users\ANJI~1.KEE\AppData\Local\Temp\helm"
HELM_CONFIG_HOME="C:\Users\anji.keesari\AppData\Roaming\helm"
HELM_DATA_HOME="C:\Users\anji.keesari\AppData\Roaming\helm"
HELM_DEBUG="false"
HELM_KUBEAPISERVER=""
HELM_KUBEASGROUPS=""
HELM_KUBEASUSER=""
HELM_KUBECAFILE=""
HELM_KUBECONTEXT=""
HELM_KUBETOKEN=""
HELM_MAX_HISTORY="10"
HELM_NAMESPACE="default"
HELM_PLUGINS="C:\Users\anji.keesari\AppData\Roaming\helm\plugins"
HELM_REGISTRY_CONFIG="C:\Users\anji.keesari\AppData\Roaming\helm\registry\config.json"
HELM_REPOSITORY_CACHE="C:\Users\ANJI~1.KEE\AppData\Local\Temp\helm\repository"
HELM_REPOSITORY_CONFIG="C:\Users\anji.keesari\AppData\Roaming\helm\repositories.yaml"
```


## Chart Management

Run tests (**lint**) to examine a chart and identify possible issues:

```
helm lint [chart]
helm lint helm-charts
```
output
```
==> Linting helm-charts
[INFO] Chart.yaml: icon is recommended

1 chart(s) linted, 0 chart(s) failed
```

Inspect a chart and list its **contents**:
```
helm show all [chart] 
helm show all helm-charts
```
output
```
apiVersion: v2
appVersion: 1.0.0
description: A Helm chart for project1 projects for AKS cluster
name: helm-release-name
type: application
version: "20220823.12"

---
namespace: sample
registryPrefix: acrname.azurecr.io
```

Display the chart’s **definition**:
```
helm show chart [chart] 
helm show chart helm-charts
```
output
```
apiVersion: v2
appVersion: 1.0.0
description: A Helm chart for project-1 projects for AKS cluster
name: helm-release-name
type: application
version: "20220823.12"
```
Display the chart’s **values**:
```
helm show values [chart]
helm show values helm-charts
```
output
```
namespace: sample
registryPrefix: acrname.azurecr.io
```
**Download** a chart:
```
helm pull [chart]
helm pull helm-charts
```
Display a list of a chart’s **dependencies**:
```
helm dependency list [chart]
helm dependency list helm-charts
```

## References
- <https://phoenixnap.com/kb/helm-commands-cheat-sheet>
- <https://github.com/RehanSaeed/Helm-Cheat-Sheet>
