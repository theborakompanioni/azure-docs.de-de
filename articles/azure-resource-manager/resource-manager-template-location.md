---
title: Azure-Ressourcenspeicherort in Vorlagen | Microsoft-Dokumentation
description: "Der Artikel veranschaulicht, wie Sie einen Speicherort für eine Ressource in einer Azure Resource Manager-Vorlage festlegen."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: 73e50a593c41e841dcaf184abb895406ff5001e9
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="set-resource-location-in-azure-resource-manager-templates"></a>Festlegen des Ressourcenspeicherorts in Azure Resource Manager-Vorlagen
Beim Bereitstellen einer Vorlage müssen Sie einen Speicherort für jede Ressource angeben. Dieses Thema zeigt, wie Sie die Speicherorte bestimmen, die in Ihrem Abonnement für jeden Ressourcentyp zur Verfügung stehen.

## <a name="determine-supported-locations"></a>Bestimmen unterstützter Speicherorte

Eine vollständige Liste der unterstützten Speicherorte für jeden Ressourcentyp finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/). Ihr Abonnement kann möglicherweise nicht auf alle Speicherorte in dieser Liste zugreifen. Um eine benutzerdefinierte Liste aller für Ihr Abonnement verfügbaren Speicherorte anzuzeigen, verwenden Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle. 

Das folgende Beispiel verwendet PowerShell, um die Speicherorte für den Ressourcentyp `Microsoft.Web\sites` abzurufen:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

Das folgende Beispiel verwendet Azure CLI 2.0, um die Speicherorte für den Ressourcentyp `Microsoft.Web\sites` abzurufen:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

## <a name="set-location-in-template"></a>Festlegen des Speicherorts in einer Vorlage

Nachdem Sie einen unterstützten Speicherort für Ihre Ressourcen ermittelt haben, müssen Sie diesen Speicherort in Ihrer Vorlage festlegen. Die einfachste Möglichkeit zum Festlegen dieses Werts ist es, eine Ressourcengruppe in einem Speicherort zu erstellen, der die entsprechenden Ressourcentypen unterstützt, und jeden Speicherort auf `[resourceGroup().location]` festzulegen. Sie können die Vorlage für Ressourcengruppen in verschiedenen Speicherorten bereitstellen, ohne Werte in der Vorlage oder Parameter ändern zu müssen. 

Das folgende Beispiel zeigt ein Speicherkonto, das im gleichen Speicherort bereitgestellt wurde wie die Ressourcengruppe:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "variables": {
      "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
    },
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

Wenn Sie den Speicherort in Ihrer Vorlage hartcodieren müssen, geben Sie den Namen einer der unterstützten Regionen an. Das folgende Beispiel zeigt ein Speicherkonto, das immer in „USA, Norden-Mitte“ bereitgestellt wird:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
    {
      "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[concat('storageloc', uniqueString(resourceGroup().id))]",
      "location": "North Central US",
      "tags": {
        "Dept": "Finance",
        "Environment": "Production"
      },
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": { }
    }
    ]
}
```

## <a name="next-steps"></a>Nächste Schritte
* Empfehlungen zum Erstellen von Vorlagen finden Sie unter [Bewährte Methoden für das Erstellen von Azure Resource Manager-Vorlagen](resource-manager-template-best-practices.md).


