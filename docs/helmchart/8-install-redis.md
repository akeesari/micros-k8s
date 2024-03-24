# Install Redis Cache Helmchart in Azure Kubernetes Services (AKS)

## Introduction

In the microservices and cloud-native applications, efficient data management is crucial for maintaining performance and scalability. Redis, an open-source, in-memory data structure store, is commonly used for data caching. Azure Kubernetes Service (AKS) offers a robust platform for deploying and managing containerized applications, including Redis caches.

This article will guide you through the process of installing Redis Cache using Helm charts in Azure Kubernetes Services (AKS). Helm is a package manager for Kubernetes that simplifies the deployment and management of applications.

## Objective

The objective of this tutorial is to demonstrate how to deploy Redis Cache in an AKS cluster using Helm charts. By the end of this tutorial, you will have a fully functional Redis cache running in your AKS environment. In this exercise we will accomplish & learn how to implement following:

- **Step 1:** Login into Azure
- **Step 2.** Connect to AKS Cluster
- **Step 3.** Add Redis Cache Helm Repository
- **Step 4.** Install Redis Cache Helmchart
- **Step 5.** Verify Redis Cache Resources in AKS
- **Step 6.** Get Redis Cache password
- **Step 7.** Connect using  redis-cli tool
- **Step 8.** Uninstalling the Chart


## Prerequisites

Before you begin, ensure you have the following prerequisites:

- An Azure account with permissions to create resources.
- `Azure CLI` installed on your local machine.
- `kubectl` installed on your local machine.
- `Helm` installed on your local machine.


## Step 1: Login into Azure

Verify that you are logged into the right Azure subscription before start anything in visual studio code

``` sh
# Login to Azure
az login 

# Shows current Azure subscription
az account show

# Lists all available Azure subscriptions
az account list

# Sets Azure subscription to desired subscription using ID
az account set -s "anji.keesari"
```

Follow the on-screen instructions to complete the login process.

## Step 2: Connect to AKS Cluster

Once logged in and set your subscription then connect to your AKS cluster. with your AKS cluster name:

Use the `az aks get-credentials` command to connect to the AKS cluster.


``` sh
# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin

# get nodes
kubectl get no
kubectl get namespace -A
```

## Step 3: Add Redis Cache Helm Repository

Before installing Redis Cache, you need to add the Helm repository for Redis Cache, here I am using bitnami helm chart.

```bash
helm repo list
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
helm search repo redis
```

```bash
helm list -aA
helm list --namespace redis
```

```sh
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
```

## Step 4: Install Redis Cache Helmchart

Now, you can install Redis Cache using Helmchart. Execute the following command:

Let's first see the values, this will allow you to see what is getting installed in your AKS cluster.

```sh
helm show values bitnami/redis > C:\Source\Repos\IaC\terraform\redis-values.yaml
```

Install Helm Chart:

```bash
# use this command if you need to create namespace along with helm install
helm install redis bitnami/redis -n redis --create-namespace --version 19.0.1

# use this command if you already have namespace created
helm upgrade --install redis bitnami/redis -n redis --version 19.0.1
```

This command installs Redis Cache in your AKS cluster. You can customize the installation by providing values files or using Helm chart options.

output:

```sh
NAME: redis
LAST DEPLOYED: Sat Mar 23 20:17:17 2024
NAMESPACE: redis
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
CHART NAME: redis
CHART VERSION: 19.0.1
APP VERSION: 7.2.4

** Please be patient while the chart is being deployed **

Redis&reg; can be accessed on the following DNS names from within your cluster:

    redis-master.redis.svc.cluster.local for read/write operations (port 6379)
    redis-replicas.redis.svc.cluster.local for read-only operations (port 6379)

To get your password run:

    export REDIS_PASSWORD=$(kubectl get secret --namespace redis redis -o jsonpath="{.data.redis-password}" | base64 -d)

To connect to your Redis&reg; server:

1. Run a Redis&reg; pod that you can use as a client:

   kubectl run --namespace redis redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.2.4-debian-12-r9 --command -- sleep infinity

   Use the following command to attach to the pod:

   kubectl exec --tty -i redis-client \
   --namespace redis -- bash

2. Connect using the Redis&reg; CLI:
   REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis-master
   REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis-replicas


    kubectl port-forward --namespace redis svc/redis-master 6379:6379 &
    REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h 127.0.0.1 -p 6379

WARNING: There are "resources" sections in the chart not set. Using "resourcesPreset" is not recommended for production. For production installations, please set the following values according to your workload needs:
  - master.resources
  - replica.resources
+info https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/
```
[![Alt text](images/redis/image-1.png){:style="border: 1px solid black; border-radius: 10px;"}](images/redis/image-1.png){:target="_blank"}

## Step 5: Verify Redis Cache Resources in AKS

To verify that Redis Cache has been successfully installed, you can list the Kubernetes resources:


```bash
helm list -aA
helm list --namespace redis
```

```sh
NAME    NAMESPACE       REVISION        UPDATED                                 STATUS          CHART           APP VERSION
redis   redis           1               2024-03-23 20:17:17.1864465 -0700 PDT   deployed        redis-19.0.1    7.2.4  
```

```bash
kubectl get all -n redis
kubectl get all,configmap,secret -n redis
kubectl get pods -n redis
```

output
```sh
NAME                            DATA   AGE
configmap/kube-root-ca.crt      1      4m37s
configmap/redis-configuration   3      4m36s
configmap/redis-health          6      4m36s
configmap/redis-scripts         2      4m36s

NAME                                 TYPE                 DATA   AGE
secret/redis                         Opaque               1      4m36s
secret/sh.helm.release.v1.redis.v1   helm.sh/release.v1   1      4m37s

NAME                   READY   STATUS    RESTARTS   AGE
pod/redis-master-0     1/1     Running   0          4m35s
NAME                              READY   AGE
statefulset.apps/redis-master     1/1     4m37s
statefulset.apps/redis-replicas   3/3     4m37s
```

You should see Redis Cache pods and services running in your cluster.

## Step 6: Access / Get Redis Cache password

To get your password run:

``` sh
# bash
kubectl get secret --namespace redis redis -o jsonpath="{.data.redis-password}" | base64 -d
export REDIS_PASSWORD=$(kubectl get secret --namespace redis redis -o jsonpath="{.data.redis-password}" | base64 -d)

# PowerShell
kubectl get secret --namespace redis redis -o json | ConvertFrom-Json | Select-Object -ExpandProperty data | Select-Object -ExpandProperty 'redis-password' | ForEach-Object { [System.Text.Encoding]::Utf8.GetString([System.Convert]::FromBase64String($_)) }

# Get the Redis password secret from Kubernetes
$redisSecret = kubectl get secret --namespace redis redis -o json | ConvertFrom-Json
# Decode the password from base64
$redisPassword = [System.Text.Encoding]::Utf8.GetString([System.Convert]::FromBase64String($redisSecret.data."redis-password"))
# Set the Redis password as an environment variable
$env:REDIS_PASSWORD = $redisPassword
```

output 
```sh
Bu0zgY9CRH
```

## Step 7: Connecting Redis Cache with redis-cli

Connecting Redis Cache with `redis-cli` allows you to interact with the Redis server directly from the command line. redis-cli is a command-line interface utility provided by Redis that enables you to execute various commands against the Redis server.

First get the password to connect to your Redis server:

```bash
export REDIS_PASSWORD=$(kubectl get secret --namespace redis redis -o jsonpath="{.data.redis-password}" | base64 -d)
```
- Run a Redis pod that you can use as a client:

```bash
kubectl run --namespace redis redis-client --restart='Never'  --env REDIS_PASSWORD=$REDIS_PASSWORD  --image docker.io/bitnami/redis:7.2.4-debian-12-r9 --command -- sleep infinity
```

output

```sh
pod/redis-client created
```

Use the following command to attach to the pod:

```bash
kubectl exec --tty -i redis-client \
    --namespace redis -- bash
```

output

```sh
I have no name!@redis-client:/$ REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis-master
```

- Connect using the Redis; CLI:

```sh
REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis-master
REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h redis-replicas
```

```sh
kubectl port-forward --namespace redis svc/redis-master 6379:6379 &
    REDISCLI_AUTH="$REDIS_PASSWORD" redis-cli -h 127.0.0.1 -p 6379
```

```sh
PING
```
output

```sh
PONG
```

Next run following set commands from the CLI:

```sh
set key1 hello
set key2 world
set key3 goodbye
set key4 world
```
The same will apply when you get the values.
```sh
get key1
get key2
get key3
get key4
```

You will see output that indicates the client is being redirected to multiple nodes.

```sh
info
```

output


```sh
# Server
redis_version:7.2.4
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:d7e3021cf535cf9d    
redis_mode:standalone
os:Linux 5.15.0-1054-azure x86_64  
arch_bits:64
monotonic_clock:POSIX clock_gettime
multiplexing_api:epoll
atomicvar_api:c11-builtin
gcc_version:12.2.0
process_id:1
process_supervised:no
run_id:b6a2fa96ca6a784d1cd5550b45e7c6bcfc01ba4d
tcp_port:6379
server_time_usec:1711251620634105
uptime_in_seconds:1366
uptime_in_days:0
hz:10
configured_hz:10
lru_clock:16752804
executable:/redis-server
config_file:
io_threads_active:0
listener0:name=tcp,bind=*,bind=-::*,port=6379

# Clients
connected_clients:1
cluster_connections:0
maxclients:10000
client_recent_max_input_buffer:20480
client_recent_max_output_buffer:20504
blocked_clients:0
tracking_clients:0
clients_in_timeout_table:0
total_blocking_keys:0
total_blocking_keys_on_nokey:0

# Memory
used_memory:1104560
used_memory_human:1.05M
used_memory_rss:16007168
used_memory_rss_human:15.27M
used_memory_peak:1322552
used_memory_peak_human:1.26M
used_memory_peak_perc:83.52%
used_memory_overhead:888676
used_memory_startup:866048
used_memory_dataset:215884
used_memory_dataset_perc:90.51%
allocator_allocated:1233272
allocator_active:1486848
allocator_resident:4481024
total_system_memory:16767619072
total_system_memory_human:15.62G
used_memory_lua:31744
used_memory_vm_eval:31744
used_memory_lua_human:31.00K
used_memory_scripts_eval:0
number_of_cached_scripts:0
number_of_functions:0
number_of_libraries:0
used_memory_vm_functions:32768
used_memory_vm_total:64512
used_memory_vm_total_human:63.00K
used_memory_functions:184
used_memory_scripts:184
used_memory_scripts_human:184B
maxmemory:0
maxmemory_human:0B
maxmemory_policy:noeviction
allocator_frag_ratio:1.21
allocator_frag_bytes:253576
allocator_rss_ratio:3.01
allocator_rss_bytes:2994176
rss_overhead_ratio:3.57
rss_overhead_bytes:11526144
mem_fragmentation_ratio:14.80
mem_fragmentation_bytes:14925496
mem_not_counted_for_evict:8
mem_replication_backlog:20508
mem_total_replication_buffers:20504
mem_clients_slaves:0
mem_clients_normal:1928
mem_cluster_links:0
mem_aof_buffer:8
mem_allocator:jemalloc-5.3.0
active_defrag_running:0
lazyfree_pending_objects:0
lazyfreed_objects:0
'
'
'
'

# CPU
used_cpu_sys:1.345162
used_cpu_user:1.532935
used_cpu_sys_children:0.001488
used_cpu_user_children:0.004844
used_cpu_sys_main_thread:1.342549
used_cpu_user_main_thread:1.532146

# Modules

# Errorstats
errorstat_ERR:count=3
errorstat_NOAUTH:count=3

# Cluster
cluster_enabled:0

# Keyspac
```
## Step 8: Uninstalling the Chart

Once you're done experimenting, you can delete the Redis Cache deployment and associated resources:

To uninstall/delete the redis helm deployment run:

```sh
helm delete redis -n redis
```
The command removes all the Kubernetes components associated with the chart and deletes the release.

## Conclusion

In this tutorial, you learned how to deploy Redis Cache in Azure Kubernetes Services using Helm charts. By following these steps, you can integrate Redis Cache seamlessly into your AKS environment, enabling efficient data caching for your containerized applications.

## Reference

- [Run scalable and resilient Redis with AKS](https://techcommunity.microsoft.com/t5/apps-on-azure-blog/run-scalable-and-resilient-redis-with-kubernetes-and-azure/ba-p/3247956)
- [helm/charts](https://github.com/helm/charts/tree/master/stable/redis)
- [Bitnami package for Redis](https://artifacthub.io/packages/helm/bitnami/redis)

<!-- 
- https://medium.com/@thanawitsupinnapong/setting-up-redis-in-kubernetes-with-helm-and-manual-persistent-volume-f1d52fa1919f 
- https://thelinuxnotes.com/index.php/how-to-deploy-redis-in-kubernetes-with-helm-chart/
-->