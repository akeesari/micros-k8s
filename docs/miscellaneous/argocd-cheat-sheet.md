# ArgoCD Cheat Sheet

## Introduction

`ArgoCD` is a popular tool for managing Kubernetes applications and deploying them in a declarative manner. ArgoCD provides a web UI, but it also has a command-line interface (CLI) that can be used to manage applications, repositories, and other resources.

This page contains a list of commonly used `argocd` commands.

Note: make sure that you login into azure, select the azure subscription & connect k8s cluster before running any `argocd` commands.

## Prerequisites

- Install Argocd CLI [Install Argocd CLI](../devops/argocd/install-argocd-cli.md)
- Azure login
- Select the subscription
- Connect to k8s Cluster



``` sh
# Azure login
az login

# Select the subscription
az account set -s "anji.keesari"
az account show --output table

# Connect to k8s Cluster

# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get namespace -A
```

## commands with description

Here are some common ArgoCD CLI commands and their purposes:

- `argocd login:` This command logs in to an ArgoCD server and saves the session token locally.
- `argocd app create:` This command creates a new application from a Git repository.
- `argocd app get:` This command retrieves information about an existing application, such as its status and configuration.
- `argocd app sync:` This command synchronizes an application's configuration with the desired state specified in its Git repository.
- `argocd app delete:` This command deletes an application from ArgoCD.
- `argocd app diff:` This command displays the differences between the current state of an application and the desired state specified in its Git repository.
- `argocd app history:` This command lists the deployment history of an application in ArgoCD.
- `argocd app rollback:` This command rolls back an application to a previous deployment revision.
- `argocd repo add:` This command adds a Git repository to ArgoCD's list of managed repositories.
- `argocd repo list:` This command lists all the Git repositories that ArgoCD is currently managing.
- `argocd repo rm:` This command removes a Git repository from ArgoCD's list of managed repositories.
- `argocd repo list-resources:` This command lists all the Kubernetes resources in a Git repository.
- `argocd proj create:` This command creates a new project in ArgoCD, which can be used to group related applications and apply shared policies.
- `argocd proj get:` This command retrieves information about an existing project, such as its applications and policies.
- `argocd proj delete:` This command deletes a project from ArgoCD.
- `argocd proj list:` This command lists all the projects in ArgoCD.
- `argocd proj delete:` This command deletes a project from ArgoCD.
- `argocd cluster add:` This command adds a new Kubernetes cluster to ArgoCD's list of managed clusters.
- `argocd cluster list:` This command lists all the Kubernetes clusters that ArgoCD is currently managing.
- `argocd cluster rm:` This command removes a Kubernetes cluster from ArgoCD's list of managed clusters.
- `argocd account update-password:` This command allows you to change the password for your ArgoCD account.
- `argocd account list:` This command lists all the user accounts that have access to ArgoCD.
- `argocd version:` This command retrieves the current version of ArgoCD.



## argocd help
use the following command to get the argocd help

```
argocd help
or 
argocd --help
```
output

```
Available Commands:
  account     Manage account settings
  admin       Contains a set of commands useful for Argo CD administrators and requires direct Kubernetes access
  app         Manage applications
  cert        Manage repository certificates and SSH known hosts entries
  cluster     Manage cluster credentials
  completion  output shell completion code for the specified shell (bash or zsh)
  context     Switch between contexts
  gpg         Manage GPG keys used for signature verification
  help        Help about any command
  login       Log in to Argo CD
  logout      Log out from Argo CD
  proj        Manage projects
  relogin     Refresh an expired authenticate token
  repo        Manage repository connection parameters
  repocreds   Manage repository connection parameters
  version     Print version information
```

## argocd commands help

use the following commands to get the `argocd commands` help

```
argocd help app
```
output
```
Manage applications

Usage:
  argocd app [flags]
  argocd app [command]        

Examples:
  # List all the applications.
  argocd app list

  # Get the details of a application
  argocd app get my-app

  # Set an override parameter
  argocd app set my-app -p image.tag=v1.0.1

Available Commands:
  actions         Manage Resource actions
  create          Create an application
  delete          Delete an application
  delete-resource Delete resource in an application
  diff            Perform a diff against the target and live state.
  edit            Edit application
  get             Get application details
  history         Show application deployment history
  list            List applications
  logs            Get logs of application pods
  manifests       Print manifests of an application
  patch           Patch application
  patch-resource  Patch resource in an application
  resources       List resource of application
  rollback        Rollback application to a previous deployed version by History ID, omitted will Rollback to the previous version
  set             Set application parameters
  sync            Sync an application to its target state
  terminate-op    Terminate running operation of an application
  unset           Unset application parameters
  wait            Wait for an application to reach a synced and healthy state
```

```
argocd help repo
```
output
```
Manage repository connection parameters

Usage:
  argocd repo [flags]
  argocd repo [command]

Available Commands:
  add         Add git repository connection parameters
  get         Get a configured repository by URL
  list        List configured repositories
  rm          Remove repository credentials
```


```
argocd help account
```
output
```
Manage account settings

Usage:
  argocd account [flags]
  argocd account [command]

Available Commands:
  can-i           Can I
  delete-token    Deletes account token
  generate-token  Generate account token
  get             Get account details
  get-user-info   Get user info
  list            List accounts
  update-password Update an account's password
```


## Login to argocd

Before running next set of command you've to login into ArgoCD.

To login to ArgoCD, you can use the `argocd login` command followed by the URL of your ArgoCD server and your credentials. Here's an example:

``` sh
argocd login <ARGOCD_SERVER> [--insecure] [--username <USERNAME>] [--password <PASSWORD>]
```

examples:

```
# localhost argocd login
argocd login localhost:8080 - need to test this

argocd login yourdomainname.com

# IP address of the ArgoCD service.
argocd login 20.241.96.132
```
Note: By default, the Argo CD API server is not exposed with an external IP. To access the API server, Change the argocd-server service type to **LoadBalancer**:

``` bash
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```
If your ArgoCD server is using a self-signed SSL certificate, you may need to use the **--insecure** flag to bypass SSL verification.

Enter ArgoCD credentials
```
admin
xxxxx - in bash you may need to right click the mouse instead of key board copy paste to make it work
```
output
```
WARNING: server is not configured with TLS. Proceed (y/n)? y
Username: admin
Password: 
'admin:login' logged in successfully
Context '20.241.96.132' updated
```

in case if you use the yourdomainname.com URL, you will see output like this.
```
time="2022-11-20T09:44:31-08:00" level=warning msg="Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, 
use flag --grpc-web."
Username: admin
Password: 
'admin:login' logged in successfully
Context 'yourdomainname.com' updated
```

Once you have logged in successfully, ArgoCD will save a session token locally so that you don't have to log in again for subsequent commands.


## cluster list

```
argocd cluster list
```
output
```
time="2022-11-20T09:48:31-08:00" level=warning msg="Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, 
use flag --grpc-web."
SERVER                          NAME        VERSION  STATUS      MESSAGE  PROJECT
https://kubernetes.default.svc  in-cluster  1.22     Successful

```


## Add cluster

Use the following command to register an external AKS Cluster with Argo CD

```
argocd cluster add aks-cluster2-dev
```

output - It will look like this:

```
WARNING: This will create a service account `argocd-manager` on the cluster referenced by context `aks-cluster2-dev` with full cluster level privileges. Do you want to continue [y/N]? y

.
.
.
Cluster 'https://cluster2-dns-89d81b75.hcp.northcentralus.azmk8s.io:443' added

```
## Repository List

```
argocd repo list
```
output

```
TYPE  NAME  REPO                                                  INSECURE  OCI    LFS    CREDS  STATUS      MESSAGE  PROJECT
git         https://github.com/argoproj/argocd-example-apps.git   false     false  false  false  Successful           default
```

## Application List

```
argocd app list
```
output

```
NAME        CLUSTER                         NAMESPACE  PROJECT  STATUS     HEALTH       SYNCPOLICY  CONDITIONS  REPO                                                  PATH         TARGET
guestbook   https://kubernetes.default.svc  default    default  Synced     Healthy      <none>      <none>      https://github.com/argoproj/argocd-example-apps.git   guestbook    HEAD 
```

## App Resources

```
argocd app resources aspnetcore-webapp
```
output

```
time="2022-11-20T09:53:32-08:00" level=warning msg="Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, 
use flag --grpc-web."
GROUP  KIND        NAMESPACE     NAME               ORPHANED
       Service     sample  aspnetcore-webapp  No      
apps   Deployment  sample  aspnetcore-webapp  No      
```

## Application Details

```
argocd app get aspnetcore-webapp
```
output
```
time="2022-11-20T09:54:28-08:00" level=warning msg="Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, 
use flag --grpc-web."
time="2022-11-20T09:54:28-08:00" level=warning msg="Failed to invoke grpc call. Use flag --grpc-web in grpc calls. To avoid this warning message, 
use flag --grpc-web."
Name:               aspnetcore-webapp
Project:            development
Server:             https://kubernetes.default.svc
Namespace:          sample
URL:                https://yourdomainname.com/applications/aspnetcore-webapp
Repo:               https://dev.azure.com/keesari/microservices/_git/argocd
Target:             develop
Path:               sample/aspnetcore-webapp
SyncWindow:         Sync Allowed
Sync Policy:        Automated (Prune)
Sync Status:        Unknown
Health Status:      Healthy

CONDITION        MESSAGE                                                   LAST TRANSITION
ComparisonError  rpc error: code = Unknown desc = authentication required  2022-11-19 21:07:07 -0800 PST


GROUP  KIND        NAMESPACE     NAME               STATUS   HEALTH   HOOK  MESSAGE
       Service     sample  aspnetcore-webapp  Unknown  Healthy        service/aspnetcore-webapp unchanged
apps   Deployment  sample  aspnetcore-webapp  Unknown  Healthy        deployment.apps/aspnetcore-webapp configured
```

## Application Delete

```
argocd app delete guestbook
```

## Application sync

```
argocd app sync guestbook
```

## Project List

```
argocd proj list
```
output

```
NAME                DESCRIPTION                                       DESTINATIONS    SOURCES  CLUSTER-RESOURCE-WHITELIST  NAMESPACE-RESOURCE-BLACKLIST  SIGNATURE-KEYS  ORPHANED-RESOURCES
default                                                               *,*             *        */*                         <none>                        <none>          disabled
```

## Create Project

```
argocd proj create myproj
```


## Logout argocd

When you run argocd logout, ArgoCD will remove the session token that was saved when you logged in, so you will need to log in again with argocd login the next time you want to run any ArgoCD commands.

```
argocd logout 52.159.112.67
```
output

```
Logged out from '52.159.112.67'
```


# Reference

- <https://argo-cd.readthedocs.io/en/release-1.8/user-guide/commands/argocd/>

