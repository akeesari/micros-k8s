#Introduction

`az acr` commands used for managing private registries with Azure Container Registries.

This page contains a list of commonly used `az acr` commands.

Note: make sure that you login into azure and select the azure subscription before running any `az acr` commands.

## Azure login

```
az login
az account list --output table
```
## Select the subscription

```
az account set -s "anji.keesari"
az account show --output table
```

## Connect to Container Registry
```
az acr login --name acrname
```
output
```
Login Succeeded
```

## To get the login server address
```
az acr list -g "rg-acrname-dev" --query "[].{acrLoginServer:loginServer}" --output table
```

output

```
AcrLoginServer
-----------------------
acrname.azurecr.io
```

## List registries

Lists all the container registries under the current subscription.

```
az acr repository list --name acrname --output table
```

output
```
Result
----------------------------------
sample/aspnet-api
sample/aspnet-app
sample/react-app
```
## show tags 
show tags of a image in the acr

```
az acr repository show-tags --name acrname --repository sample/aspnet-api --output table

```
output

```
Result
----------
20220823.3
20220823.4
```

## check-health
```
az acr check-health -n "acrname" -y
```

## helm list
List all helm charts in an Azure Container Registry.
```
az acr helm list -n 'acrname'
```

## References
- <https://learn.microsoft.com/en-us/cli/azure/acr?view=azure-cli-latest>
