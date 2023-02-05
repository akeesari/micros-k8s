## Introduction

A resource provider in Azure is a service that supplies a specific type of resource, such as virtual machines, storage accounts, or virtual networks. When you create a new Azure subscription, certain resource providers are automatically registered, but others may need to be registered manually.

## Register manually

To view the registered resource providers for an Azure subscription, you can follow these steps:

1. Sign in to the Azure portal (<https://portal.azure.com/>) with your Azure account.

1. In the top-left corner of the Azure portal, click on the "Subscriptions" link.

1. Select the subscription that you want to view the registered resource providers for.

1. On the subscription page, click on the "Resource providers" link on the left-hand menu.

1. The "Resource providers" page will show a list of all the resource providers that are currently registered for the selected subscription, as well as their registration status.

1. You can filter the list by typing the name of the resource provider in the search bar.

If you want to register a new resource provider for the subscription, click on the "+ Register" button on the top of the page, and then select the resource provider you want to register.

This manual register takes lot of time especially if you've to do the same for multiple subscriptions for supporting different environment.

In this lab you will see how to register resource providers in azure subscription using PowerShell.

**log in to Azure account & select the subscription**

```
az login

az account list
or
az account list --output table

az account set -s "anji.keesari"

az account show
or
az account show --output table
```

**List of all resource providers**

Use this command to see all resource providers in Azure, and the registration status for your subscription.

```
az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
```


Sample Output

```
Provider                                   Status
-----------------------------------------  -------------
Microsoft.AlertsManagement                 Registered
Microsoft.Cache                            Registered
Microsoft.Web                              Registered
Microsoft.ApiManagement                    Registered
Microsoft.Network                          Registered
microsoft.insights                         Registered
Microsoft.ResourceHealth                   Registered
Microsoft.ContainerRegistry                Registered
Dynatrace.Observability                    NotRegistered
Microsoft.AAD                              NotRegistered
microsoft.aadiam                           NotRegistered
Microsoft.Addons                           NotRegistered
Microsoft.ADHybridHealthService            Registered
Microsoft.AgFoodPlatform                   NotRegistered
Microsoft.AnalysisServices                 NotRegistered
Microsoft.AnyBuild                         NotRegistered
Microsoft.ApiSecurity                      NotRegistered
```


**registered resource providers only**

Use this command to see registered resource providers only in Azure.

```
az provider list --query "sort_by([?registrationState=='Registered'].{Provider:namespace, Status:registrationState}, &Provider)" --out table
```

Sample Output

```
Microsoft.ADHybridHealthService     Registered
Microsoft.AVS                       Registered
Microsoft.Advisor                   Registered
Microsoft.AlertsManagement          Registered
Microsoft.ApiManagement             Registered
```
## Register single provider example

Get Resource Provider

```
Connect-AzAccount
Get-AzResourceProvider -ProviderNamespace Microsoft.Addons
```
Output

```
ProviderNamespace : Microsoft.Addons
RegistrationState : NotRegistered
ResourceTypes     : {supportProviders}
Locations         : {West Central US, South Central US, East US, West Europe}

ProviderNamespace : Microsoft.Addons
RegistrationState : NotRegistered
ResourceTypes     : {operations}
Locations         : {West Central US, South Central US, East US, West Europe}

ProviderNamespace : Microsoft.Addons
RegistrationState : NotRegistered
ResourceTypes     : {operationResults}
Locations         : {West Central US, South Central US, East US, West Europe}
```

Register Resource Provider

```
Register-AzResourceProvider -ProviderNamespace Microsoft.Addons
```
Output
```
ProviderNamespace : Microsoft.Addons
RegistrationState : Registering
ResourceTypes     : {supportProviders, operations, operationResults}
Locations         : {West Central US, South Central US, East US, West Europe}
```
Status After register

```
# command
Get-AzResourceProvider -ProviderNamespace Microsoft.Addons

# output
ProviderNamespace : Microsoft.Addons
RegistrationState : Registered
ResourceTypes     : {supportProviders}
Locations         : {West Central US, South Central US, East US, West Europe}

ProviderNamespace : Microsoft.Addons
RegistrationState : Registered
ResourceTypes     : {operations}
Locations         : {West Central US, South Central US, East US, West Europe}

ProviderNamespace : Microsoft.Addons
RegistrationState : Registered
ResourceTypes     : {operationResults}
Locations         : {West Central US, South Central US, East US, West Europe}

```

## Register using PowerShell

This PowerShell script helps you obtain a list of “Resource Providers” from an existing subscription, and registers them to the new subscription.

follow these instruction before running this script:

1. Update source and target subscriptionId 
1. Uncomment #-WhatIf:$WhatIf for testing before actually registering.
1. Once everything is fine then comment and run the script to register providers


``` ps1 title="resource-providers.ps1"

# Register missing resource providers in a new subscription based on a list from an old subscription

Set-AzContext -SubscriptionId '33dffc83-ffec-4773-b939-fc5be0d00a558' #old-subscription
$ExistingResourceProvidersInOldSubscription = Get-AzResourceProvider


Set-AzContext -SubscriptionId 'sfafed0f-0e40-43c6-8ccb-f5a5807211222' #new-subscription
$ExistingResourceProvidersInNewSubscription = Get-AzResourceProvider


foreach($ExistingResourceProviderInOldSubscription in $ExistingResourceProvidersInOldSubscription) {
    if($ExistingResourceProviderInOldSubscription.RegistrationState -eq 'Registered'){
        $providerfound = $false;
        foreach($ExistingResourceProviderInNewSubscription in $ExistingResourceProvidersInNewSubscription){
                if($ExistingResourceProviderInNewSubscription.ProviderNamespace -eq $ExistingResourceProviderInOldSubscription.ProviderNamespace){
                    $providerfound = $true;
                    if($ExistingResourceProviderInNewSubscription.RegistrationState -eq 'Registered'){
                        # Write-Host '------------------------------'
                        Write-Host $ExistingResourceProviderInOldSubscription.ProviderNamespace 'PRESENT, REGISTERED'
                        # Write-Host '------------------------------'
                    }
                    else{
                        Write-Host $ExistingResourceProviderInOldSubscription.ProviderNamespace 'PRESENT, UNREGISTERED, Registering'
                        Register-AzResourceProvider -ProviderNamespace $ExistingResourceProviderInOldSubscription.ProviderNamespace #-WhatIf:$WhatIf
                    }                        
                }
            }
        if(-Not($providerfound)){
            Write-Host $ExistingResourceProviderInOldSubscription.ProviderNamespace 'ABSENT, Registering'
            Register-AzResourceProvider -ProviderNamespace $ExistingResourceProviderInOldSubscription.ProviderNamespace #-WhatIf:$WhatIf
        }
    }
}

```

Output

```
PS C:\Source\Repos\infrastructure-as-code> .\resource-providers.ps1
.
.
.
.
Microsoft.Diagnostics ABSENT, Registering
What if: Performing the operation "Registering provider ..." on target "Microsoft.Diagnostics".
Microsoft.Advisor ABSENT, Registering
What if: Performing the operation "Registering provider ..." on target "Microsoft.Advisor".
Microsoft.AAD ABSENT, Registering
What if: Performing the operation "Registering provider ..." on target "Microsoft.AAD".
Microsoft.DevOps ABSENT, Registering
```