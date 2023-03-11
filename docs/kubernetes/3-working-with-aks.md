Now it is time to learn some basic commands of `kubectl` to interacting with our AKS cluster so that performing actions in  next labs will easer.

## Prerequisites

- Download & Install Azure CLI - <https://learn.microsoft.com/en-us/cli/azure/install-azure-cli>
- Existing AKS cluster
- Visual studio code
- kubectl

## Install kubectl

Here are the steps to use the `kubectl` command to manage an AKS cluster:

First, you'll need to install kubectl on your local machine. You can download the latest version of kubectl from the Kubernetes official website. <https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/>

Windows OS users.

``` sh
choco install kubernetes-cli
```

Mac OS users.

look into this for more info - <https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/>

``` sh
brew install kubectl

# verify the installation
kubectl version
```

## Login to azure & set subscription

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

## Connect to AKS cluster

Before using `kubectl` commands, make sure that you connect to Kubernetes cluster first using following commands.

``` sh
# azure CLI
az aks get-credentials --resource-group <resource-group-name> --name <aks-cluster-name>

# Azure Kubernetes Service Cluster User Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev"

# Azure Kubernetes Service Cluster Admin Role
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster1-dev" --admin
```

##  Test workloads in AKS

Once you've connected to the cluster successfully then try running following commands to see everything is working as expected in your cluster.

``` sh
kubectl get all --namespace <namespace-name>
# get nodes
kubectl get no
kubectl get namespace -A
```

## Cluster-info

Listing and inspecting your cluster...helpful for knowing which cluster is your current context

``` sh
kubectl cluster-info
```
output

``` sh
Kubernetes control plane is running at https://cluster1-dns-73957b84.hcp.eastus.azmk8s.io:443
CoreDNS is running at https://cluster1-dns-73957b84.hcp.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://cluster1-dns-73957b84.hcp.eastus.azmk8s.io:443/api/v1/namespaces/kube-system/services/https:metrics-server:/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.

```

## Namespaces

```
kubectl get namespaces
```
output

```
NAME                STATUS   AGE
default             Active   6d4h
gatekeeper-system   Active   6d4h
kube-node-lease     Active   6d4h
kube-public         Active   6d4h
kube-system         Active   6d4h
sample              Active   8h
```
## Nodes

```
kubectl get nodes
```

```
NAME                                STATUS   ROLES   AGE     VERSION
aks-agentpool-25316841-vmss000000   Ready    agent   4h37m   v1.23.12
aks-agentpool-25316841-vmss000001   Ready    agent   4h37m   v1.23.12
```

## Pods

```
kubectl get pods -A
```

```
NAMESPACE           NAME                                     READY   STATUS    RESTARTS   AGE
gatekeeper-system   gatekeeper-audit-75cc4b59d7-7rm7m        1/1     Running   0          4h38m
gatekeeper-system   gatekeeper-controller-78bdb459dd-2fqkm   1/1     Running   0          4h38m
gatekeeper-system   gatekeeper-controller-78bdb459dd-9kcmt   1/1     Running   0          4h38m
kube-system         aks-secrets-store-csi-driver-6fmxb       3/3     Running   0          4h38m
kube-system         aks-secrets-store-csi-driver-nkwls       3/3     Running   0          4h38m
kube-system         aks-secrets-store-provider-azure-glh4r   1/1     Running   0          4h38m
kube-system         aks-secrets-store-provider-azure-v2vrl   1/1     Running   0          4h38m
kube-system         ama-logs-gm6b6                           2/2     Running   0          4h38m
kube-system         ama-logs-rs-76b7c448-l54gj               1/1     Running   0          4h38m
kube-system         ama-logs-sld4q                           2/2     Running   0          4h38m
kube-system         azure-ip-masq-agent-gjz65                1/1     Running   0          4h38m
kube-system         azure-ip-masq-agent-lf45m                1/1     Running   0          4h38m
kube-system         azure-npm-fktnn                          1/1     Running   0          4h37m
kube-system         azure-npm-n4qmq                          1/1     Running   0          4h37m
kube-system         azure-policy-699bff6d75-txw8w            1/1     Running   0          4h38m
kube-system         azure-policy-webhook-854ffcbfdb-6q2gc    1/1     Running   0          4h38m
kube-system         cloud-node-manager-7lr4k                 1/1     Running   0          4h38m
kube-system         cloud-node-manager-rpt2q                 1/1     Running   0          4h38m
kube-system         coredns-autoscaler-5589fb5654-dbpcz      1/1     Running   0          4h38m
kube-system         coredns-b4854dd98-jrcf2                  1/1     Running   0          4h38m
kube-system         coredns-b4854dd98-kr5f8                  1/1     Running   0          4h37m
kube-system         csi-azuredisk-node-jj9xf                 3/3     Running   0          4h38m
kube-system         csi-azuredisk-node-nwfls                 3/3     Running   0          4h38m
kube-system         csi-azurefile-node-qrvls                 3/3     Running   0          4h38m
kube-system         csi-azurefile-node-wzt2j                 3/3     Running   0          4h38m
kube-system         konnectivity-agent-cd99df756-l6gqp       1/1     Running   0          4h12m
kube-system         konnectivity-agent-cd99df756-pm4xs       1/1     Running   0          4h12m
kube-system         kube-proxy-4kx9d                         1/1     Running   0          4h38m
kube-system         kube-proxy-5dm98                         1/1     Running   0          4h38m
kube-system         metrics-server-f77b4cd8-pmsk4            1/1     Running   0          4h38m
kube-system         metrics-server-f77b4cd8-wsmll            1/1     Running   0          4h38m
```

## Deployments

```
kubectl get deployments -A
```
output

```
NAMESPACE           NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
gatekeeper-system   gatekeeper-audit        1/1     1            1           4h39m
gatekeeper-system   gatekeeper-controller   2/2     2            2           4h39m
kube-system         ama-logs-rs             1/1     1            1           4h39m
kube-system         azure-policy            1/1     1            1           4h39m
kube-system         azure-policy-webhook    1/1     1            1           4h39m
kube-system         coredns                 2/2     2            2           4h39m
kube-system         coredns-autoscaler      1/1     1            1           4h39m
kube-system         konnectivity-agent      2/2     2            2           4h39m
kube-system         metrics-server          2/2     2            2           4h39m

```

## Services
```
kubectl get svc -A
```
output 
```
NAMESPACE           NAME                           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)         AGE
default             kubernetes                     ClusterIP   10.25.0.1       <none>        443/TCP         4h41m
gatekeeper-system   gatekeeper-webhook-service     ClusterIP   10.25.97.0      <none>        443/TCP         4h40m
kube-system         azure-policy-webhook-service   ClusterIP   10.25.58.191    <none>        443/TCP         4h40m
kube-system         kube-dns                       ClusterIP   10.25.0.10      <none>        53/UDP,53/TCP   4h40m
kube-system         metrics-server                 ClusterIP   10.25.122.164   <none>        443/TCP         4h40m
kube-system         npm-metrics-cluster-service    ClusterIP   10.25.231.109   <none>        9000/TCP        4h40m
```

## ConfigMaps

```
kubectl get configmaps -A
```

```
NAMESPACE           NAME                                    DATA   AGE
default             kube-root-ca.crt                        1      4h41m
gatekeeper-system   kube-root-ca.crt                        1      4h41m
kube-node-lease     kube-root-ca.crt                        1      4h41m
kube-public         kube-root-ca.crt                        1      4h41m
kube-system         ama-logs-rs-config                      1      4h41m
kube-system         azure-ip-masq-agent-config-reconciled   1      4h41m
kube-system         azure-npm-config                        1      4h41m
kube-system         cluster-autoscaler-status               1      4h40m
kube-system         container-azm-ms-aks-k8scluster         1      4h41m
kube-system         coredns                                 1      4h41m
kube-system         coredns-autoscaler                      1      4h40m
kube-system         coredns-custom                          0      4h41m
kube-system         extension-apiserver-authentication      6      4h41m
kube-system         kube-root-ca.crt                        1      4h41m
kube-system         overlay-upgrade-data                    4      4h41m
```

## Secrets
```
kubectl get secrets -A
```

```
NAMESPACE           NAME                                             TYPE                                  DATA   AGE
default             default-token-h2svh                              kubernetes.io/service-account-token   3      4h41m
gatekeeper-system   default-token-fjx97                              kubernetes.io/service-account-token   3      4h41m
gatekeeper-system   gatekeeper-admin-token-jhhbk                     kubernetes.io/service-account-token   3      4h41m
gatekeeper-system   gatekeeper-webhook-server-cert                   Opaque                                3      4h41m
kube-node-lease     default-token-gwjtc                              kubernetes.io/service-account-token   3      4h41m
kube-public         default-token-lmnr7                              kubernetes.io/service-account-token   3      4h41m
kube-system         aks-secrets-store-csi-driver-token-n9wg2         kubernetes.io/service-account-token   3      4h41m
kube-system         aks-secrets-store-provider-azure-token-zrswn     kubernetes.io/service-account-token   3      4h41m
kube-system         ama-logs-secret                                  Opaque                                2      4h41m
kube-system         ama-logs-token-ff7zz                             kubernetes.io/service-account-token   3      4h41m
kube-system         attachdetach-controller-token-km8wr              kubernetes.io/service-account-token   3      4h41m
kube-system         azure-cloud-provider-token-krcx6                 kubernetes.io/service-account-token   3      4h41m
kube-system         azure-npm-token-vx74r                            kubernetes.io/service-account-token   3      4h41m
kube-system         azure-policy-token-kk8hf                         kubernetes.io/service-account-token   3      4h41m
kube-system         azure-policy-webhook-account-token-jf62w         kubernetes.io/service-account-token   3      4h41m
kube-system         azure-policy-webhook-cert                        Opaque                                3      4h41m
kube-system         bootstrap-signer-token-9nkl5                     kubernetes.io/service-account-token   3      4h41m
kube-system         bootstrap-token-0f44ke                           bootstrap.kubernetes.io/token         4      4h41m
kube-system         certificate-controller-token-hdq7d               kubernetes.io/service-account-token   3      4h41m
kube-system         cloud-node-manager-token-vd9l2                   kubernetes.io/service-account-token   3      4h41m
kube-system         clusterrole-aggregation-controller-token-5sscz   kubernetes.io/service-account-token   3      4h41m
kube-system         coredns-autoscaler-token-ktffp                   kubernetes.io/service-account-token   3      4h41m
kube-system         coredns-token-v4rm5                              kubernetes.io/service-account-token   3      4h41m
kube-system         cronjob-controller-token-kp2w4                   kubernetes.io/service-account-token   3      4h41m
kube-system         csi-azuredisk-node-sa-token-fm6mv                kubernetes.io/service-account-token   3      4h41m
kube-system         csi-azurefile-node-sa-token-dj2d8                kubernetes.io/service-account-token   3      4h41m
kube-system         daemon-set-controller-token-j28q7                kubernetes.io/service-account-token   3      4h41m
kube-system         default-token-4wvg4                              kubernetes.io/service-account-token   3      4h41m
kube-system         deployment-controller-token-4tmbj                kubernetes.io/service-account-token   3      4h41m
kube-system         disruption-controller-token-p48hj                kubernetes.io/service-account-token   3      4h41m
kube-system         endpoint-controller-token-7g4qt                  kubernetes.io/service-account-token   3      4h41m
kube-system         endpointslice-controller-token-j8jkm             kubernetes.io/service-account-token   3      4h41m
kube-system         endpointslicemirroring-controller-token-csssp    kubernetes.io/service-account-token   3      4h41m
kube-system         ephemeral-volume-controller-token-6qf9d          kubernetes.io/service-account-token   3      4h41m
kube-system         expand-controller-token-rrk2h                    kubernetes.io/service-account-token   3      4h41m
kube-system         generic-garbage-collector-token-56vgh            kubernetes.io/service-account-token   3      4h41m
kube-system         horizontal-pod-autoscaler-token-shkvl            kubernetes.io/service-account-token   3      4h41m
kube-system         job-controller-token-5wn2v                       kubernetes.io/service-account-token   3      4h41m
kube-system         konnectivity-agent-token-vgtm8                   kubernetes.io/service-account-token   3      4h41m
kube-system         konnectivity-certs                               Opaque                                3      4h41m
kube-system         kube-proxy-token-zrfjt                           kubernetes.io/service-account-token   3      4h41m
kube-system         metrics-server-token-92vvw                       kubernetes.io/service-account-token   3      4h41m
kube-system         namespace-controller-token-f7g8t                 kubernetes.io/service-account-token   3      4h41m
kube-system         node-controller-token-jz4h4                      kubernetes.io/service-account-token   3      4h41m
kube-system         persistent-volume-binder-token-pfqqx             kubernetes.io/service-account-token   3      4h41m
kube-system         pod-garbage-collector-token-sxfld                kubernetes.io/service-account-token   3      4h41m
kube-system         pv-protection-controller-token-swj5m             kubernetes.io/service-account-token   3      4h41m
kube-system         pvc-protection-controller-token-tqdv7            kubernetes.io/service-account-token   3      4h41m
kube-system         replicaset-controller-token-q8chf                kubernetes.io/service-account-token   3      4h41m
kube-system         replication-controller-token-7d4zq               kubernetes.io/service-account-token   3      4h41m
kube-system         resourcequota-controller-token-5thvs             kubernetes.io/service-account-token   3      4h41m
kube-system         root-ca-cert-publisher-token-5nc9n               kubernetes.io/service-account-token   3      4h41m
kube-system         service-account-controller-token-4bg7v           kubernetes.io/service-account-token   3      4h41m
kube-system         statefulset-controller-token-kjgh9               kubernetes.io/service-account-token   3      4h41m
kube-system         token-cleaner-token-qbzs2                        kubernetes.io/service-account-token   3      4h41m
kube-system         ttl-after-finished-controller-token-7zgnt        kubernetes.io/service-account-token   3      4h41m
kube-system         ttl-controller-token-9252z                       kubernetes.io/service-account-token   3      4h41m
```

## Cluster details

```
az aks show -g 'rg-aks-dev' -n 'aks-cluster1-dev'
```
```
{
  "aadProfile": {
    "adminGroupObjectIDs": [
    ...
    ...

```

``` sh
kubectl get roles -A
kubectl get rolebindings -A
kubectl get clusterroles -A
kubectl get clusterrolebindings -A

```


## more commands

``` sh

#We can easily filter using group
kubectl api-resources | grep pod

#Explain an indivdual resource in detail
kubectl explain pod | more 
kubectl explain pod.spec | more 
kubectl explain pod.spec.containers | more 
kubectl explain pod --recursive | more 

#Let's take a closer look at our nodes using Describe
#Check out Name, Taints, Conditions, Addresses, System Info, Non-Terminated Pods, and Events
kubectl describe nodes c1-cp1 | more 
kubectl describe nodes c1-node1 | more

#Use -h or --help to find help
kubectl -h | more
kubectl get -h | more
kubectl create -h | more

#Ok, so now that we're tired of typing commands out, let's enable bash auto-complete of our kubectl commands
sudo apt-get install -y bash-completion
echo "source <(kubectl completion bash)" >> ~/.bashrc
source ~/.bashrc
kubectl g[tab][tab] po[tab][tab] --all[tab][tab]

```


## Troubleshooting errors

In case if you are getting following error while running `kubectl` commands than that means you need to convert or switch to Azure kubelogin.

```
❯ az aks browse  -g "rg-aks-dev" -n "aks-cluster1-dev"
Kubernetes resources view on https://portal.azure.com/#resource/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourceGroups/rg-aks-dev/providers/Microsoft.ContainerService/managedClusters/aks-cluster1-dev/workloads
"Kubernetes resources view on https://portal.azure.com/#resource/subscriptions/b635d52c-5170-4366-b262-cc12cba2d9be/resourceGroups/rg-aks-dev/providers/Microsoft.ContainerService/managedClusters/aks-cluster1-dev/workloads"

~ on ☁️
❯ kubectl get no
error: The azure auth plugin has been removed.
Please use the https://github.com/Azure/kubelogin kubectl/client-go credential plugin instead.
See https://kubernetes.io/docs/reference/access-authn-authz/authentication/#client-go-credential-plugins for further details

```
or

```
error: unknown flag: --environment
E0226 12:20:52.660790  116560 memcache.go:238] couldn't get current server API group list: Get "https://cluster1-dns-73957b84.hcp.eastus.azmk8s.io:443/api?timeout=32s": getting credentials: exec: executable kubelogin failed with exit code 1
```

reference for more reading- <https://aptakube.com/blog/how-to-use-azure-kubelogin>

## Install kubelogin
```
brew install Azure/kubelogin/kubelogin
```

``` sh

==> Downloading https://formulae.brew.sh/api/formula.json
Running `brew update --auto-update`...

==> Downloading https://formulae.brew.sh/api/cask.json

==> Tapping azure/kubelogin
Cloning into '/opt/homebrew/Library/Taps/azure/homebrew-kubelogin'...
remote: Enumerating objects: 111, done.
remote: Counting objects: 100% (12/12), done.
remote: Compressing objects: 100% (9/9), done.
remote: Total 111 (delta 6), reused 6 (delta 3), pack-reused 99
Receiving objects: 100% (111/111), 26.54 KiB | 274.00 KiB/s, done.
Resolving deltas: 100% (54/54), done.
Tapped 2 formulae (19 files, 49.0KB).
==> Fetching azure/kubelogin/kubelogin
==> Downloading https://github.com/Azure/kubelogin/releases/download/v0.0.26/kubelogin-darwin-arm6
==> Downloading from https://objects.githubusercontent.com/github-production-release-asset-2e65be/
######################################################################## 100.0%
==> Installing kubelogin from azure/kubelogin
🍺  /opt/homebrew/Cellar/kubelogin/0.0.26: 3 files, 43.4MB, built in 1 second
==> Running `brew cleanup kubelogin`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).

```

To convert your kubectl authentication method to Azure CLI, all you need to do is run:

```
kubelogin convert-kubeconfig -l azurecli
```

That's all we need for now to start working with our AKS cluster.