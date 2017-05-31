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
ms.date: 05/15/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 17c4dc6a72328b613f31407aff8b6c9eacd70d9a
ms.openlocfilehash: 9c9eff8c828329b9d8358f88b90c174c64f5c29f
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI

In diesem Thema wird erläutert, wie Ihre Ressourcen mithilfe von Azure CLI 2.0 und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden. Wenn Sie nicht mit den Konzepten der Bereitstellung und Verwaltung Ihrer Azure-Lösungen vertraut sind, informieren Sie sich unter [Übersicht über Azure Resource Manager](resource-group-overview.md).  

Die Resource Manager-Vorlage, die Sie bereitstellen, kann entweder eine lokale Datei auf Ihrem Computer oder eine externe Datei sein, die sich in einem Repository wie GitHub befindet. Die Vorlage, die Sie in diesem Artikel bereitstellen, finden Sie im Abschnitt [Beispielvorlage](#sample-template) oder als [Speicherkontovorlage in GitHub](https://github.com/Azure/azure-quickstart-templates/blob/master/101-storage-account-create/azuredeploy.json).

[!INCLUDE [sample-cli-install](../../includes/sample-cli-install.md)]

<a id="deploy-local-template" />

## <a name="deploy-a-template-from-your-local-machine"></a>Bereitstellen einer Vorlage von Ihrem lokalen Computer aus

Beim Bereitstellen von Ressourcen in Azure gehen Sie folgendermaßen vor:

1. Melden Sie sich bei Ihrem Azure-Konto an.
2. Erstellen Sie eine Ressourcengruppe, die als Container für die bereitgestellten Ressourcen fungiert. Der Name einer Ressourcengruppe darf nur alphanumerische Zeichen, Punkte, Unterstriche, Bindestriche und Klammern enthalten. Der Name kann bis zu 90 Zeichen umfassen. Der Name darf nicht mit einem Punkt enden.
3. Stellen Sie für die Ressourcengruppe die Vorlage bereit, die die zu erstellenden Ressourcen definiert.

Eine Vorlage kann Parameter enthalten, mit denen Sie die Bereitstellung anpassen können. Beispielsweise können Sie Werte angeben, die einer bestimmten Umgebung (z.B. Entwicklung, Test und Produktion) angepasst sind. Die Beispielvorlage definiert einen Parameter für die Speicherkonto-SKU. 

Im folgenden Beispiel wird eine Ressourcengruppe erstellt und eine Vorlage von Ihrem lokalen Computer aus bereitgestellt:

```azurecli
az login

az group create --name ExampleGroup --location "Central US"
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters "{\"storageAccountType\":{\"value\":\"Standard_GRS\"}}"
```

Die Bereitstellung kann einige Minuten dauern. Wenn sie abgeschlossen ist, wird eine Nachricht mit dem Ergebnis angezeigt:

```azurecli
"provisioningState": "Succeeded",
```

## <a name="deploy-a-template-from-an-external-source"></a>Bereitstellen einer Vorlage aus einer externen Quelle

Anstatt Resource Manager-Vorlagen auf dem lokalen Computer zu speichern, könnten Sie sie vorzugsweise an einem externen Speicherort speichern. Sie können Vorlagen in einem Quellcodeverwaltungs-Repository (z.B. GitHub) speichern. Für den gemeinsamen Zugriff in Ihrer Organisation können Sie sie auch in einem Azure-Speicherkonto speichern.

Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter. Verwenden Sie den URI im Beispiel, um die Beispielvorlage aus GitHub bereitzustellen.
   
```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json" \
    --parameters "{\"storageAccountType\":{\"value\":\"Standard_GRS\"}}"
```

Das obige Beispiel erfordert einen URI mit öffentlichem Zugriff für die Vorlage, was in den meisten Szenarien funktioniert, da die Vorlage keine vertraulichen Daten enthalten sollte. Wenn Sie vertrauliche Daten (z.B. ein Administratorkennwort) angeben müssen, übergeben Sie diesen Wert als sicheren Parameter. Wenn Sie jedoch keinen öffentlichen Zugriff auf Ihre Vorlage wünschen, können Sie sie schützen, indem Sie sie in einem privaten Speichercontainer speichern. Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token (Shared Access Signature) erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).

## <a name="parameter-files"></a>Parameterdateien

Anstatt Parameter als Inlinewerte in Ihrem Skript zu übergeben, ist es wohl einfacher, eine JSON-Datei zu verwenden, die die Parameterwerte enthält. Die Parameterdatei muss im folgenden Format vorliegen:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
     "storageAccountType": {
         "value": "Standard_GRS"
     }
  }
}
```

Beachten Sie, dass der Parameterabschnitt einen Parameternamen enthält, der dem in Ihrer Vorlage definierten Parameter (storageAccountType) entspricht. Die Parameterdatei enthält einen Wert für den Parameter. Dieser Wert wird der Vorlage automatisch während der Bereitstellung übergeben. Sie können mehrere Parameterdateien für verschiedene Bereitstellungsszenarien erstellen und dann die entsprechende Parameterdatei übergeben. 

Kopieren Sie das obige Beispiel, und speichern Sie es unter dem Dateinamen `storage.parameters.json`.

Um eine lokale Parameterdatei zu übergeben, verwenden Sie `@` zur Angabe einer lokalen Datei mit dem Namen „storage.parameters.json“ anzugeben.

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

## <a name="test-a-template-deployment"></a>Testen einer Vorlagenbereitstellung

Verwenden Sie [az group deployment validate](/cli/azure/group/deployment#validate), um die Vorlage und Parameterwerte zu testen, ohne dabei Ressourcen bereitzustellen. 

```azurecli
az group deployment validate \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters @storage.parameters.json
```

Wenn keine Fehler erkannt werden, gibt der Befehl Informationen über die Testbereitstellung zurück. Beachten Sie insbesondere, dass der **error**-Wert NULL ist.

```azurecli
{
  "error": null,
  "properties": {
      ...
```

Wenn ein Fehler erkannt wird, gibt der Befehl eine Fehlermeldung zurück. Beispielsweise wird bei dem Versuch, einen falschen Wert für die Speicherkonto-SKU zu übergeben, folgender Fehler zurückgegeben:

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template validation failed: 'The provided value 'badSKU' for the template parameter 
      'storageAccountType' at line '13' and column '20' is not valid. The parameter value is not part of the allowed 
      value(s): 'Standard_LRS,Standard_ZRS,Standard_GRS,Standard_RAGRS,Premium_LRS'.'.",
    "target": null
  },
  "properties": null
}
```

Wenn Ihre Vorlage einen Syntaxfehler aufweist, gibt der Befehl einen Fehler zurück, der angibt, dass die Vorlage nicht analysiert werden konnte. Die Meldung gibt Zeilennummer und Position des Analysefehlers an.

```azurecli
{
  "error": {
    "code": "InvalidTemplate",
    "details": null,
    "message": "Deployment template parse failed: 'After parsing a value an unexpected character was encountered:
      \". Path 'variables', line 31, position 3.'.",
    "target": null
  },
  "properties": null
}
```

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Nutzen Sie zur Verwendung des vollständigen Modus den `mode`-Parameter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters "{\"storageAccountType\":{\"value\":\"Standard_GRS\"}}"
```

## <a name="sample-template"></a>Vorlagenbeispiel

Bei den Beispielen in diesem Thema wird die folgende Vorlage verwendet. Kopieren Sie sie, und speichern Sie sie unter dem Dateinamen „storage.json“. Informationen zum Erstellen dieser Vorlage finden Sie unter [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md).  

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccountType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_GRS",
        "Standard_ZRS",
        "Premium_LRS"
      ],
      "metadata": {
        "description": "Storage Account type"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "2016-01-01",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "[parameters('storageAccountType')]"
      },
      "kind": "Storage", 
      "properties": {
      }
    }
  ],
  "outputs": {
      "storageAccountName": {
          "type": "string",
          "value": "[variables('storageAccountName')]"
      }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* In den Beispielen dieses Artikels werden Ressourcen für eine Ressourcengruppe in Ihrem Standardabonnement bereitgestellt. Wenn Sie ein anderes Abonnement verwenden möchten, lesen Sie [Manage multiple Azure subscriptions](/cli/azure/manage-azure-subscriptions-azure-cli) (Verwalten mehrerer Azure-Abonnements).
* Ein vollständiges Beispielskript, mit dem eine Vorlage bereitgestellt wird, finden Sie unter [Resource Manager-Vorlage – Bereitstellungsskript](resource-manager-samples-cli-deploy.md).
* Um zu verstehen, wie Parameter in der Vorlage definiert werden, lesen Sie [Verstehen der Struktur und Syntax von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).
* Informationen zum Bereitstellen einer Vorlage, die ein SAS-Token erfordert, finden Sie unter [Bereitstellen einer privaten Vorlage mit SAS-Token](resource-manager-cli-sas-token.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).

