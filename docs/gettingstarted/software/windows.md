# Download & install Software in Windows OS

Download and install following software as per the need, you can use either `choco` tool (recommended) or direct install 

Note: - Restart your computer when prompted or needed.

## Install Chocolatey

We are going to use `choco` commands for installing all required software and developer tools. 

**What is Chocolatey?**

Chocolatey is a package manager for Windows that allows you to install, upgrade, and manage software packages from the command line. It is similar to package managers on Linux systems, such as apt or yum, and it can be used to install a wide range of software applications and libraries on Windows.

it is strongly recommend using choco commands by searching required software instead of manually trying to install using the above links. 

To use Chocolatey, you will need to have Windows PowerShell installed on your system. You can then install Chocolatey using a PowerShell command, and use it to install and manage packages from the command line.

**Install Chocolatey**

This is the first thing we need to install before installing anything in the Windows OS.

To install Chocolatey on your Windows system, you will need to open a terminal window (such as PowerShell or Command Prompt) and run the following command:

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

```
reference - [https://docs.chocolatey.org/en-us/choco/setup](https://docs.chocolatey.org/en-us/choco/setup){:target="_blank"}

Once the installation is finished, you can use the `choco` command to install, upgrade, and manage software packages using Chocolatey.

**Search choco commands** 

Use the following link and click on search button and start typing required software and copy the command into clipboard then open PowerShell window in admin mode to run the commands.

 [https://community.chocolatey.org/](https://community.chocolatey.org/){:target="_blank"}

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

## Install Kubernetes CLI or kubectl CLI
used for interacting with kubernetes

```
choco install kubernetes-cli
```
look into this for more info - <https://kubernetes.io/docs/tasks/tools/install-kubectl-windows/>

```
# Test to ensure the version you installed is up-to-date:
kubectl version
kubectl version --client

# use this for detailed view of version:
kubectl version --client --output=yaml

or 
kubectl version --output=json

#Verify kubectl configuration
kubectl cluster-info

```

## Install azure kubelogin

used for interacting with AKS cluster 

```
choco install azure-kubelogin
```

## Install Helm
used for interacting with Kubernetes for helm

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
## Install WSL
Windows Subsystem for Linux (WSL) is a feature of Windows that allows developers to run a Linux environment without the need for a separate virtual machine or dual booting. 

```
choco install wsl
```

## Install JQ
 JSON processor.

```
choco install jq
```

## Windows Terminal 

Allows us to access multiple command-line tools and shells in one customizable interface. It is an open-source project developed and maintained by Microsoft.

```
choco install microsoft-windows-terminal
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

If you would prefer to install manually then here is the same list of software you can download and Install them manually

- Visual studio code (recommended) - <https://code.visualstudio.com/download/>
- Visual studio (optional) - <https://visualstudio.microsoft.com/downloads/>
- SQL Server Management Studio (optional) - <https://www.microsoft.com/en-us/sql-server/sql-server-downloads>
- Notepad++ - <https://notepad-plus-plus.org/downloads/>
- Google Chrome – search in google for later version of Google Chrome and download & install it
- Node JS - <https://nodejs.org/en/download/>
- Git - <https://git-scm.com/download/win>
- Docker desktop - <https://docs.docker.com/desktop/install/windows-install/>


## Additional Software

You may need these additional software to perform daily activities.

- Zoom - <https://zoom.us/download>
- Teams - <https://www.microsoft.com/en-us/microsoft-teams/download-app>
- WhatsApp - <https://www.whatsapp.com/download>
- SQL Search - <https://www.red-gate.com/products/sql-development/sql-search/>
- JSON viewer online - <https://codebeautify.org/jsonviewer>
- regexr validator - <https://regexr.com/>
- SAML Tracer [Browser extention](https://chrome.google.com/webstore/detail/saml-tracer/mpdajninpobndbfcldcmbpnnbhibjmch?hl=en)
- WSL - https://learn.microsoft.com/en-us/windows/wsl/install>
- [Azure storage-explorer](https://azure.microsoft.com/en-us/products/storage/storage-explorer/)
- RDC - <https://learn.microsoft.com/en-us/sysinternals/downloads/rdcman>
- base64encode - <https://www.base64encode.org/>