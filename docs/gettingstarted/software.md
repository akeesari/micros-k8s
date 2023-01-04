Download and install following software's as per the need, you can use either choco tool (recommended) or direct install 

# Windows OS

## Manual

Download and Install Manual

- Visual studio code (recommended) - https://code.visualstudio.com/download
- Visual studio (optional) - https://visualstudio.microsoft.com/downloads/
- SQL Server Management Studio (optional) - https://www.microsoft.com/en-us/sql-server/sql-server-downloads
- Notepad++ - https://notepad-plus-plus.org/downloads/
- Google Chrome – search in google for download and install
- Node JS - https://nodejs.org/en/download/
- Git - https://git-scm.com/download/win
- Docker desktop - https://docs.docker.com/desktop/install/windows-install/

## Chocolatey

Download and Install using Chocolatey

`Chocolatey` has the largest online registry of Windows packages. Chocolatey packages encapsulate everything required to manage a particular piece of software into one deployment artifact by wrapping installers, executables, zips, and/or scripts into a compiled package file.
I’d strongly recommend using choco commands by searching required softwares instead of manually trying to install using the above links. 

**Choco commands** 

Use the following link and click on search button and start typing required softwares and copy into clipboard, open PowerShell window in admin mode to run these commands

https://community.chocolatey.org/

- choco install vscode
- choco install sql-server-management-studio
- choco install googlechrome
- choco install nodejs
- choco install git
- choco install docker-desktop
- choco install azure-cli
- choco install kubernetes-cli  - Kubernetes Command Line Interface (CLI) 
- choco install azure-kubelogin -  kubelogin is a command-line utility implementing azure authentication into your AKS clusters.
- choco install kubernetes-helm – 
- choco install pgadmin4


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
- PostgreSQL from Microsoft 
- Remote development 
- Terraform 

Quickly check to see the status of these installation by running version commands in PowerShell script or VS code terminal


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
## Additional Software's

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



# Mac OS

## Homebrew 
Homebrew is a free and open-source software package management system that simplifies the installation of software on Apple's operating system, macOS, as well as Linux. 

contents from this video might be helpful before configuring your Mac OS development machine.

https://www.youtube.com/watch?v=0MiGnwPdNGE - How I setup the Terminal on my M1 Max MacBook Pro

## Install homebrew
Install `homebrew` before installing anything - https://brew.sh/

homebrew is like a choco
homebrew = cocho

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
output

``` 
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/anjikeesari/.zprofile   
eval "$(/opt/homebrew/bin/brew shellenv)"  
```
``` brew upgrade ```

## install terminal 

```brew install --cask iterm2```

## install powershell

``` 
# install
brew install --cask powershell

# upgrade
brew upgrade --cask powershell
```

## install vs code

```
brew install --cask visual-studio-code
```

## install azure-cli

```
brew install azure-cli
```

## install kubectl cli 

https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/

```
brew install kubectl
```

## install dotnet
```
brew install --cask dotnet
```

## install python3
```
brew install python3
python3 --version
```

# install iterm2
```
brew install iterm2 
```

## install git 
brew install git  
git --version    

## install docker
```
docker --version
brew install nvm   
nvm install 16
```
## install node

```
brew install node
node --version  
npm version
```

## install zoom
```
brew install --cask zoom
```

##install teams

```
brew install --cask microsoft-teams
```
## uninstall teams

it will ask the Mac login password for any software uninstall.

```
brew uninstall microsoft-teams
```
## Install .NET 7 SDK for Mac

https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-7.0.100-macos-x64-installer?journey=vs-code

## install-docker
https://dotnet.microsoft.com/en-us/learn/aspnet/microservice-tutorial/install-docker
https://docs.docker.com/desktop/install/mac-install/

## install github 
```
brew install gh
```
Here is the list of software's you may need during the development.
```
1. Zsh 
2. Homebrew - Mac
2. choco - Mac
3. Chrome 
3. Alfred 
4. Iterm2 
5. Git 
6. Pycharm 
5. Visual Studio Code 
6. Vim 
7. Extentions a. Prettier b. Bracket Pair Colorizer c. Live Share 
8. Slack 
9. Zoom 
10. Droplr 
11. Loom 
12. Virtual Desktop 13. Spectical
```