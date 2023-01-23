<!-- # Chapter 3: Azure DevOps -->

## Introduction:

One of my goals is to simplify the development, deployment, and scaling of complex applications and to bring the full power of Kubernetes to all projects. Organization’s devops pipeline provides a platform which allows all projects to develop, deploy and scale container-based applications, highly productive, yet flexible environment for developers.

Azure DevOps Pipelines is a cloud-based continuous integration and continuous delivery (CI/CD) service that allows you to automate the building, testing, and deployment of your code. It is a part of Azure DevOps, a set of tools for software development teams to plan, collaborate, and ship code.

The tools we introduced to meet the goals:

## Helm Charts

One of the key tools we use from the Kubernetes ecosystem is Helm. Helm is a package manager for Kubernetes, which simplifies the process of installing and managing applications on a Kubernetes cluster. A Helm chart is a collection of files that define how an application should be installed and configured on a Kubernetes cluster.

## Argo CD

Argo CD is a GitOps-based continuous delivery tool for Kubernetes. It is an open-source tool that allows you to declaratively manage and deploy applications on Kubernetes clusters.

The following diagram shows the end-to-end CI/CD process

## CI & CD Architecture

<IMG  src="https://learn.microsoft.com/en-us/azure/architecture/microservices/images/aks-cicd-flow.png"  alt="CD/CD pipeline"/>


## Reference

- https://learn.microsoft.com/en-us/azure/architecture/microservices/ci-cd-kubernetes - Build a CI/CD pipeline for microservices on Kubernetes with Azure DevOps and Helm
