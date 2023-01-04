
## Introduction
This repo contains documentation on **Building and Managing Microservices with Kubernetes using Argocd & Helm**, it is kind of ebook for developers who is working on Microservices Architecture and deploying in Azure Kubernetes services (AKS).

This documentation is built using markdown language and mkdocs-material.

## Prerequisites

- Visual Studio code
- Create new repo called `mkdocs` in GitHub
- Clone the repo locally

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

run following bash script to install required softwares before running the site.

```
bash ./init_setup.sh
```

## Running the site locally

run following commands to run the site locally.

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
- https://docs.github.com/en/repositories/- 