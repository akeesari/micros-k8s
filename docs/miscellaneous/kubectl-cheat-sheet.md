
## Introduction

Kubectl is the command line configuration tool for Kubernetes that communicates with a Kubernetes API server. Using Kubectl allows you to create, inspect, update, and delete Kubernetes objects.

This page contains a list of commonly used kubectl commands and flags.

Note: make sure that you login into azure and connect to AKS cluster before running any kubectl commands

## Azure login

```
az login

az account list
or
az account list --output table
```
## Select the subscription

```
az account set -s "anji.keesari"

az account show
or
az account show --output table
```

## Connect to Cluster
```
## Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-rgname-dev" -n "aks-clustername-dev"


## Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-rgname-dev" -n "aks-clustername-dev" --admin

az aks show -g "rg-rgname-dev" -n "aks-clustername-dev"
```
<!-- more info <https://learn.microsoft.com/en-us/azure/aks/control-kubeconfig-access#available-cluster-roles-permissions> -->

## Cluster Management

Display endpoint information about the master and services in the cluster

```
kubectl cluster-info
```
## Version

```
kubectl version
kubectl version --short
kubectl version --short --client
```

## kubectl --help

```
kubectl logs --help
kubectl exec --help
kubectl describe --help
kubectl port-forward --help
```

## Kubectl context and configuration
```
kubectl config view                                  # Show Merged kubeconfig settings.
kubectl config view -o jsonpath='{.users[].name}'    # display the first user
kubectl config view -o jsonpath='{.users[*].name}'   # get a list of users
kubectl config get-contexts                          # display list of contexts
kubectl config current-context                       # display the current-context
kubectl config use-context my-cluster-name           # set the default context to my-cluster-name
kubectl config set-cluster my-cluster-name           # set a cluster entry in the kubeconfig
```

## Creating objects

```
kubectl apply -f ./my-manifest.yaml            # create resource(s)
kubectl apply -f ./my1.yaml -f ./my2.yaml      # create from multiple files
kubectl apply -f ./dir                         # create resource(s) in all manifest files in dir
kubectl apply -f https://git.io/vPieo          # create resource(s) from url
kubectl create deployment nginx --image=nginx  # start a single instance of nginx
kubectl create job hello --image=busybox:1.28 -- echo "Hello World"     #create a Job which prints "Hello World"
kubectl create cronjob hello --image=busybox:1.28   --schedule="*/1 * * * *" -- echo "Hello World"            #create a CronJob that prints "Hello World" every minute
kubectl explain pods                           # get the documentation for pod manifests
```

## Viewing, finding resources

use the namespace in every command to get the resources specific to a namespace

```
# Get commands list

kubectl get nodes
kubectl get namespaces
kubectl get deployments
kubectl get pods
kubectl get services
kubectl get configmaps
kubectl get secrets
kubectl get ingress
kubectl get pv
kubectl get events

# Get commands with basic output

kubectl get services                          # List all services in the namespace
kubectl get pods --all-namespaces             # List all pods in all namespaces
kubectl get pods -o wide                      # List all pods in the current namespace, with more details
kubectl get deployment my-dep                 # List a particular deployment
kubectl get pods                              # List all pods in the namespace
kubectl get pods -A                           # List all pods in all namespaces
kubectl get pod my-pod -o yaml                # Get a pod's YAML

# Describe commands with verbose output

kubectl describe nodes aks-agentpool-57174937-vmss000000
kubectl describe pods aspnet-api -n sample

```

## Updating resources 

```
kubectl set image deployment/frontend www=image:v2               # Rolling update "www" containers of "frontend" deployment, updating the image
kubectl rollout history deployment/frontend                      # Check the history of deployments including the revision
kubectl rollout undo deployment/frontend                         # Rollback to the previous deployment
kubectl rollout undo deployment/frontend --to-revision=2         # Rollback to a specific revision
kubectl rollout status -w deployment/frontend                    # Watch rolling update status of "frontend" deployment until completion
kubectl rollout restart deployment/frontend                      # Rolling restart of the "frontend" deployment
```

## Patching resources

```
# Update a container's image; spec.containers[*].name is required because it's a merge key
kubectl patch pod valid-pod -p '{"spec":{"containers":[{"name":"kubernetes-serve-hostname","image":"new image"}]}}'

# Update a container's image using a json patch with positional arrays
kubectl patch pod valid-pod --type='json' -p='[{"op": "replace", "path": "/spec/containers/0/image", "value":"new image"}]'

# Disable a deployment livenessProbe using a json patch with positional arrays
kubectl patch deployment valid-deployment  --type json   -p='[{"op": "remove", "path": "/spec/template/spec/containers/0/livenessProbe"}]'

```
## Editing resources 

```
# Edit the service named docker-registry
kubectl edit svc/aspnet-api -n sample
```

## Scaling resources 

```
 # Scale a replicaset named 'foo' to 3
kubectl scale --replicas=2 rs/aspnet-api-5466c8b646 -n sample 
```

## Deleting resources

```
 # Delete pods and services with same names "aspnet-api" and "aspnet-api"
kubectl delete pod,service aspnet-api aspnet-api -n sample

```

command for Interacting with running Pods

## kubectl logs
```
kubectl logs aspnet-api-5f4cc4fd9-2lffn -n sample
```

## port-forward

```
kubectl port-forward my-pod 5000:6000 
kubectl port-forward service/aspnet-api 80:80 -n sample

# listen on local port 5000 and forward to port 5000 on Service backend
kubectl port-forward svc/my-service 5000                  

# listen on local port 5000 and forward to Service target port with name <my-service-port>
kubectl port-forward svc/my-service 5000:my-service-port  
```

## kubectl exec 

```
# Run command in existing pod (1 container case)
kubectl exec -n sample -p aspnet-api-5f4cc4fd9-2lffn -- ls /

# Interactive shell access to a running pod (1 container case)                 
kubectl exec --stdin --tty my-pod -- /bin/sh        

# Run command in existing pod (multi-container case)
kubectl exec my-pod -c my-container -- ls /         
```

Interacting with Deployments and Services

```
# dump Pod logs for a Deployment (single-container case)
kubectl logs deploy/my-deployment                         

# dump Pod logs for a Deployment (multi-container case)
kubectl logs deploy/my-deployment -c my-container         

```

## Resource types

List all supported resource types along with their shortnames, API group, whether they are namespaced, and Kind:

```
kubectl api-resources
kubectl api-resources | more
```

Other operations for exploring API resources:

```
kubectl api-resources --namespaced=true      # All namespaced resources
kubectl api-resources --namespaced=false     # All non-namespaced resources
kubectl api-resources -o name                # All resources with simple output (only the resource name)
kubectl api-resources -o wide                # All resources with expanded (aka "wide") output
kubectl api-resources --verbs=list,get       # All resources that support the "list" and "get" request verbs
kubectl api-resources --api-group=extensions # All resources in the "extensions" API group
```

## Command invoke

Use command invoke to access a private Azure Kubernetes Service (AKS) cluster, reference - <https://learn.microsoft.com/en-us/azure/aks/command-invoke>


```
  az aks command invoke --resource-group 'rg-rgname-dev' --name 'aks-aksname-dev' --command "kubectl get namespaces"
  az aks command invoke --resource-group 'rg-rgname-dev' --name 'aks-aksname-dev' --command "kubectl create namespace test"
```

## Short Names

Here is the full list of kubectl short names:

```
Short Name	Long Name
csr	        certificatesigningrequests
cs	        componentstatuses
cm	        configmaps
ds	        daemonsets
deploy	    deployments
ep	        endpoints
ev	        events
hpa	        horizontalpodautoscalers
ing	        ingresses
limits	    limitranges
ns	        namespaces
no	        nodes
pvc	        persistentvolumeclaims
pv	        persistentvolumes
po	        pods
pdb	        poddisruptionbudgets
psp	        podsecuritypolicies
rs	        replicasets
rc	        replicationcontrollers
quota	    resourcequotas
sa	        serviceaccounts
svc	        services
```

## References
- <https://kubernetes.io/docs/reference/kubectl/cheatsheet/>
<!-- - <https://youtu.be/feLpGydQVio> - video
- <https://www.youtube.com/watch?v=Si6og3Wa2Hg> - Visual Studio Code and Kubernetes plugin for beginners
- https://www.bluematador.com/learn/kubectl-cheatsheet
- https://spacelift.io/blog/kubernetes-cheat-sheet
- https://gist.github.com/pydevops/0efd399befd960b5eb18d40adb68ef83
- https://kubernetes.io/docs/reference/kubectl/cheatsheet/
- https://docs.microsoft.com/en-us/cli/azure/aks?view=azure-cli-latest - Manage Azure Kubernetes Services. commands
- https://www.bluematador.com/learn/kubectl-cheatsheet
- https://phoenixnap.com/kb/kubectl-commands-cheat-sheet
- https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands##logs -->