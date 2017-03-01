---
title: Bereitstellen von Ressourcen in Azure | Microsoft-Dokumentation
description: Verwenden Sie Azure PowerShell oder die Azure-CLI, um Ressourcen in Azure bereitzustellen. Die Ressourcen werden in einer Resource Manager-Vorlage definiert.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 41d9476d4ce6b7afc2ba9c757e96db7e8e72d5ae
ms.openlocfilehash: a77354d0719240e5916b5ec945dad75e534802d3
ms.lasthandoff: 02/21/2017


---
# <a name="deploy-resources-to-azure"></a>Bereitstellen von Ressourcen in Azure

In diesem Thema wird gezeigt, wie Sie Ressourcen für Ihr Azure-Abonnement bereitstellen. Sie können entweder Azure PowerShell oder die Azure-CLI verwenden, um eine Resource Manager-Vorlage bereitzustellen, mit der die Infrastruktur für Ihre Lösung definiert wird.

Eine Einführung in die Konzepte von Resource Manager finden Sie unter [Übersicht über Azure Resource Manager](resource-group-overview.md).

## <a name="steps-for-deployment"></a>Schritte zur Bereitstellung

In diesem Thema wird vorausgesetzt, dass Sie die im Thema angegebene [Beispielspeichervorlage](#example-storage-template) bereitstellen. Sie können auch eine andere Vorlage verwenden, aber die übergebenen Parameter unterscheiden sich dann von den Angaben im Thema.

Nach der Erstellung der Vorlage können Sie zum Bereitstellen Ihrer Vorlage die folgenden allgemeinen Schritte ausführen:

1. Anmelden am Konto
2. Auswählen des zu verwendenden Abonnements (nur erforderlich, wenn Sie über mehrere Abonnements verfügen und nicht das Standardabonnement verwenden möchten)
3. Erstellen einer Ressourcengruppe
4. Bereitstellen der Vorlage
5. Überprüfen des Bereitstellungsstatus

In den folgenden Abschnitten wird gezeigt, wie Sie diese Schritte mit [PowerShell](#powershell) oder der [Azure-CLI](#azure-cli) ausführen.

## <a name="powershell"></a>PowerShell

1. Informationen zur Installation von Azure PowerShell finden Sie unter [Erste Schritte mit Azure PowerShell-Cmdlets](/powershell/azureps-cmdlets-docs).

2. Verwenden Sie die folgenden Cmdlets, um schnell mit der Bereitstellung zu beginnen:

  ```powershell
  Login-AzureRmAccount
  Set-AzureRmContext -SubscriptionID {subscription-id}

  New-AzureRmResourceGroup -Name ExampleGroup -Location "Central US"
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json 
  ```

  Das `Set-AzureRmContext`-Cmdlet wird nur benötigt, wenn Sie ein anderes Abonnement als Ihr Standardabonnement verwenden möchten. Verwenden Sie Folgendes, um alle Abonnements und die dazugehörigen IDs anzuzeigen:

  ```powershell
  Get-AzureRmSubscription
  ```

3. Die Bereitstellung kann einige Minuten dauern. Nach Abschluss des Vorgangs wird eine Meldung der folgenden Art angezeigt:

  ```powershell
  DeploymentName          : ExampleDeployment
  CorrelationId           : 07b3b233-8367-4a0b-b9bc-7aa189065665
  ResourceGroupName       : ExampleGroup
  ProvisioningState       : Succeeded
  ...
  ```

4. Verwenden Sie Folgendes, um sich zu vergewissern, dass Ihre Ressourcengruppe und Ihr Speicherkonto für Ihr Abonnement bereitgestellt wurden:

  ```powershell
  Get-AzureRmResourceGroup -Name ExampleGroup
  Find-AzureRmResource -ResourceGroupNameEquals ExampleGroup
  ```

5. Beim Bereitstellen einer Vorlage können Sie Vorlagenparameter als PowerShell-Parameter angeben. Da das obige Beispiel keine Vorlagenparameter enthält, wurden die Standardwerte der Vorlage verwendet. Verwenden Sie Folgendes, um ein weiteres Speicherkonto bereitzustellen, und geben Sie die Parameterwerte für das Präfix des Speichernamens und der Speicherkonto-SKU an:

  ```powershell
  New-AzureRmResourceGroupDeployment -Name ExampleDeployment2 -ResourceGroupName ExampleGroup -TemplateFile c:\MyTemplates\azuredeploy.json -storageNamePrefix "contoso" -storageSKU "Standard_GRS"
  ```

  Ihre Ressourcengruppe enthält jetzt zwei Speicherkonten. 

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

1. Informationen zur Installation der Azure-Befehlszeilenschnittstelle (Azure-CLI) finden Sie unter [Installieren der Azure CLI 2.0](/cli/azure/install-az-cli2).

2. Verwenden Sie die folgenden Befehle, um schnell mit der Bereitstellung zu beginnen:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create --name ExampleDeployment --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json
  ```

  Der Befehl `az account set` wird nur benötigt, wenn Sie ein anderes Abonnement als Ihr Standardabonnement verwenden möchten. Verwenden Sie Folgendes, um alle Abonnements und die dazugehörigen IDs anzuzeigen:

  ```azurecli
  az account list
  ```

3. Die Bereitstellung kann einige Minuten dauern. Nach Abschluss des Vorgangs wird eine Meldung der folgenden Art angezeigt:

  ```azurecli
  "provisioningState": "Succeeded",
  ```

4. Verwenden Sie Folgendes, um sich zu vergewissern, dass Ihre Ressourcengruppe und Ihr Speicherkonto für Ihr Abonnement bereitgestellt wurden:

  ```azurecli
  az group show --name ExampleGroup
  az resource list --resource-group ExampleGroup
  ```

5. Beim Bereitstellen einer Vorlage können Sie Vorlagenparameter als PowerShell-Parameter angeben. Da das obige Beispiel keine Vorlagenparameter enthält, wurden die Standardwerte der Vorlage verwendet. Verwenden Sie Folgendes, um ein weiteres Speicherkonto bereitzustellen, und geben Sie die Parameterwerte für das Präfix des Speichernamens und der Speicherkonto-SKU an:

  ```azurecli
  az group deployment create --name ExampleDeployment2 --resource-group ExampleGroup --template-file c:\MyTemplates\azuredeploy.json --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Ihre Ressourcengruppe enthält jetzt zwei Speicherkonten. 

## <a name="example-storage-template"></a>Beispielspeichervorlage

Verwenden Sie die folgende Beispielvorlage, um ein Speicherkonto für Ihr Abonnement bereitzustellen:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Nächste Schritte

* Ausführliche Informationen zur Verwendung von PowerShell zum Bereitstellen von Vorlagen finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure PowerShell](/azure/azure-resource-manager/resource-group-template-deploy).
* Information zum Bereitstellen von Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](/azure/azure-resource-manager/resource-group-template-deploy-cli).




