
## Introduction
This repository contains documentation on **Building and Managing Microservices with Kubernetes using Argocd & Helm**, this documentation will be helful for the developers who is working on Microservices Architecture and deploying in Azure Kubernetes services (AKS).

This documentation is built using markdown language, MkDocs and mkdocs-material.

## Prerequisites

- Visual Studio code
- Create new repo called `mkdocs` in GitHub
- Clone the repo locally

In order to run this project locally your need to install following:

## Download and install Python3

```
brew install python3
python3 --version
```

## Download and installing pip

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
pip3 --version
pip --version
```

you can find more info here- https://www.geeksforgeeks.org/how-to-install-pip-in-macos/

## Install mkdocs-material
 
```
pip install mkdocs-material
```

you can find more info here - https://squidfunk.github.io/mkdocs-material/getting-started/
   
## Project setup

You need to run following bash script to install required software to run this project locally.

```
bash ./init_setup.sh
```

## Running the site locally

Execute the following command to run the site locally with Localhost URL.

```
mkdocs serve 
```

output
```
INFO     -  Building documentation...
INFO     -  Cleaning site directory
INFO     -  Documentation built in 1.44 seconds
INFO     -  [19:20:51] Watching paths for changes: 'docs', 'mkdocs.yml'
INFO     -  [19:20:51] Serving on http://127.0.0.1:8000/mkdocs/
```

browse the site with following URL
http://127.0.0.1:8000/mkdocs/


## References
- https://www.youtube.com/watch?v=OOxL-r1L334&t=958s - YouTube video, helped me to create the mkdocs site.
- https://www.markdownguide.org/ - markdown language
- https://squidfunk.github.io/mkdocs-material/ - Material for MkDocs
- https://squidfunk.github.io/mkdocs-material/getting-started/ - getting-started, used for project setup
- https://mermaid-js.github.io/mermaid/#/ - mermaid 