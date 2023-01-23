## Introduction

One of my goals is to simplify the development, deployment, and scaling of complex applications and to bring the full power of Kubernetes to all projects. DevOps pipeline provides a platform which allows all projects to develop, deploy and scale container-based applications, highly productive, yet flexible environment for developers. `Argo CD` is one of the tools introduced to reach the goals.

With Argo CD, developers can create application manifests and store them in a Git repository. They can then use Argo CD to deploy those manifests to a Kubernetes cluster, and it will continuously monitor and reconcile the live environment with the desired state defined in Git.

Argo CD is a declarative and GitOps-based tool that provides a few features like:

- Rollback and history of all changes
- Role-based access control
- Infrastructure as code
- Automated and manual deployment
- Automated error detection and recovery
  
It is designed to work seamlessly with other popular Kubernetes tools such as Helm and Kustomize and can be used to automate the deployment of complex, multi-tier applications across multiple clusters and namespaces.

## Objective

In this exercise we will accomplish & learn following:

- What is Argo CD?
- How it works?
- Why do we need to use Argo CD?
- Scenarios to use Argo CD
- Key Features
- Argo CD Architecture


## What is Argo CD?

Argo CD is a declarative, GitOps continuous delivery tool for Azure Kubernetes Cluster deployments.


## How it works?


Argo CD follows the GitOps pattern of using Git repositories as the source of truth for defining the desired application state. Kubernetes manifests can be specified in several ways:

- kustomize applications
- helm charts
- Plain directory of YAML/json manifests

![image.jpg](images/image-1.png)
<!-- ![image.png](/.attachments/image-abc39e0e-6ef7-4d7b-b6b9-f45e8cb3238b.png) -->


- It solves the deployment problems in the CD process by being a more integral part of the Kubernetes cluster. Instead of pushing changes to the Kubernetes cluster, Argo CD pulls Kubernetes manifest changes and applies them to the cluster. 

- Once you set up Argo CD inside of your Kubernetes cluster, you configure Argo CD to connect and track your Git repo changes.

- If any changes are detected, then Argo CD applies those changes automatically to the cluster. 

- Now `developers` can commit code in azure devops git repos, which will automatically build a new image, push it to the azure container registry (docker repo), and then finally update the Kubernetes manifest file that will be automatically pulled by Argo CD, ultimately saving manual work, reducing the initial setup configuration, and eliminating security risks.

- But what about `DevOps teams` making other changes to the application configuration? Whatever manifest files connected to the Git repo will be tracked and synced by Argo CD and pulled into the Kubernetes cluster, providing a single flexible deployment tool for developers and DevOps.

- Argo CD watches the changes in the cluster as well, which becomes important if someone updates the cluster manually. 

- Argo CD will detect the divergence of states between the cluster and Git repo. Argo CD compares desired configuration in the Git repo with the actual state in the Kubernetes cluster and syncs what is defined in the Git repo to the cluster, overriding whatever update was done manually—guaranteeing that the Git repo remains the single source of truth. But, of course, Argo CD is flexible enough to be configured to not automatically override manual updates if a quick update needs to happen directly to the cluster and an alert be sent instead.

## Why do we need to use Argo CD?

ArgoCD is an easy-to-use tool that allows development teams to deploy and manage applications without having to learn a lot about Kubernetes, and without needing full access to the Kubernetes system

<!-- **Scenarios to use Argo CD**

- Scenario-1
- Scenario-2 -->

## Key Features

- Automated deployment of applications to specified target environments

- Support for multiple config management/templating tools (Kustomize, Helm, plain-YAML)

- Ability to manage and deploy to multiple clusters

- SSO Integration ( OAuth2, LDAP, SAML 2.0, Microsoft)

- Multi-tenancy and RBAC policies for authorization

- Rollback/Roll-anywhere to any application configuration committed in Git repository

- Health status analysis of application resources

- Automated configuration drift detection and visualization

- Automated or manual syncing of applications to its desired state

- Web UI which provides real-time view of application activity

- CLI for automation and CI integration

- Webhook integration with Azure DevOps

- Access tokens for automation

- PreSync, Sync, PostSync hooks to support complex application rollouts (e.g.blue/green & canary upgrades)


<!-- # Architecture diagram


1. API Server: 
1. Repository Server: 
1. Application Controller: 

![image.png](/.attachments/image-48645e55-40cf-4177-a0f9-6dd32538ca11.png) -->

# References
- https://www.youtube.com/watch?v=aWDIQMbp1cc&t=64s
- https://mohitgoyal.co/2021/04/30/declarative-gitops-continuous-deployment-for-application-with-kubernetes-argo-cd-helm-kustomize-and-kind-index/ - excellent
- https://github.com/goyalmohit/argocd-example-apps/tree/master/apps - Good examples