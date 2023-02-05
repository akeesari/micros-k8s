## Code block with numbers

``` py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```


## Code block python
``` python
import tensorflow as tf
```

## Code block with title

``` py title="bubble_sort.py"
def bubble_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```
## Code block c#


``` c#
using System;

namespace HelloWorld
{
  class Program
  {
    static void Main(string[] args)
    {
      Console.WriteLine("Hello World!");    
    }
  }
}
```
## Code block Terraform

``` tf
resource "azurerm_network_security_group" "example" {
  name                = "example-security-group"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
}

resource "azurerm_virtual_network" "example" {
  name                = "example-network"
  location            = azurerm_resource_group.example.location
  resource_group_name = azurerm_resource_group.example.name
  address_space       = ["10.0.0.0/16"]
  dns_servers         = ["10.0.0.4", "10.0.0.5"]

  subnet {
    name           = "subnet1"
    address_prefix = "10.0.1.0/24"
  }

  subnet {
    name           = "subnet2"
    address_prefix = "10.0.2.0/24"
    security_group = azurerm_network_security_group.example.id
  }

  tags = {
    environment = "Production"
  }
}
```

## Text formatting
Text can be {--deleted--} and replacement text {++added++}. This can also be
combined into {~~one~>a single~~} operation. {==Highlighting==} is also
possible {>>and comments can be added inline<<}.

{==

Formatting can also be applied to blocks by putting the opening and closing
tags on separate lines and adding new lines between the tags and the content.

==}

## Images

<figure markdown>
  ![Image title](https://dummyimage.com/600x400/){ width="300" }
  <figcaption>Image caption</figcaption>
</figure>

## Using unordered lists

- Nulla et rhoncus turpis. Mauris ultricies elementum leo. Duis efficitur
  accumsan nibh eu mattis. Vivamus tempus velit eros, porttitor placerat nibh
  lacinia sed. Aenean in finibus diam.

    * Duis mollis est eget nibh volutpat, fermentum aliquet dui mollis.
    * Nam vulputate tincidunt fringilla.
    * Nullam dignissim ultrices urna non auctor.

## Using ordered lists

1.  Vivamus id mi enim. Integer id turpis sapien. Ut condimentum lobortis
    sagittis. Aliquam purus tellus, faucibus eget urna at, iaculis venenatis
    nulla. Vivamus a pharetra leo.

    1.  Vivamus venenatis porttitor tortor sit amet rutrum. Pellentesque aliquet
        quam enim, eu volutpat urna rutrum a. Nam vehicula nunc mauris, a
        ultricies libero efficitur sed.

    2.  Morbi eget dapibus felis. Vivamus venenatis porttitor tortor sit amet
        rutrum. Pellentesque aliquet quam enim, eu volutpat urna rutrum a.

        1.  Mauris dictum mi lacus
        2.  Ut sit amet placerat ante
        3.  Suspendisse ac eros arcu

# heading-1
## heading-2
### heading-3
#### heading-4
**bold** 

## note

!!! note
    This document explains how to get started with developing for NGINX Ingress controller.
    This document explains how to get started with developing for NGINX Ingress controller.

## important
!!! important
    The majority of make tasks run as docker containers, This document explains how to get started with developing for NGINX Ingress controller.This document explains how to get started with developing for NGINX Ingress controller.
    <https://kubernetes.github.io/ingress-nginx/developer-guide/getting-started/>
    This document explains how to get started with developing for NGINX Ingress controller.
## attention

!!! attention
    Starting in Version 0.22.0, ingress definitions using the annotation `nginx.ingress.kubernetes.io/rewrite-target` are not backwards compatible with previous versions. In Version 0.22.0 and beyond, any substrings within the request URI that need to be passed to the rewritten path must explicitly be defined in a [capture group](https://www.regular-expressions.info/refcapture.html).


## Table 

# Ingress examples

This directory contains a catalog of examples on how to run, configure and scale Ingress.
Please review the [prerequisites](PREREQUISITES.md) before trying them.

The examples on these pages include the `spec.ingressClassName` field which replaces the deprecated `kubernetes.io/ingress.class: nginx` annotation. Users of ingress-nginx < 1.0.0 (Helm chart < 4.0.0) should use the [legacy documentation](https://github.com/kubernetes/ingress-nginx/tree/legacy/docs/examples).

For more information, check out the [Migration to apiVersion networking.k8s.io/v1](../#faq-migration-to-apiversion-networkingk8siov1) guide.

Category | Name | Description | Complexity Level
---------| ---- | ----------- | ----------------
Apps | [Docker Registry](docker-registry/README.md) | TODO | TODO
Auth | [Basic authentication](auth/basic/README.md) | password protect your website | Intermediate
Auth | [Client certificate authentication](auth/client-certs/README.md) | secure your website with client certificate authentication | Intermediate
Auth | [External authentication plugin](auth/external-auth/README.md) | defer to an external authentication service | Intermediate
