Since we've already installed ArgoCD helm chart in AKS cluster, this AKS cluster is called "in-cluster" with URL - <https://kubernetes.default.svc>.

Argo CD can be run in two modes:

- **In-cluster mode** - where it is deployed as a Kubernetes Deployment in the same cluster that it manages.
- **External cluster mode** -where it is deployed outside the Kubernetes cluster that it manages.

in-cluster mode is the recommended way to run Argo CD as it provides better security, scalability, ease of management, and resilience. However, external cluster mode may be necessary in certain situations, such as managing multiple Kubernetes clusters or integrating with external systems.

For in-cluster we don't need to do any registrations but in order to deploy applications to an external Kubernetes cluster, you will need to register an external K8s cluster with Argo CD.

 Also, note that you will not able to register a new K8 cluster in the Argo CD web UI. You can only register a new K8s cluster from the Argo CD CLI. 
 
## Implementation Details

To register an Azure Kubernetes Service (AKS) cluster with Argo CD using the argocd CLI, follow these steps:

- First, you need to install the argocd CLI.
- Next, you need to log in to your Argo CD instance using the CLI.
```
argocd login --insecure --port-forward-namespace argocd 20.124.172.79
```
output
``` sh
Username: admin
Password: 
Password: 
'admin:login' logged in successfully
Context '20.124.172.79' updated
```
- Create a Kubernetes context for your AKS cluster using the Azure CLI. 
``` sh
az aks get-credentials -g "rg-aks-dev" -n "aks-cluster2-dev" --admin
```
- Use the argocd CLI to add your AKS cluster to Argo CD. 
``` sh
argocd cluster add aks-cluster2-dev
```
- After completing the previous steps you can run argocd cluster list again or go into the portal. You will see your new cluster added.
``` sh
argocd cluster list
```
## Reference
<!-- - <https://www.buchatech.com/2021/12/registering-a-kubernetes-cluster-with-argo-cd/> -->