





## Update deployment.yaml files for tolerations

```bash
      tolerations:
        - key: "TenantName"
          operator: "Equal"
          value: "tenant1"
          effect: "NoSchedule"  # This pod tolerates the taint
```









## Check the status of your node pools 

```bash
az aks nodepool list --resource-group myResourceGroup --cluster-name myAKSCluster

# example
az aks nodepool list --resource-group 'rg-aks-dev' --cluster-name 'aks-cluster1-dev'

# output 
[
  {
    "availabilityZones": null,
    "capacityReservationGroupId": null,    
    "count": 2,
    "creationData": null,
    "currentOrchestratorVersion": "1.26.6",
    "enableAutoScaling": true,
    "enableCustomCaTrust": false,
    "enableEncryptionAtHost": false,
    "enableFips": false,
    "enableNodePublicIp": false,
    "enableUltraSsd": false,
    "gpuInstanceProfile": null,
    "hostGroupId": null,
    "id": "/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourcegroups/rg-aks-dev/providers/Microsoft.ContainerService/managedClusters/aks-cluster1-dev/agentPools/agentpool",
    "kubeletConfig": null,
    "kubeletDiskType": "OS",
    "linuxOsConfig": null,
    "maxCount": 5,
    "maxPods": 110,
    "messageOfTheDay": null,
    "minCount": 2,
    "mode": "System",
    "name": "agentpool",
    "networkProfile": null,
    "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202308.22.0",
    "nodeLabels": null,
    "nodePublicIpPrefixId": null,
    "nodeTaints": null,
    "orchestratorVersion": "1.26.6",
    "osDiskSizeGb": 128,
    "osDiskType": "Managed",
    "osSku": "Ubuntu",
    "osType": "Linux",
    "podSubnetId": null,
    "powerState": {
      "code": "Running"
    },
    "provisioningState": "Succeeded",
    "proximityPlacementGroupId": null,
    "resourceGroup": "rg-aks-dev",
    "scaleDownMode": "Delete",
    "scaleSetEvictionPolicy": null,
    "scaleSetPriority": null,
    "spotMaxPrice": null,
    "tags": null,
    "type": "Microsoft.ContainerService/managedClusters/agentPools",
    "typePropertiesType": "VirtualMachineScaleSets",
    "upgradeSettings": {
      "maxSurge": null
    },
    "vmSize": "Standard_B4ms",
    "vnetSubnetId": "/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourceGroups/rg-vnet1-dev/providers/Microsoft.Network/virtualNetworks/vnet-spoke-dev/subnets/snet-aks1",
    "windowsProfile": null,
    "workloadRuntime": null
  },
  {
    "availabilityZones": [
      "2",
      "3",
      "1"
    ],
    "capacityReservationGroupId": null,
    "count": 1,
    "creationData": null,
    "currentOrchestratorVersion": "1.26.6",
    "enableAutoScaling": true,
    "enableCustomCaTrust": false,
    "enableEncryptionAtHost": false,
    "enableFips": false,
    "enableNodePublicIp": false,
    "enableUltraSsd": false,
    "gpuInstanceProfile": null,
    "hostGroupId": null,
    "id": "/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourcegroups/rg-aks-dev/providers/Microsoft.ContainerService/managedClusters/aks-cluster1-dev/agentPools/usernodepool",
    "kubeletConfig": null,
    "kubeletDiskType": "OS",
    "linuxOsConfig": null,
    "maxCount": 5,
    "maxPods": 110,
    "messageOfTheDay": null,
    "minCount": 1,
    "mode": "User",
    "name": "usernodepool",
    "networkProfile": null,
    "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202308.22.0",
    "nodeLabels": {
      "TenantName": "tenant1"
    },
    "nodePublicIpPrefixId": null,
    "nodeTaints": [
      "TenantName=tenant1:NoSchedule"
    ],
    "orchestratorVersion": "1.26.6",
    "osDiskSizeGb": 128,
    "osDiskType": "Managed",
    "osSku": "Ubuntu",
    "osType": "Linux",
    "podSubnetId": null,
    "powerState": {
      "code": "Running"
    },
    "provisioningState": "Succeeded",
    "proximityPlacementGroupId": null,
    "resourceGroup": "rg-aks-dev",
    "scaleDownMode": "Delete",
    "scaleSetEvictionPolicy": null,
    "scaleSetPriority": null,
    "spotMaxPrice": null,
    "tags": {
      "CreatedBy": "Anji.Keesari",
      "Environment": "dev",
      "Owner": "Anji.Keesari",
      "Project": "Project-1"
    },
    "type": "Microsoft.ContainerService/managedClusters/agentPools",
    "typePropertiesType": "VirtualMachineScaleSets",
    "upgradeSettings": {
      "maxSurge": null
    },
    "vmSize": "Standard_B8ms",
    "vnetSubnetId": "/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourceGroups/rg-vnet1-dev/providers/Microsoft.Network/virtualNetworks/vnet-spoke-dev/subnets/snet-aks1",
    "windowsProfile": null,
    "workloadRuntime": null
  }
]
```

```bash
az aks get-versions --location eastus --query "orchestrators" -o table

# output
OrchestratorType    OrchestratorVersion    Default
------------------  ---------------------  ---------
Kubernetes          1.27.3
Kubernetes          1.27.1
Kubernetes          1.26.6                 True
Kubernetes          1.26.3
Kubernetes          1.25.11
Kubernetes          1.25.6
```
## Create a new node pool with the desired SKU

Standard_DS2_v2
Standard_B8ms

```sh
az aks nodepool add \ 
    --resource-group myResourceGroup \ 
    --cluster-name myAKSCluster \ 
    --name mynodepool \ 
    --node-count 3 \ 
    --node-vm-size Standard_DS3_v2 \ 
    --mode System \ 
    --no-wait

# example
az aks nodepool add \
    --resource-group 'rg-aks-dev' \
    --cluster-name 'aks-cluster1-dev' \
    --name agentpool1 \
    --node-count 2 \
    --node-vm-size Standard_DS3_v2 \
    --mode System \
    --no-wait

az aks nodepool add `
    --resource-group 'rg-aks-dev' `
    --cluster-name 'aks-cluster1-dev' `
    --name nodepool2 `
    --node-count 3 `
    --node-vm-size Standard_B8ms `
    --mode System `
    --no-wait


# output

```



```sh
kubectl get nodes

# output
NAME                                 STATUS   ROLES   AGE   VERSION
aks-mynodepool-20823458-vmss000000   Ready    agent   23m   v1.21.9
aks-mynodepool-20823458-vmss000001   Ready    agent   23m   v1.21.9
aks-mynodepool-20823458-vmss000002   Ready    agent   23m   v1.21.9
aks-nodepool1-31721111-vmss000000    Ready    agent   10d   v1.21.9
aks-nodepool1-31721111-vmss000001    Ready    agent   10d   v1.21.9
aks-nodepool1-31721111-vmss000002    Ready    agent   10d   v1.21.9
```


## Cordon the existing nodes

```sh
az aks nodepool update --disable-cluster-autoscaler -g 'rg-aks-dev' -n nodepool1 --cluster-name 'aks-cluster1-dev'

# output



# Cordon the existing nodes

kubectl cordon `
aks-nodepool2-23546727-vmss00001r `
aks-nodepool2-23546727-vmss00001s `
aks-nodepool2-23546727-vmss00001u 
```

## Drain the existing nodes

```sh
# Drain the existing nodes

kubectl drain `
aks-nodepool2-23546727-vmss00001r `
aks-nodepool2-23546727-vmss00001s `
aks-nodepool2-23546727-vmss00001u `
--ignore-daemonsets --delete-emptydir-data
```

```sh
# Remove the existing node pool

az aks nodepool delete `
    --resource-group 'rg-aks-dev' `
    --cluster-name 'aks-cluster1-dev' `
    --name agentpool

```

  
## Reference

- <a href="https://learn.microsoft.com/en-us/azure/aks/resize-node-pool?tabs=azure-cli" target="_blank">Resize node pools in AKS</a>
- <a href="https://shisho.dev/dojo/providers/azurerm/Container/azurerm-kubernetes-cluster-node-pool/" target="_blank">Azure Container Node Pool</a>
- <a href="https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/data-sources/kubernetes_cluster_node_pool" target="_blank">Azure Container Node Pool</a>
- 
- 