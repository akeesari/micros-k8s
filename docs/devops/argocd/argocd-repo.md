## Introduction

**Argo CD repository explained**

Argo CD repository is a Git repository that contains Kubernetes manifests for the applications you want to deploy with Argo CD. The repository can be either public or private and can be hosted on various Git platforms such as Azure DevOps, GitHub, GitLab, Bitbucket, etc. Argo CD uses GitOps methodology to manage the deployment of applications to a Kubernetes cluster. With GitOps, the desired state of the applications is defined in Git repositories and Argo CD constantly monitors the repositories to ensure that the deployed applications are in sync with the desired state.

In order to use Argo CD with a Git repository, you need to configure the repository as a source of truth for your applications. You can do this by adding the repository as a source in Argo CD's web interface or CLI. Argo CD can then pull the latest changes from the repository and apply them to the Kubernetes cluster, ensuring that the cluster is always in sync with the desired state defined in the repository.

Argo CD supports various types of Kubernetes manifests including YAML, JSON, and Helm charts. You can organize your manifests into different directories or subdirectories within the repository to group your applications logically. You can also use tools like Kustomize to manage overlays and patches for your manifests.

Argo CD repository is an essential component of the Argo CD deployment pipeline. By using Git repositories as a source of truth, you can ensure that the deployment of applications is fully auditable, version-controlled, and repeatable. This provides a consistent and reliable way to manage deployments in a Kubernetes cluster.


## Implementation Details

In this exercise we will accomplish & learn how to implement following:

- Create a new repo in ArgoCD

## Prerequisites

- login to Azure
- Connect to AKS cluster
- Login to argocd
- Login to azure devops
 
 
To configure a new repository as a source for Argo CD, follow these steps:

1. Log in to the Argo CD web interface.
1. Click on the "Settings" icon in the left sidebar.
1. Select "Repositories" from the dropdown menu.
1. Click the "New Repository" button.
1. Enter a name for the repository and the URL to the Git repository.
1. Specify the type of the repository: Git or Helm.
1. (Optional) Set up SSH or HTTPS access to the repository.
1. To use SSH access, you can add the SSH key to the repository using the Git platform's interface. Then, in Argo CD, choose "SSH Private Key" as the authentication type and enter the SSH private key.
1. To use HTTPS access, you can enter the username and password for the repository in Argo CD, or you can set up a personal access token (PAT) and enter that instead.
1. (Optional) Set up credentials for the repository if it requires authentication.
1. If your repository requires authentication, you can enter the username and password or the PAT in the repository settings.
1. (Optional) Enable automatic synchronization of the repository.
1. If you want Argo CD to automatically synchronize the repository with the cluster when changes are made, you can enable automatic synchronization.
1. Click "Create" to create the new repository.

In this lab we are going to use (step-11) PAT of the Azure DevOps to connection to private Git repo.

**Steps to create personal access token from azure devops**

To create a personal access token (PAT) in Azure DevOps, follow these steps:

- Go to your Azure DevOps organization or project and click on your profile picture on the top right corner.

- Select "Security" from the dropdown menu.

- Click on "Personal access tokens" and then "New Token".

- Choose a name for your token and select the appropriate organization and project.

- Select the desired scope for the token by checking the boxes next to the required permissions.

- Choose an expiration date or select "Never" if you want the token to never expire.

- Click "Create" to generate the token.

- Your token will be displayed on the screen. Copy the token and store it in a secure place.

Note: Be sure to keep your token safe and never share it with anyone else. You can use this token to authenticate your requests when accessing Azure DevOps APIs or when running command-line tools.

After creation the PAT fill the following details 

- Repository URL - copy & paste azure DevOps URL
- Username - leave this empty for PAT scenario
- Password- copy & paste the personal access token (PAT) from the azure devops

Once the repository is configured as a source for Argo CD, you can use it to manage and deploy applications. You can add applications to the repository by either connecting to the Git repository or adding them manually. Argo CD will automatically detect changes in the repository and trigger a deployment when a new version of an application is available. You can also manually trigger deployments using the Argo CD web UI or CLI.


## Reference
- <https://argo-cd.readthedocs.io/en/stable/user-guide/private-repositories/>