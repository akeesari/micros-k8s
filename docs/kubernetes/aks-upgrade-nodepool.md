
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
kubectl get namespace -A
```

## Technical Scenario

Assume you want to resize an existing node pool, called nodepool1, from SKU size Standard_DS2_v2 to Standard_DS3_v2. To accomplish this task, you'll need to create a new node pool using Standard_DS3_v2, move workloads from nodepool1 to the new node pool, and remove nodepool1. In this example, we'll call this new node pool mynodepool.

```sh
kubectl get nodes -o wide

# output
NAME                                STATUS   ROLES   AGE    VERSION    INTERNAL-IP   EXTERNAL-IP   OS-IMAGE             KERNEL-VERSION     CONTAINER-RUNTIME
aks-agentpool-25316841-vmss000000   Ready    agent   198d   v1.23.12   10.64.4.4     <none>        Ubuntu 18.04.6 LTS   5.4.0-1101-azure   containerd://1.6.15+azure-1
aks-agentpool-25316841-vmss000001   Ready    agent   198d   v1.23.12   10.64.4.113   <none>        Ubuntu 18.04.6 LTS   5.4.0-1101-azure   containerd://1.6.15+azure-1

```

```sh
kubectl get pods -o wide -A

# output
kube-system         azure-policy-557988f6df-q5rgf                       1/1     Running   0             23d    10.64.4.171   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         azure-policy-webhook-5dfcfc5998-cn7p6               1/1     Running   0             23d    10.64.4.161   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         cloud-node-manager-j8zg4                            1/1     Running   1 (23d ago)   72d    10.64.4.4     aks-agentpool-25316841-vmss000000   <none>           <none>
kube-system         cloud-node-manager-p8nk5                            1/1     Running   0             72d    10.64.4.113   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         coredns-785fcf7bdd-6d84r                            1/1     Running   0             82d    10.64.4.214   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         coredns-785fcf7bdd-n8bvt                            1/1     Running   0             23d    10.64.4.199   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         coredns-autoscaler-65bb858f95-6lwkf                 1/1     Running   0             23d    10.64.4.117   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         csi-azuredisk-node-2n6md                            3/3     Running   0             10d    10.64.4.4     aks-agentpool-25316841-vmss000000   <none>           <none>
kube-system         csi-azuredisk-node-r7snn                            3/3     Running   0             10d    10.64.4.113   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         csi-azurefile-node-7tjxl                            3/3     Running   0             3d1h   10.64.4.113   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         csi-azurefile-node-nfntk                            3/3     Running   0             3d1h   10.64.4.4     aks-agentpool-25316841-vmss000000   <none>           <none>
kube-system         konnectivity-agent-6d4987776d-2wbc2                 1/1     Running   0             23d    10.64.4.142   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         konnectivity-agent-6d4987776d-hdgx9                 1/1     Running   0             82d    10.64.4.204   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         kube-proxy-dvl5k                                    1/1     Running   0             44d    10.64.4.113   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         kube-proxy-jx2qw                                    1/1     Running   1 (23d ago)   44d    10.64.4.4     aks-agentpool-25316841-vmss000000   <none>           <none>
kube-system         metrics-server-7757d565cf-rrktr                     2/2     Running   0             41d    10.64.4.181   aks-agentpool-25316841-vmss000001   <none>           <none>
kube-system         metrics-server-7757d565cf-zz97t                     2/2     Running   0             23d    10.64.4.190   aks-agentpool-25316841-vmss000001   <none>           <none>
sample              aks-helloworld-one-6965865b8b-dk6w9                 1/1     Running   0             192d   10.64.4.116   aks-agentpool-25316841-vmss000001   <none>           <none>
sample              aks-helloworld-two-66c5cf894b-vmztv                 1/1     Running   0             192d   10.64.4.115   aks-agentpool-25316841-vmss000001   <none>           <none>
sample              aspnet-api-79b4cbf4bb-54cng                         1/1     Running   0             10d    10.64.4.100   aks-agentpool-25316841-vmss000000   <none>           <none>

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
    "currentOrchestratorVersion": "1.24.9",
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
    "nodeImageVersion": "AKSUbuntu-1804gen2containerd-2023.02.09",
    "nodeLabels": null,
    "nodePublicIpPrefixId": null,
    "nodeTaints": null,
    "orchestratorVersion": "1.24.9",
    "osDiskSizeGb": 128,
    "osDiskType": "Managed",
    "osSku": "Ubuntu",
    "osType": "Linux",
    "podSubnetId": null,
    "powerState": {
      "code": "Running"
    },
    "provisioningState": "Failed",
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

Use the az aks nodepool add command to create a new node pool called mynodepool with three nodes using the Standard_DS3_v2 VM SKU:

current size - Standard_DS2_v2
new size - Standard_B8ms

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
    --resource-group 'rg-ewm30-dev' `
    --cluster-name 'aks-multitenant-dev' `
    --name nodepool2 `
    --node-count 3 `
    --node-vm-size Standard_B8ms `
    --mode System `
    --no-wait


# output
The behavior of this command has been altered by the following extension: aks-preview
```

After a few minutes, the new node pool has been created:

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

Cordoning marks specified nodes as unschedulable and prevents any more pods from being added to the nodes.

First, obtain the names of the nodes you'd like to cordon with `kubectl get nodes`. Your output should look similar to the following:

```sh
az aks nodepool update --disable-cluster-autoscaler -g 'rg-ewm30-dev' -n nodepool1 --cluster-name 'aks-multitenant-dev'

# output
The behavior of this command has been altered by the following extension: aks-preview
 | Running ..

 {
  "availabilityZones": null,
  "capacityReservationGroupId": null,
  "count": 2,
  "creationData": null,
  "currentOrchestratorVersion": "1.26.6",
  "enableAutoScaling": false,
  "enableCustomCaTrust": null,
  "enableEncryptionAtHost": false,
  "enableFips": false,
  "enableNodePublicIp": false,
  "enableUltraSsd": false,
  "gpuInstanceProfile": null,
  "hostGroupId": null,
  "id": "/subscriptions/5e5eafbf-e675-41d2-9d44-4f93b050d001/resourcegroups/rg-ewm30-prod/providers/Microsoft.ContainerService/managedClusters/aks-multitenant-p01/agentPools/agentpool",
  "kubeletConfig": null,
  "kubeletDiskType": "OS",
  "linuxOsConfig": null,
  "maxCount": null,
  "maxPods": 110,
  "messageOfTheDay": null,
  "minCount": null,
  "mode": "System",
  "name": "agentpool",
  "networkProfile": null,
  "nodeImageVersion": "AKSUbuntu-2204gen2containerd-202307.27.0",
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
  "resourceGroup": "rg-ewm30-prod",
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
  "vnetSubnetId": "/subscriptions/5e5eafbf-e675-41d2-9d44-4f93b050d001/resourceGroups/rg-networkcore-prod/providers/Microsoft.Network/virtualNetworks/vnet-ewm30spoke1-prod/subnets/snet-aks-multitenant",
  "windowsProfile": null,
  "workloadRuntime": null
}
```



Next, using `kubectl cordon <node-names>`, specify the desired nodes in a space-separated list:

```sh
kubectl cordon `
aks-nodepool2-23546727-vmss00001r `
aks-nodepool2-23546727-vmss00001s `
aks-nodepool2-23546727-vmss00001u 

# output
node/aks-nodepool2-23546727-vmss00001r cordoned
aks-nodepool2-23546727-vmss00001s cordoned
aks-nodepool2-23546727-vmss00001u  cordoned
```

## Drain the existing nodes

Draining nodes will cause pods running on them to be evicted and recreated on the other, schedulable nodes.

To drain nodes, use `kubectl drain <node-names> --ignore-daemonsets --delete-emptydir-data`, again using a space-separated list of node names:

```sh
# Drain the existing nodes

kubectl drain `
aks-nodepool2-23546727-vmss00001r `
aks-nodepool2-23546727-vmss00001s `
aks-nodepool2-23546727-vmss00001u `
--ignore-daemonsets --delete-emptydir-data
```
After the drain operation finishes, all pods other than those controlled by daemon sets are running on the new node pool:


```sh
kubectl get pods -o wide -A

# output
NAMESPACE     NAME                                  READY   STATUS    RESTARTS   AGE     IP           NODE                                 NOMINATED NODE   READINESS GATES
default       sampleapp2-74b4b974ff-676sz           1/1     Running   0          15m     10.244.4.5   aks-mynodepool-20823458-vmss000002   <none>           <none>
default       sampleapp2-76b6c4c59b-rhmzq           1/1     Running   0          16m     10.244.4.3   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   azure-ip-masq-agent-4n66k             1/1     Running   0          10d     10.240.0.6   aks-nodepool1-31721111-vmss000002    <none>           <none>
kube-system   azure-ip-masq-agent-9p4c8             1/1     Running   0          10d     10.240.0.4   aks-nodepool1-31721111-vmss000000    <none>           <none>
kube-system   azure-ip-masq-agent-nb7mx             1/1     Running   0          10d     10.240.0.5   aks-nodepool1-31721111-vmss000001    <none>           <none>
kube-system   azure-ip-masq-agent-sxn96             1/1     Running   0          49m     10.240.0.9   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   azure-ip-masq-agent-tsq98             1/1     Running   0          49m     10.240.0.8   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   azure-ip-masq-agent-xzrdl             1/1     Running   0          49m     10.240.0.7   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   coredns-845757d86-d2pkc               1/1     Running   0          17m     10.244.3.2   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   coredns-845757d86-f8g9s               1/1     Running   0          17m     10.244.5.2   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   coredns-autoscaler-5f85dc856b-f8xh2   1/1     Running   0          17m     10.244.4.2   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   csi-azuredisk-node-7md2w              3/3     Running   0          49m     10.240.0.7   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   csi-azuredisk-node-9nfzt              3/3     Running   0          10d     10.240.0.4   aks-nodepool1-31721111-vmss000000    <none>           <none>
kube-system   csi-azuredisk-node-bblsb              3/3     Running   0          10d     10.240.0.5   aks-nodepool1-31721111-vmss000001    <none>           <none>
kube-system   csi-azuredisk-node-lcmtz              3/3     Running   0          49m     10.240.0.9   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   csi-azuredisk-node-mmncr              3/3     Running   0          49m     10.240.0.8   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   csi-azuredisk-node-tjhj4              3/3     Running   0          10d     10.240.0.6   aks-nodepool1-31721111-vmss000002    <none>           <none>
kube-system   csi-azurefile-node-29w6z              3/3     Running   0          49m     10.240.0.9   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   csi-azurefile-node-4nrx7              3/3     Running   0          49m     10.240.0.7   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   csi-azurefile-node-9pcr8              3/3     Running   0          3d11h   10.240.0.6   aks-nodepool1-31721111-vmss000002    <none>           <none>
kube-system   csi-azurefile-node-bh2pc              3/3     Running   0          3d11h   10.240.0.5   aks-nodepool1-31721111-vmss000001    <none>           <none>
kube-system   csi-azurefile-node-gqqnv              3/3     Running   0          49m     10.240.0.8   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   csi-azurefile-node-h75gq              3/3     Running   0          3d11h   10.240.0.4   aks-nodepool1-31721111-vmss000000    <none>           <none>
kube-system   konnectivity-agent-6cd55c69cf-2bbp5   1/1     Running   0          17m     10.240.0.7   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   konnectivity-agent-6cd55c69cf-7xzxj   1/1     Running   0          16m     10.240.0.8   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   kube-proxy-4wzx7                      1/1     Running   0          10d     10.240.0.4   aks-nodepool1-31721111-vmss000000    <none>           <none>
kube-system   kube-proxy-7h8r5                      1/1     Running   0          49m     10.240.0.7   aks-mynodepool-20823458-vmss000000   <none>           <none>
kube-system   kube-proxy-g5tvr                      1/1     Running   0          10d     10.240.0.6   aks-nodepool1-31721111-vmss000002    <none>           <none>
kube-system   kube-proxy-mrv54                      1/1     Running   0          10d     10.240.0.5   aks-nodepool1-31721111-vmss000001    <none>           <none>
kube-system   kube-proxy-nqmnj                      1/1     Running   0          49m     10.240.0.9   aks-mynodepool-20823458-vmss000002   <none>           <none>
kube-system   kube-proxy-zn77s                      1/1     Running   0          49m     10.240.0.8   aks-mynodepool-20823458-vmss000001   <none>           <none>
kube-system   metrics-server-774f99dbf4-2x6x8       1/1     Running   0          16m     10.244.4.4   aks-mynodepool-20823458-vmss000002   <none>           <none>
```

## Remove the existing node pool

To delete the existing node pool, use the Azure portal or the az aks nodepool delete command:

```sh
az aks nodepool delete `
    --resource-group 'rg-ewm30-dev' `
    --cluster-name 'aks-multitenant-dev' `
    --name agentpool

```
After completion, the final result is the AKS cluster having a single, new node pool with the new, desired SKU size and all the applications and pods properly running:
  
## Reference

- <a href="https://learn.microsoft.com/en-us/azure/aks/resize-node-pool?tabs=azure-cli" target="_blank">Resize node pools in AKS</a>
