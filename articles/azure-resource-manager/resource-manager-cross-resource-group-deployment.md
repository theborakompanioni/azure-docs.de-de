---
title: "Bereitstellen von Azure-Ressourcen für mehrere Ressourcengruppen | Microsoft-Dokumentation"
description: "Hier wird gezeigt, wie während der Bereitstellung mehrere Azure-Ressourcengruppen als Ziel festgelegt werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/15/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: d8b041213b269775175a810e585103d3c538557f
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---

# <a name="deploy-azure-resources-to-more-than-one-resource-group"></a>Bereitstellen von Azure-Ressourcen für mehrere Ressourcengruppen

In der Regel stellen Sie alle Ressourcen in der Vorlage als einzelne Ressourcengruppe bereit. Es gibt jedoch Szenarien, bei denen Sie eine Reihe von Ressourcen zwar gemeinsam, aber in verschiedenen Ressourcengruppen bereitstellen möchten. Sie möchten beispielsweise den virtuellen Sicherungscomputer für Azure Site Recovery in einer separaten Ressourcengruppe und an einem separaten Standort bereitstellen. Resource Manager ermöglicht die Verwendung geschachtelter Vorlagen, um nicht die Ressourcengruppe, die für die übergeordnete Vorlage verwendet wird, sondern andere Ressourcengruppen als Ziel festzulegen.

Die Ressourcengruppe ist der Lebenszykluscontainer für die Anwendung und ihre Ressourcensammlung. Sie erstellen die Ressourcengruppe außerhalb der Vorlage und geben die gewünschte Ressourcengruppe für die Bereitstellung an. Eine Einführung zu Ressourcengruppen finden Sie unter [Übersicht über den Azure Resource Manager](resource-group-overview.md).

## <a name="example-template"></a>Beispielvorlage

Um eine andere Ressource als Ziel festzulegen, müssen Sie während der Bereitstellung eine geschachtelte oder verknüpfte Vorlage verwenden. Der Ressourcentyp `Microsoft.Resources/deployments` bietet einen `resourceGroup`-Parameter, mit dem Sie eine andere Ressourcengruppe für die geschachtelte Bereitstellung angeben können. Alle Ressourcengruppen müssen vorhanden sein, bevor Sie die Bereitstellung ausführen. Im folgenden Beispiel werden zwei Speicherkonten bereitgestellt: eins in der während der Bereitstellung angegebenen Ressourcengruppe und eins in einer Ressourcengruppe namens `crossResourceGroupDeployment`:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "StorageAccountName1": {
            "type": "string"
        },
        "StorageAccountName2": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "apiVersion": "2017-05-10",
            "name": "nestedTemplate",
            "type": "Microsoft.Resources/deployments",
            "resourceGroup": "crossResourceGroupDeployment",
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "name": "[parameters('StorageAccountName2')]",
                            "apiVersion": "2015-06-15",
                            "location": "West US",
                            "properties": {
                                "accountType": "Standard_LRS"
                            }
                        }
                    ]
                },
                "parameters": {}
            }
        },
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('StorageAccountName1')]",
            "apiVersion": "2015-06-15",
            "location": "West US",
            "properties": {
                "accountType": "Standard_LRS"
            }
        }
    ]
}
```

Wenn Sie für `resourceGroup` den Namen einer nicht vorhandenen Ressourcengruppe angeben, tritt bei der Bereitstellung ein Fehler auf. Wenn Sie keinen Wert für `resourceGroup` angeben, verwendet Resource Manager die übergeordnete Ressourcengruppe.  

## <a name="deploy-the-template"></a>Bereitstellen der Vorlage

Zum Bereitstellen der Beispielvorlage können Sie das Portal, Azure PowerShell oder die Azure CLI verwenden. Für Azure PowerShell oder Azure CLI-müssen Sie eine Version von Mai 2017 oder später verwenden. In den Beispielen wird davon ausgegangen, dass Sie die Vorlage lokal als Datei mit dem Namen **crossrgdeployment.json** gespeichert haben.

Für PowerShell:

```powershell
Login-AzureRmAccount

New-AzureRmResourceGroup -Name mainResourceGroup -Location "South Central US"
New-AzureRmResourceGroup -Name crossResourceGroupDeployment -Location "Central US"
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName mainResourceGroup `
  -TemplateFile c:\MyTemplates\crossrgdeployment.json
```

Für die Azure CLI:

```azurecli
az login

az group create --name mainResourceGroup --location "South Central US"
az group create --name crossResourceGroupDeployment --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group mainResourceGroup \
    --template-file crossrgdeployment.json
```

Nach Abschluss der Bereitstellung werden zwei Ressourcengruppen angezeigt. Jede Ressourcengruppe enthält ein Speicherkonto.

## <a name="use-resourcegroup-function"></a>Verwenden der resourceGroup()-Funktion

Für ressourcengruppenübergreifende Bereitstellungen wird die [resouceGroup()-Funktion](resource-group-template-functions-resource.md#resourcegroup) je nachdem, wie Sie die geschachtelte Vorlage festlegen, anders aufgelöst. 

Wenn Sie eine Vorlage in eine andere Vorlage einbetten, wird resouceGroup() in der geschachtelten Vorlage in die übergeordnete Ressourcengruppe aufgelöst. Eine eingebettete Vorlage verwendet das folgende Format:

```json
"apiVersion": "2017-05-10",
"name": "embeddedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "template": {
        ...
        resourceGroup() refers to parent resource group
    }
}
```

Wenn Sie einen Link zu einer getrennten Vorlage einrichten, wird resouceGroup() in der verlinkten Vorlage in die geschachtelte Ressourcengruppe aufgelöst. Eine verlinkte Vorlage verwendet das folgende Format:

```json
"apiVersion": "2017-05-10",
"name": "linkedTemplate",
"type": "Microsoft.Resources/deployments",
"resourceGroup": "crossResourceGroupDeployment",
"properties": {
    "mode": "Incremental",
    "templateLink": {
        ...
        resourceGroup() in linked template refers to linked resource group
    }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-powershell-sas-token.md).

