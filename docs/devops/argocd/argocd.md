## Introduction

One of my goals is to simplify the development, deployment, and scaling of complex applications and to bring the full power of Kubernetes to all projects. 

`Argo CD` is one of the tools introduced to reach the goals.

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

Argo CD is a GitOps-based continuous delivery tool for Kubernetes applications. It helps to manage and synchronize application deployments with the desired state specified in the Git repository. Argo CD allows developers to version control their application configuration and use Git as a single source of truth for deployments, ensuring that the desired state of the applications in the cluster always matches the desired state defined in Git.

## How it works?


Argo CD follows the GitOps pattern of using Git repositories as the source of truth for defining the desired application state. Kubernetes manifests can be specified in several ways:

- kustomize applications
- helm charts
- Plain directory of YAML/json manifests

![image.jpg](images/image-1.png)
<!-- ![image.png](/.attachments/image-abc39e0e-6ef7-4d7b-b6b9-f45e8cb3238b.png) -->

Argo CD works by continuously monitoring the desired state of applications defined in Git, and comparing that to the actual state of the applications in a target Kubernetes cluster. It uses this comparison to perform automated sync operations that bring the live cluster into the desired state.

Here is a high-level overview of the steps involved in using Argo CD:

1. Define the desired state of your applications in a Git repository.
1. Connect Argo CD to the Git repository and target Kubernetes cluster.
1. Create applications in Argo CD, which are a mapping between the desired state in Git and the target cluster.
1. Argo CD continuously monitors the state of the applications and compares it to the desired state in Git.
1. When a difference is detected, Argo CD performs a sync operation, which updates the target cluster to match the desired state in Git.
1. Argo CD provides a user interface that allows you to view the current state of your applications, perform manual sync operations, and track changes over time.

This GitOps-based approach provides a reliable and consistent way to manage and deploy applications to a Kubernetes cluster, helping to eliminate inconsistencies and minimize errors.


## Why do we need to use Argo CD?

Here are several reasons why one might choose to use Argo CD:

1. `Consistency and reliability:` Argo CD provides a single source of truth for your application deployments, ensuring that the desired state of your applications in the cluster always matches the desired state defined in Git.
1. `Automation:` Argo CD automates the process of deploying and updating applications, reducing the chance of human error and freeing up time for developers to focus on other tasks.
1. `Version control:` Argo CD integrates with Git, allowing you to version control your application configurations and track changes over time.
1. `Collaboration:` Argo CD makes it easier for teams to collaborate on application deployments by providing a common, shared repository for defining the desired state of applications.
1. `Rollback:` Argo CD provides a way to quickly revert to previous versions of an application if something goes wrong during an update.
1. `Scalability:` Argo CD can scale to manage multiple applications and multiple clusters, making it easier to manage large-scale deployments.

By using Argo CD, organizations can ensure consistent, reliable, and automated deployments of their applications to a Kubernetes cluster.

<!-- **Scenarios to use Argo CD**

- Scenario-1
- Scenario-2 -->

## Key Features of argocd

Argo CD is a continuous delivery tool for Kubernetes applications that offers several key features:

1. `GitOps:` Argo CD uses Git as the single source of truth for your application deployments, ensuring that the desired state of your applications in the cluster always matches the desired state defined in Git.
1. `Continuous monitoring and synchronization:` Argo CD continuously monitors the state of your applications and compares it to the desired state in Git. When a difference is detected, Argo CD performs a sync operation to bring the live cluster into the desired state.
1. `User interface:` Argo CD provides a user interface that allows you to view the current state of your applications, perform manual sync operations, and track changes over time.
1. `Role-based access control:` Argo CD allows you to control who can access and make changes to your applications, making it easier to manage deployments in large organizations.
1. `Rollback:` Argo CD provides a way to quickly revert to previous versions of an application if something goes wrong during an update.
Custom resource validation: Argo CD can validate the custom resource definitions used in your applications, ensuring that your applications are deployed according to your desired state.

1. `Multi-cluster support:` Argo CD can manage applications across multiple Kubernetes clusters, making it easier to manage large-scale deployments.

These features make Argo CD a powerful tool for managing and deploying applications in a Kubernetes cluster in a GitOps-based manner.

## Argo CD architecture

Argo CD architecture consists of the following components:

1. `Git repository:` The Git repository contains the desired state of your applications, which is used as the source of truth for deployments.
1. `Argo CD server:` The Argo CD server is the main component that connects to the Git repository and target Kubernetes cluster. It performs continuous monitoring and synchronization of applications, and provides a user interface for managing deployments.
1. `Kubernetes cluster:` The target Kubernetes cluster is where your applications are deployed and run. The Argo CD server continuously compares the state of your applications in the cluster to the desired state defined in Git.
1. `Argo CD CLI:` The Argo CD CLI is a command-line interface for interacting with the Argo CD server, allowing you to perform manual sync operations and manage applications from the command line.
1. `Argo CD API:` The Argo CD API provides a REST API for programmatically interacting with the Argo CD server, allowing you to automate tasks and integrate Argo CD into your existing DevOps workflow.

The Argo CD server and the target Kubernetes cluster communicate using the Kubernetes API, allowing Argo CD to manage and deploy applications in the cluster. The Argo CD server runs as a set of Kubernetes resources in the target cluster,

providing a scalable and highly available solution for managing deployments.

The architecture of Argo CD is designed to provide a reliable and consistent way to manage and deploy applications to a Kubernetes cluster, while allowing for flexible integration into existing DevOps workflows and tools.

<!-- # Architecture diagram


1. API Server: 
1. Repository Server: 
1. Application Controller: 

![image.png](/.attachments/image-48645e55-40cf-4177-a0f9-6dd32538ca11.png) -->

## References
- <https://argo-cd.readthedocs.io/en/stable/>
<!-- - <https://www.youtube.com/watch?v=aWDIQMbp1cc&t=64s>
- <https://mohitgoyal.co/2021/04/30/>declarative-gitops-continuous-deployment-for-application-with-kubernetes-argo-cd-helm-kustomize-and-kind-index/> - excellent
- <https://github.com/goyalmohit/argocd-example-apps/tree/master/apps> - Good examples -->