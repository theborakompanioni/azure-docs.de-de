---
title: Bereitstellen von Ressourcen mit Azure-CLI und Vorlagen | Microsoft Docs
description: Verwenden Sie Azure Resource Manager und Azure-CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 493b7932-8d1e-4499-912c-26098282ec95
ms.service: azure-resource-manager
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: abdbb9a43f6f01303844677d900d11d984150df0
ms.openlocfilehash: c889a609b8d49474216fe1dcfba69a881edb4133
ms.lasthandoff: 04/20/2017


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI

In diesem Thema wird erläutert, wie Ihre Ressourcen mithilfe von [Azure CLI 2.0](/cli/azure/install-az-cli2) und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.  Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist.

Die in diesen Beispielen verwendete Vorlage (storage.json) finden Sie im Artikel [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md#final-template). Wenn Sie die Vorlage mit diesen Beispielen verwenden möchten, erstellen Sie eine JSON-Datei, und fügen Sie den kopierten Inhalt hinzu.

## <a name="deploy-local-template"></a>Bereitstellen einer lokalen Vorlage

Verwenden Sie die folgenden Befehle beim Bereitstellen einer lokalen Vorlage mit Inlineparametern, um schnell mit der Bereitstellung zu beginnen:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters "{\"storageNamePrefix\":{\"value\":\"contoso\"},\"storageSKU\":{\"value\":\"Standard_GRS\"}}"
```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Nachricht mit dem Ergebnis angezeigt:

```azurecli
"provisioningState": "Succeeded",
```

Im vorherigen Beispiel wurde die Ressourcengruppe in Ihrem Standardabonnement erstellt. Um ein anderes Abonnement zu verwenden, fügen Sie nach der Anmeldung den Befehl [az account set](/cli/azure/account#set) hinzu.

## <a name="deploy-external-template"></a>Bereitstellen einer externen Vorlage

Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter. Die Vorlage kann sich unter jedem öffentlich zugänglichen URI (z.B. einer Datei im Speicherkonto) befinden.
   
```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/exampleuser/MyTemplates/master/storage.json" \
    --parameters "{\"storageNamePrefix\":{\"value\":\"contoso\"},\"storageSKU\":{\"value\":\"Standard_GRS\"}}"
```

Sie können die Vorlage schützen, indem Sie festlegen, dass ein SAS-Token (Shared Access Signature) für den Zugriff erforderlich ist. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).

## <a name="parameter-files"></a>Parameterdateien

Mit den vorherigen Beispielen wurde demonstriert, wie Parameter als Inlinewerte übergeben werden. Sie können Parameterwerte in einer Datei angeben und diese Datei bei der Bereitstellung übergeben. 

Die Parameterdatei muss im folgenden Format vorliegen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageNamePrefix": {
         "value": "contoso"
     },
     "storageSKU": {
         "value": "Standard_GRS"
     }
  }
}
```

Um eine lokale Parameterdatei zu übergeben, verwenden Sie Folgendes:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-deployment"></a>Testen einer Bereitstellung

Verwenden Sie [az group deployment validate](/cli/azure/group/deployment#validate), um die Vorlage und Parameterwerte zu testen, ohne dabei Ressourcen bereitzustellen. Es gelten jeweils die gleichen Optionen für die Verwendung von lokalen oder Remotedateien.

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="debug"></a>Debuggen

Um Informationen zu den Vorgängen für einen Bereitstellungsfehler anzuzeigen, verwenden Sie:
   
```azurecli
az group deployment operation list --resource-group ExampleGroup --name vmlinux --query "[*].[properties.statusMessage]"
```

Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).


## <a name="export-resource-manager-template"></a>Exportieren einer Resource Manager-Vorlage
Für eine vorhandene Ressourcengruppe (die über Azure CLI oder eine andere Methode, z.B. das Portal, bereitgestellt wird), können Sie die Resource Manager-Vorlage für die Ressourcengruppe anzeigen. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können problemlos zukünftige Bereitstellungen der Lösung automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.
2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich die JavaScript Object Notation (JSON) zu Ihrer Lösung ansehen.

Führen Sie zum Anzeigen der Vorlage für eine Ressourcengruppe den Befehl [az group export](/cli/azure/group#export) aus.

```azurecli
az group export --name ExampleGroup
```

Weitere Informationen finden Sie unter [Exportieren einer Azure Resource Manager-Vorlage aus vorhandenen Ressourcen](resource-manager-export-template.md).


[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Nutzen Sie zur Verwendung des vollständigen Modus den `mode`-Parameter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters "{\"storageNamePrefix\":{\"value\":\"contoso\"},\"storageSKU\":{\"value\":\"Standard_GRS\"}}"
```


## <a name="next-steps"></a>Nächste Schritte
* Ein vollständiges Beispielskript, mit dem eine Vorlage bereitgestellt wird, finden Sie unter [Bereitstellung über Resource Manager-Vorlage – PowerShell-Skript](resource-manager-samples-cli-deploy.md).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).

