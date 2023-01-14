If you are a Mac developer, download and install following software as per the need, open the terminal and install software using Homebrew command.


## What is Homebrew?

Homebrew is a free and open-source software package management system that simplifies the installation of software on Apple's operating system, macOS, as well as Linux. 

## Install homebrew

This is the fist software you may need to install before installing anything in Mac. for more infor look into this - https://brew.sh/

homebrew is like a choco

homebrew (for Mac users) = cocho (for Windows users)

To use Homebrew, you will need to have a terminal window open and install Homebrew on your system. To install Homebrew, you can copy and paste the following command into the terminal:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

output

``` 
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/anjikeesari/.zprofile   
eval "$(/opt/homebrew/bin/brew shellenv)"  
```
upgrade brew

``` 
brew upgrade 
```

Once the installation is finished, you can use the `brew` command to install, upgrade, and manage software packages using Homebrew.

## install terminal 

```
brew install --cask iterm2
```

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

look into this for more info - https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/

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

## install iterm2

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

## install teams

```
# install
brew install --cask microsoft-teams

# uninstall teams, it will ask the Mac login password for any software uninstall.
brew uninstall microsoft-teams
```
## Install .NET 7 SDK for Mac

use the following linlk for download and install it manually.

https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/sdk-7.0.100-macos-x64-installer?journey=vs-code

## install-docker

follow the instruction to install docker.

https://dotnet.microsoft.com/en-us/learn/aspnet/microservice-tutorial/install-docker
https://docs.docker.com/desktop/install/mac-install/

## install github

```
brew install gh
```

## install jq

```
brew install jq
```

Here is the list of other software you may need during the development.

```
1. Zsh 
2. Homebrew - Mac
2. choco - windows
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

## Reference

- https://www.youtube.com/watch?v=0MiGnwPdNGE - How I setup the Terminal on my M1 Max MacBook Pro