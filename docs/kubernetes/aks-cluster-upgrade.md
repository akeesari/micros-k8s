
## Introduction


## login to Azure

Verify that you are logged into the right Azure subscription before start anything in visual studio code

``` sh
# Login to Azure
az login 

# Set Azure subscription
az account set -s "anji.keesari"
```

## Connect to Cluster

Use the following command to connect to your AKS cluster.

``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get no -o wide
kubectl get namespace -A
```


## Command to list the kubernetes version and the available kubernetes version upgrades on control plane 

```sh
az aks get-upgrades \
   --resource-group ResourceGroupName --name 'AKSClusterName' --output table

# example
az aks get-upgrades \
   --resource-group 'rg-aks-dev' --name 'aks-cluster1-dev' --output table

# output
Name     ResourceGroup    MasterVersion    Upgrades       
-------  ---------------  ---------------  ---------------
default  rg-aks-dev       1.24.9           1.25.6, 1.25.11

```


## Command to list the kubernetes version for all the nodes in your nodepools 

```sh
az aks nodepool list \  
   --resource-group ResourceGroupName --cluster-name AKSClusterName \
   --query "[].{Name:name,k8version:orchestratorVersion}" --output table

# example
az aks nodepool list \
   --resource-group 'rg-aks-dev' --cluster-name 'aks-cluster1-dev' \
   --query "[].{Name:name,k8version:orchestratorVersion}" --output table
# output
Name       K8version
---------  -----------
agentpool  1.24.9
```

## Upgrade kubernetes version on all nodes at once 

In order to upgrade all nodes at once - we run the following command, this will upgrade all control plane and worker nodes at once to the desired kubernetes version

```sh
az aks upgrade \
   --resource-group ResourceGroupName --name AKSClusterName \
    --no-wait \
   --kubernetes-version KubernetesVersion

# example
az aks upgrade \
   --resource-group 'rg-aks-dev' --name 'aks-cluster1-dev' \
    --no-wait \
   --kubernetes-version 1.24.9

#output
Kubernetes may be unavailable during cluster upgrades.
 Are you sure you want to perform this operation? (y/N): y
Since control-plane-only argument is not specified, this will upgrade the control plane AND all nodepools to version 1.25.6. Continue? (y/N): y

```

Message: Upgrades are disallowed while cluster is in a failed state. For resolution steps visit https://aka.ms/aks-cluster-failed to troubleshoot why the cluster state may have failed and steps to fix cluster state.←[0m

fix for the error:
<https://serverfault.com/questions/1067433/aks-version-upgrade-error-operation-failed-with-status-conflict-details-up>

## Upgrade kubernetes version on control plane alone 

If we only want to upgrade the kubernetes version for control plane alone - we run the same command but with the --control-plane-only switch 

```sh
az aks upgrade \
   --resource-group ResourceGroupName --name AKSClusterName \
   --control-plane-only --no-wait \
   --kubernetes-version KubernetesVersion
```

## Upgrade Kubernetes version on specific node pools

If we need to upgrade the kubernetes version on only specific node pools we can run the following command by specifiying the nodepoolname and the kubernetes version 

```sh
az aks nodepool upgrade \
   --resource-group ResourceGroupName --cluster-name AKSClusterName --name NodePoolName \
   --no-wait --kubernetes-version KubernetesVersion
```

## Nodepool image upgrade 

Running the following command lists all the current nodeimage versions of the  nodepool 

```sh
az aks nodepool list \
   --resource-group ResourceGroupName --cluster-name AKSClusterName \
   --query "[].{Name:name,NodeImageVersion:nodeImageVersion}" --output table

# example
az aks nodepool list `
   --resource-group 'rg-ewm30-test' --cluster-name 'aks-multitenant-test' `
   --query "[].{Name:name,NodeImageVersion:nodeImageVersion}" --output table

```

the following command to list the available latest nodeimage version for us 

```sh
az aks nodepool get-upgrades \
   --resource-group ResourceGroupName --cluster-name AKSClusterName \
   --nodepool-name NodePoolName --output table

# example
az aks nodepool get-upgrades `
   --resource-group 'rg-ewm30-test' --cluster-name 'aks-multitenant-test' `
   --nodepool-name agentpool --output table
```

## Upgrade nodeimage version for specific nodepool 

In order to upgrade the nodeimage version for a specific nodepool we run the following command : 

```sh
az aks nodepool upgrade \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool \
    --node-image-only
```

```sh
az aks nodepool show \
    --resource-group myResourceGroup \
    --cluster-name myAKSCluster \
    --name mynodepool

# example
az aks nodepool show `
    --resource-group 'rg-ewm30-test' `
    --cluster-name 'aks-multitenant-test' `
    --name agentpool

```
## Upgrade nodeimage version for all nodepools at once 

```sh
az aks upgrade \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-image-only
```
Remember If you don’t run the nodeimage switch remember the whole cluster including the kubernetes version of the control plane is also upgraded.

```sh
az aks show \
    --resource-group myResourceGroup \
    --name myAKSCluster


az aks show -g 'rg-ewm30-test' -n  'aks-multitenant-test' 
```

AKS Patching and Node Pool Upgrade Azure Kubernetes Services (AKS) 

Type of upgrades:

- Kubernetes version upgrades  - <https://learn.microsoft.com/en-us/azure/aks/upgrade-cluster?tabs=azure-cli>
- node images version Upgrade - <https://learn.microsoft.com/en-us/azure/aks/node-image-upgrade>
- Automatically upgrade an AKS cluster - <https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster> 

3 ways to upgrade nodes
- All at once
- Only Control plan
- only selected nodes
  
## Reference

- <a href="https://learn.microsoft.com/en-us/azure/aks/supported-kubernetes-versions?tabs=azure-cli#aks-kubernetes-release-calendar"  target="_blank" rel="noopener">Supported Kubernetes versions in Azure Kubernetes Service (AKS)</a>
- <a href="https://learn.microsoft.com/en-us/azure/aks/create-node-pools" target="_blank">Create node pools for a cluster in AKS</a>
- <a href="https://www.youtube.com/watch?v=45UxnRj11_g"  target="_blank" rel="noopener">Azure Kubernetes Services (AKS) Node Pools</a>
- <a href="https://www.youtube.com/watch?v=45UxnRj11_g"  target="_blank" rel="noopener">Azure Kubernetes Services (AKS) Node Pools</a>
- <a href="https://www.youtube.com/watch?v=aGT3UtZoiA0"  target="_blank" rel="noopener">Taints, Tolerations, NodeSelector in AKS (Azure Kubernetes Services)</a>
- <a href="https://www.youtube.com/watch?v=Afl72C-FEMg&t=604s"  target="_blank" rel="noopener">AKS Patching and Node Pool Upgrade Azure Kubernetes Services (AKS) </a>
- Kubernetes version upgrades  - <https://learn.microsoft.com/en-us/azure/aks/upgrade-cluster?tabs=azure-cli>
- node images version Upgrade - <https://learn.microsoft.com/en-us/azure/aks/node-image-upgrade>
- Automatically upgrade an AKS cluster - <https://learn.microsoft.com/en-us/azure/aks/auto-upgrade-cluster> 
