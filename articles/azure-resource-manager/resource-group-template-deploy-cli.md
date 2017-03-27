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
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: a087df444c5c88ee1dbcf8eb18abf883549a9024
ms.openlocfilehash: b084c722b75152b8a2b867f21d546abd04a96f04
ms.lasthandoff: 03/15/2017


---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-cli"></a>Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI
> [!div class="op_single_selector"]
> * [PowerShell](resource-group-template-deploy.md)
> * [Azure-Befehlszeilenschnittstelle](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [REST-API](resource-group-template-deploy-rest.md)
> 
> 

In diesem Thema wird erläutert, wie Ihre Ressourcen mithilfe von [Azure CLI 2.0](/cli/azure/install-az-cli2) und Azure Resource Manager-Vorlagen in Azure bereitgestellt werden.  Ihre Vorlage kann entweder eine lokale Datei oder eine externe Datei sein, die über einen URI verfügbar ist. Wenn sich Ihre Vorlage in einem Speicherkonto befindet, können Sie den Zugriffs auf die Vorlage beschränken und ein SAS-Token (Shared Access Signature) während der Bereitstellung angeben.

## <a name="deploy"></a>Bereitstellen

* Verwenden Sie die folgenden Befehle beim Bereitstellen einer lokalen Vorlage mit Inlineparametern, um schnell mit der Bereitstellung zu beginnen:

  ```azurecli
  az login
  az account set --subscription {subscription-id}

  az group create --name ExampleGroup --location "Central US"
  az group deployment create \
      --name ExampleDeployment \
      --resource-group ExampleGroup \
      --template-file storage.json \
      --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
  ```

  Die Bereitstellung kann einige Minuten dauern. Nach Abschluss des Vorgangs wird eine Meldung der folgenden Art angezeigt:

  ```azurecli
  "provisioningState": "Succeeded",
  ```
  
* Der Befehl `az account set` wird nur benötigt, wenn Sie ein anderes Abonnement als Ihr Standardabonnement verwenden möchten. Verwenden Sie Folgendes, um alle Abonnements und die dazugehörigen IDs anzuzeigen:

  ```azurecli
  az account list
  ```

* Verwenden Sie zum Bereitstellen einer externen Vorlage den **template-uri**-Parameter:
   
   ```azurecli
   az group deployment create \
       --name ExampleDeployment \
       --resource-group ExampleGroup \
       --template-uri "https://raw.githubusercontent.com/exampleuser/MyTemplates/master/storage.json" \
       --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
   ```

* Um die Parameterwerte in einer Datei zu übergeben, verwenden Sie Folgendes:

   ```azurecli
   az group deployment create \
       --name ExampleDeployment \
       --resource-group ExampleGroup \
       --template-file storage.json \
       --parameters @storage.parameters.json
   ```

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


[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

Nutzen Sie zur Verwendung des vollständigen Modus den Modusparameter:

```azurecli
az group deployment create \
    --name ExampleDeployment \
    --mode Complete \
    --resource-group ExampleGroup \
    --template-file storage.json \
    --parameters '{"storageNamePrefix":{"value":"contoso"},"storageSKU":{"value":"Standard_GRS"}}'
```

## <a name="deploy-template-from-storage-with-sas-token"></a>Bereitstellen einer Vorlage aus Speicher mithilfe eines SAS-Tokens
Sie können Ihre Vorlagen einem Speicherkonto hinzufügen und sie während der Bereitstellung mit einem SAS-Token verknüpfen.

> [!IMPORTANT]
> Wenn Sie die nachstehenden Schritte befolgen, hat nur der Kontobesitzer Zugriff auf das Blob mit der Vorlage. Wenn Sie jedoch ein SAS-Token für das Blob erstellen, können andere Benutzer über diesen URI auf das Blob zugreifen. Wenn ein anderer Benutzer den URI abfängt, hat dieser Benutzer Zugriff auf die Vorlage. Ein SAS-Token ist eine gute Möglichkeit zum Einschränken des Zugriffs auf Ihre Vorlagen. Sie sollten allerdings Kennwörter auf keinen Fall direkt in die Vorlage einschließen.
> 
> 

### <a name="add-private-template-to-storage-account"></a>Hinzufügen einer privaten Vorlage zum Speicherkonto
Die folgenden Befehle richten einen privaten Speicherkontocontainer ein und laden eine Vorlage hoch:
   
```azurecli
az group create --name "ManageGroup" --location "South Central US"
az storage account create \
    --resource-group ManageGroup \
    --location "South Central US" \
    --sku Standard_LRS \
    --kind Storage \
    --name {your-unique-name}
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
az storage container create \
    --name templates \
    --public-access Off \
    --connection-string $connection
az storage blob upload \
    --container-name templates \
    --file vmlinux.json \
    --name vmlinux.json \
    --connection-string $connection
```

### <a name="provide-sas-token-during-deployment"></a>Bereitstellen eines SAS-Tokens während der Bereitstellung
Generieren Sie zum Bereitstellen einer privaten Vorlage in einem Speicherkonto ein SAS-Token, und fügen Sie es dem URI für die Vorlage hinzu. Legen Sie die Ablaufzeit so fest, dass ausreichend Zeit für die Bereitstellung bleibt.
   
```azurecli
seconds='@'$(( $(date +%s) + 1800 ))
expiretime=$(date +%Y-%m-%dT%H:%MZ --date=$seconds)
connection=$(az storage account show-connection-string \
    --resource-group ManageGroup \
    --name {your-unique-name} \
    --query connectionString)
token=$(az storage blob generate-sas \
    --container-name templates \
    --name vmlinux.json \
    --expiry $expiretime \
    --permissions r \
    --output tsv \
    --connection-string $connection)
url=$(az storage blob url \
    --container-name templates \
    --name vmlinux.json \
    --output tsv \
    --connection-string $connection)
az group deployment create --resource-group ExampleGroup --template-uri $url?$token
```

Ein Beispiel der Verwendung eines SAS-Tokens mit verknüpften Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen mit Azure Resource Manager](resource-group-linked-templates.md).

## <a name="debug"></a>Debuggen

Um Informationen zu den Vorgängen für einen Bereitstellungsfehler anzuzeigen, verwenden Sie:
   
```azurecli
az group deployment operation list --resource-group ExampleGroup --name vmlinux --query "[*].[properties.statusMessage]"
```

Tipps zum Beheben gängiger Azure-Bereitstellungsfehler finden Sie unter [Beheben gängiger Azure-Bereitstellungsfehler mit Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="next-steps"></a>Nächste Schritte
* Ein Beispiel für die Bereitstellung von Ressourcen über die .NET-Clientbibliothek finden Sie unter [Bereitstellen von Ressourcen mithilfe von .NET-Bibliotheken und einer Vorlage](../virtual-machines/virtual-machines-windows-csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Informationen zum Definieren von Parametern in der Vorlage finden Sie unter [Erstellen von Vorlagen](resource-group-authoring-templates.md#parameters).
* Informationen zum Bereitstellen der Lösung in andere Umgebungen finden Sie unter [Entwicklungs- und Testumgebungen in Microsoft Azure](solution-dev-test-environments.md).
* Weitere Informationen zum Verwenden eines KeyVault-Verweises zum Übergeben sicherer Werte finden Sie unter [Übergeben sicherer Werte während der Bereitstellung](resource-manager-keyvault-parameter.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Eine vierteilige Reihe zum Automatisieren von Bereitstellungen finden Sie unter [Automatisieren von Anwendungsbereitstellungen auf virtuellen Azure-Computern](../virtual-machines/virtual-machines-windows-dotnet-core-1-landing.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Diese Reihe behandelt die Anwendungsarchitektur, Zugriff und Sicherheit, Verfügbarkeit und Skalierung sowie die Bereitstellung einer Anwendung.


