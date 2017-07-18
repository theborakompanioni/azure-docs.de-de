---
title: "Erstellen einer privaten Docker-Containerregistrierung – Azure CLI | Microsoft-Dokumentation"
description: Erste Schritte beim Erstellen und Verwalten von privaten Docker-Containerregistrierungen mit der Azure CLI 2.0
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: na
tags: 
keywords: 
ms.assetid: 29e20d75-bf39-4f7d-815f-a2e47209be7d
ms.service: container-registry
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/11/2017
ms.author: nepeters
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 2ad539c85e01bc132a8171490a27fd807c8823a4
ms.openlocfilehash: c7cdb1b13bf32388d18c2a25af28337a81861c1e
ms.contentlocale: de-de
ms.lasthandoff: 07/12/2017

---

# <a name="create-a-managed-container-registry-using-the-azure-cli"></a>Erstellen einer verwalteten Containerregistrierung mithilfe der Azure-Befehlszeilenschnittstelle

Azure Container Registry ist ein verwalteter Docker-Containerregistrierungsdienst zum Speichern privater Docker-Containerimages. In diesem Leitfaden erfahren Sie, wie Sie mithilfe der Azure-Befehlszeilenschnittstelle eine verwaltete Azure Container Registry-Instanz erstellen.

Verwaltete Azure-Containerregistrierungen befinden sich derzeit in der Vorschauphase und stehen nicht in allen Regionen zur Verfügung.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für diesen Schnellstart die Azure CLI-Version 2.0.4 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Erstellen Sie mit dem Befehl [az group create](/cli/azure/group#create) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden. 

Im folgenden Beispiel wird am Standort *westcentralus* eine Ressourcengruppe namens *myResourceGroup* erstellt.

```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus
```

## <a name="create-a-container-registry"></a>Erstellen einer Containerregistrierung

Erstellen Sie mithilfe des Befehls [az acr create](/cli/azure/acr#create) eine ACR-Instanz.

> [!NOTE]
> Geben Sie beim Erstellen einer Registrierung einen global eindeutigen Namen einer Domäne der obersten Ebene an, der nur Buchstaben und Zahlen enthält.

 Der Registrierungsname in dem Beispiel lautet *myContainerRegistry1*. Ersetzen Sie ihn durch einen eigenen eindeutigen Namen.

```azurecli
az acr create --name myContainerRegistry1 --resource-group myResourceGroup --sku Managed_Standard
```

Wenn die Registrierung erstellt wird, sieht die Ausgabe etwa wie folgt aus:

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-29T04:50:28.607134+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry1",
  "location": "westcentralus",
  "loginServer": "mycontainerregistry1.azurecr.io",
  "name": "myContainerRegistry1",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Managed_Standard",
    "tier": "Managed"
  },
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

## <a name="log-in-to-acr-instance"></a>Anmelden bei der ACR-Instanz

Bevor Sie Push- und Pullvorgänge für Containerimages ausführen können, müssen Sie sich bei der ACR-Instanz anmelden. Verwenden Sie hierzu den Befehl [az acr login](/cli/azure/acr#login).

```azurecli-interactive
az acr login --name myAzureContainerRegistry1
```

Nach Abschluss des Vorgangs wird eine Erfolgsmeldung zurückgegeben.

## <a name="use-azure-container-registry"></a>Verwenden von Azure Container Registry

### <a name="list-container-images"></a>Auflisten von Containerimages

Verwenden Sie die CLI-Befehle vom Typ `az acr`, um die Images und Tags in einem Repository abzufragen.

> [!NOTE]
> Derzeit wird der Befehl `docker search` zum Abfragen von Images und Tags für die Containerregistrierung nicht unterstützt.

### <a name="list-repositories"></a>Auflisten von Repositorys

Im folgenden Beispiel sind die Repositorys in einer Registrierung im JSON-Format (JavaScript Object Notation) aufgelistet:

```azurecli
az acr repository list -n myContainerRegistry1 -o json
```

### <a name="list-tags"></a>Auflisten von Tags

Im folgenden Beispiel sind die Tags im Repository **samples/nginx** im JSON-Format aufgelistet:

```azurecli
az acr repository show-tags -n myContainerRegistry1 --repository samples/nginx -o json
```

## <a name="next-steps"></a>Nächste Schritte

In dieser Schnellstartanleitung haben Sie mithilfe der Azure-Befehlszeilenschnittstelle eine verwaltete Azure Container Registry-Instanz erstellt.

> [!div class="nextstepaction"]
> [Freigeben Ihres ersten Image mit der Docker CLI](container-registry-get-started-docker-cli.md)

