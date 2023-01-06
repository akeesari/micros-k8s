Download and install following software's as per the need, you can use either `choco` tool (recommended) or direct install 

## Install Chocolatey

Download and Install using Chocolatey

`Chocolatey` has the largest online registry of Windows packages. Chocolatey packages encapsulate everything required to manage a particular piece of software into one deployment artifact by wrapping installers, executables, zips, and/or scripts into a compiled package file.
I’d strongly recommend using choco commands by searching required softwares instead of manually trying to install using the above links. 

**Choco commands** 

Use the following link and click on search button and start typing required software and copy the command into clipboard then open PowerShell window in admin mode to run the commands.

 https://community.chocolatey.org/

## Install VS code

```
choco install vscode
```

verify the installation
```
code --help
```

## Install SQL server

```
choco install sql-server-management-studio
```

## Install Chrome

```
choco install googlechrome
```

## Install Node JS

```
choco install nodejs
```
```
node --version
```

## Install Git

```
choco install git
```
```
git --version
```

## Install Docker

```
choco install docker-desktop
```

```
docker --version
```

## Install Azure CLI
used to create and manage Azure resources.

```
choco install azure-cli
```

```
az --version
```

## Install Terraform
used to create Azure resources for IaC.

```
choco install terraform
```

```
terraform --version
```

## Install Kubernetes CLI
used for interacting with kubernetes

```
choco install kubernetes-cli
```

## Install azure kubelogin

used for interacting with AKS cluster 

```
choco install azure-kubelogin
```

## Install Helm
used for interacting with kubernetes for helm

```
choco install kubernetes-helm
```

## Install pgadmin4
used for managing PostgreSQL databases

```
choco install pgadmin4
```

## Install Python
used for mkdocs

```
choco install python
```

## Install Pip
used for installing and managing Python packages

```
choco install pip
```

After install VS code, install the following extensions in vs code as per the need:

- Azure CLI Tools
- Azure Account
- Azure Kubernetes Services
- Azure Terraform
- C#
- Bridge to Kubernetes
- Dev Containers
- Docker
- Dotnet
- Helm Intelligence
- Kubernetes – very helpful for debugging services in AKS 
- PostgreSQL - get this from Microsoft 
- Remote development 
- Terraform 

After the installation quickly check to see the status of these installation by running version commands in PowerShell script or VS code terminal

For example:

```
PS C:\WINDOWS\system32> code --help
Visual Studio Code 1.71.2

PS C:\WINDOWS\system32> node --version
v14.16.0

PS C:\WINDOWS\system32> git --version
git version 2.29.2.windows.2

PS C:\WINDOWS\system32> docker --version
Docker version 20.10.17, build 100c701

PS C:\WINDOWS\system32> az --version
azure-cli                         2.38.0 *
…

PS C:\WINDOWS\system32> docker version
Client:
 Cloud integration: v1.0.24
 Version:           20.10.17
…

```

## Manual Install

Download and Install following Manually

- Visual studio code (recommended) - https://code.visualstudio.com/download/
- Visual studio (optional) - https://visualstudio.microsoft.com/downloads/
- SQL Server Management Studio (optional) - https://www.microsoft.com/en-us/sql-server/sql-server-downloads
- Notepad++ - https://notepad-plus-plus.org/downloads/
- Google Chrome – search in google for later versionf of Google Chrome and download & install it
- Node JS - https://nodejs.org/en/download/
- Git - https://git-scm.com/download/win
- Docker desktop - https://docs.docker.com/desktop/install/windows-install/


## Additional Software's

You may need these additional softwar to perform daily activities.

- Zoom - https://zoom.us/download
- Teams - https://www.microsoft.com/en-us/microsoft-teams/download-app
- WhatsApp - https://www.whatsapp.com/download
- SQL Search - https://www.red-gate.com/products/sql-development/sql-search/
- JSON viewer online - https://codebeautify.org/jsonviewer
- regexr validator - https://regexr.com/
- SAML Tracer [Browser extention](https://chrome.google.com/webstore/detail/saml-tracer/mpdajninpobndbfcldcmbpnnbhibjmch?hl=en)
- WSL - https://learn.microsoft.com/en-us/windows/wsl/install
- [Azure storage-explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)
- RDC - https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman
- base64encode - https://www.base64encode.org/