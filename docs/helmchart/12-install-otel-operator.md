# **Install OpenTelemetry Operator Helmchart in AKS**

## **Introduction**

The **OpenTelemetry Operator** is a Kubernetes Operator that simplifies the deployment and management of OpenTelemetry components in a Kubernetes cluster. OpenTelemetry is a set of APIs, libraries, agents, and instrumentation to enable observability in cloud-native applications. It is designed to collect metrics, traces, and logs from applications, microservices, and infrastructure. 

In this article, we will guide you through the process of installing the OpenTelemetry Operator Helm Cha

## **Step 1: Login into Azure**

Ensure that you are logged into the correct Azure subscription before proceeding.

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

## **Step 2: Connect to AKS Cluster**

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

## **Step 3: Add Helm Chart Repository**

Run the following commands to add the OpenTelemetry operator's Helm chart repository:

```sh
# add helm repo
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts
helm repo update
helm repo update open-telemetry

# check all versions 
helm search repo opentelemetry-operator
helm search repo open-telemetry/opentelemetry-operator  --versions

# download values file for some version
helm show values open-telemetry/opentelemetry-operator --version 0.83.1 > otel-operator-values.yaml
```

References:

- [GitHub reference](https://github.com/open-telemetry/opentelemetry-operator)
- [Artifact Hub reference](https://artifacthub.io/packages/helm/opentelemetry-helm/opentelemetry-operator)

## **Step 4: Install OpenTelemetry Operator**

To install the operator with default settings, use the following command:
```sh
helm install opentelemetry-operator open-telemetry/opentelemetry-operator `
--namespace opentelemetry-operator --create-namespace `
--set "manager.collectorImage.repository=otel/opentelemetry-collector-contrib"
# --version 3.4.1 otel\otel-operator-values.yaml
```



## **Step 5: Verify OpenTelemetry Operator Resources**


```sh
kubectl get pods -n opentelemetry-operator
kubectl get svc -n opentelemetry-operator
kubectl get all -n opentelemetry-operator
kubectl get all,secrets,configmap,ingress -n opentelemetry-operator
```

output
```sh
NAME                                          READY   STATUS    RESTARTS   AGE
pod/opentelemetry-operator-86d4f76d84-hbjsn   2/2     Running   0          19h

NAME                                     TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)             AGE
service/opentelemetry-operator           ClusterIP   10.25.240.34   <none>        8443/TCP,8080/TCP   19h
service/opentelemetry-operator-webhook   ClusterIP   10.25.44.68    <none>        443/TCP             19h

NAME                                     READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/opentelemetry-operator   1/1     1            1           19h

NAME                                                DESIRED   CURRENT   READY   AGE
replicaset.apps/opentelemetry-operator-86d4f76d84   1         1         1       19h

NAME                                                            TYPE                 DATA   AGE
secret/opentelemetry-operator-controller-manager-service-cert   kubernetes.io/tls    3      19h
secret/sh.helm.release.v1.opentelemetry-operator.v1             helm.sh/release.v1   1      19h

NAME                         DATA   AGE
configmap/kube-root-ca.crt   1      19h
```

## **Step 6: Port Forwarding Testing**

To interact with the OpenTelemetry operator locally, you can port-forward the operator's service to your local machine.

Find the operator’s service:

```sh
kubectl get svc -n opentelemetry-operator
```

```
kubectl port-forward service/opentelemetry-operator -n opentelemetry-operator 8080:8080
```

```sh
Forwarding from 127.0.0.1:8080 -> 8080
Forwarding from [::1]:8080 -> 8080
```

http://localhost:8080

```sh
404 page not found
```


```sh
kubectl port-forward service/opentelemetry-operator -n opentelemetry-operator 8443:8443
```

```sh
Forwarding from 127.0.0.1:8443 -> 8443
Forwarding from [::1]:8443 -> 8443
Handling connection for 8443
```


https://localhost:8443

```sh
Unauthorized
```

## **Reference**

- [https://github.com/open-telemetry/opentelemetry-operator/blob/main/README.md](https://github.com/open-telemetry/opentelemetry-operator/blob/main/README.md)
- [https://github.com/open-telemetry/opentelemetry-operator](https://github.com/open-telemetry/opentelemetry-operator)
- [https://artifacthub.io/packages/helm/opentelemetry-helm/opentelemetry-operator](https://artifacthub.io/packages/helm/opentelemetry-helm/opentelemetry-operator)
- [https://github.com/open-telemetry/opentelemetry-helm-charts](https://github.com/open-telemetry/opentelemetry-helm-charts)