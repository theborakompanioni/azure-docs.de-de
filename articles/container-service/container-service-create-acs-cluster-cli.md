---
title: Bereitstellen eines Azure Container Service-Clusters per CLI | Microsoft Docs
description: Bereitstellen eines Azure Container Service-Clusters per Azure-CLI 2.0 (Vorschau)
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: 
ms.assetid: 8da267e8-2aeb-4c24-9a7a-65bdca3a82d6
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/01/2016
ms.author: saudas
translationtype: Human Translation
ms.sourcegitcommit: 855f0fe77bd55f6ec0dacad4bc28603ac1c6979c
ms.openlocfilehash: c4a513686433e802f27f78de60e8b7fca21b4634


---
# <a name="using-the-azure-cli-20-preview-to-create-an-azure-container-service-cluster"></a>Verwenden der Azure-CLI 2.0 (Vorschau) zum Erstellen eines Azure Container Service-Clusters

Sie benötigen Folgendes, um einen Azure Container Service-Cluster zu erstellen:
* Azure-Konto ([kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/))
* Installierte [Azure-Befehlszeilenschnittstelle 2.0 (Vorschau)](https://github.com/Azure/azure-cli#installation)
* Anmeldung bei Ihrem Azure-Konto (siehe unten)

## <a name="log-in-to-your-account"></a>Anmelden am Konto
```azurecli
az login 
```
Verwenden Sie diesen [Link](https://login.microsoftonline.com/common/oauth2/deviceauth), um die Authentifizierung mit dem Gerätecode durchzuführen, der in der CLI angegeben ist.

![Befehlseingabe](media/container-service-create-acs-cluster-cli/login.png)

![Browser](media/container-service-create-acs-cluster-cli/login-browser.png)


## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
```azurecli
az group create -n acsrg1 -l "westus"
```

![Abbildung: „resource group create“](media/container-service-create-acs-cluster-cli/rg-create.png)

## <a name="list-of-available-azure-container-service-cli-commands"></a>Liste mit verfügbaren Azure Container Service-CLI-Befehlen

```azurecli
az acs -h
```

![Verwendung des ACS-Befehls](media/container-service-create-acs-cluster-cli/acs-command-usage-help.png)

## <a name="create-an-azure-container-service-cluster"></a>Erstellen eines Azure Container Service-Clusters

*Verwendung von „acs create“ in der CLI*

```azurecli
az acs create -h
```
Der Name des Containerdiensts, die im vorherigen Schritt erstellte Ressourcengruppe und ein eindeutiger DNS-Name sind obligatorisch. Andere Eingaben werden auf die Standardwerte festgelegt (siehe folgenden Screenshot als Hilfe), sofern sie nicht durch die entsprechenden Switches überschrieben werden.
![Abbildung: Hilfe zu „acs create“](media/container-service-create-acs-cluster-cli/acs-command-usage-help.png)

*Schneller Vorgang „acs create“ mit Standardwerten. Verwenden Sie den zweiten Befehl, wenn Sie keinen SSH-Schlüssel haben. Mit diesem zweiten Erstellungsbefehl mit dem Switch „--generate-ssh-keys“ wird ein Schlüssel für Sie erstellt.*

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789
```

```azurecli
az acs create -n acs-cluster -g acsrg1 -d applink789 --generate-ssh-keys
```

*Achten Sie darauf, dass „dns-prefix“ (Switch „-d“) eindeutig ist. Wenn Sie eine Fehlermeldung erhalten, können Sie es mit einer eindeutigen Zeichenfolge erneut versuchen.*

Warten Sie nach dem Eingeben des vorherigen Befehls ca. 10 Minuten, bis der Cluster erstellt wurde.

![Abbildung: acs create](media/container-service-create-acs-cluster-cli/cluster-create.png)

## <a name="list-acs-clusters"></a>Auflisten von ACS-Clustern 

### <a name="under-a-subscription"></a>Unter einem Abonnement

```azurecli
az acs list --output table
```

### <a name="in-a-specific-resource-group"></a>In einer bestimmten Ressourcengruppe

```azurecli
az acs list -g acsrg1 --output table
```

![Abbildung: acs list](media/container-service-create-acs-cluster-cli/acs-list.png)


## <a name="display-details-of-a-container-service-cluster"></a>Anzeigen der Details eines Container Service-Clusters

```azurecli
az acs show -g acsrg1 -n acs-cluster --output list
```

![Abbildung: acs list](media/container-service-create-acs-cluster-cli/acs-show.png)


## <a name="scale-the-acs-cluster"></a>Skalieren des ACS-Clusters
*Sowohl das horizontale Herunterskalieren als auch das horizontale Hochskalieren sind zulässig. Der Parameter „new-agent-count“ ist die neue Anzahl von Agents im ACS-Cluster.*

```azurecli
az acs scale -g acsrg1 -n acs-cluster --new-agent-count 4
```

![Abbildung: acs scale](media/container-service-create-acs-cluster-cli/acs-scale.png)

## <a name="delete-a-container-service-cluster"></a>Löschen eines Container Service-Clusters
```azurecli
az acs delete -g acsrg1 -n acs-cluster 
```
*Beachten Sie, dass mit diesem Befehl nicht alle Ressourcen (Netzwerk und Speicher) gelöscht werden, die beim Erstellen des Container Service erstellt wurden. Zum Löschen aller Ressourcen wird empfohlen, dass pro Ressourcengruppe ein einzelner ACS-Cluster erstellt und dann die Ressourcengruppe selbst gelöscht wird, wenn der ACS-Cluster nicht mehr benötigt wird. So wird sichergestellt, dass alle zugehörigen Ressourcen gelöscht und keine Gebühren dafür berechnet werden.*



<!--HONumber=Nov16_HO3-->


