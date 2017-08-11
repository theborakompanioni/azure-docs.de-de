---
title: "Einbinden eines Azure Files-Datenträgers in Azure Container Instances"
description: Erfahren Sie, wie Sie ein Azure Files-Volume einbinden, sodass der Zustand bei Azure Container Instances beibehalten wird.
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/01/2017
ms.author: seanmck
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: d0e56fb385c4997bd1a14d1afed0af7a38181b22
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---

# <a name="mounting-an-azure-file-share-with-azure-container-instances"></a>Einbinden einer Azure-Dateifreigabe in Azure Container Instances

Standardmäßig ist Azure Container Instances zustandslos. Wenn der Container abstürzt oder beendet wird, gehen alle Zustände verloren. Um den Zustand nach Ablauf der Lebensdauer des Containers beizubehalten, müssen Sie ein Volume aus einem externen Speicher einbinden. In diesem Artikel wird das Einbinden einer Azure-Dateifreigabe für die Verwendung mit Azure Container Instances veranschaulicht.

## <a name="create-an-azure-file-share"></a>Erstellen einer Azure-Dateifreigabe

Bevor Sie eine Azure-Dateifreigabe in Azure Container Instances einbinden, müssen Sie sie erstellen. Führen Sie das folgende Skript aus, um ein Speicherkonto zum Hosten der Dateifreigabe und die Freigabe selbst zu erstellen. Beachten Sie, dass der Name des Speicherkontos global eindeutig sein muss, daher fügt das Skript der Basis-Zeichenfolge einen Zufallswert hinzu.

```azurecli-interactive
# Change these four parameters
ACI_PERS_STORAGE_ACCOUNT_NAME=mystorageaccount$RANDOM
ACI_PERS_RESOURCE_GROUP=myResourceGroup
ACI_PERS_LOCATION=eastus
ACI_PERS_SHARE_NAME=acishare

# Create the storage account with the parameters
az storage account create -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -l $ACI_PERS_LOCATION --sku Standard_LRS

# Export the connection string as an environment variable, this is used when creating the Azure file share
export AZURE_STORAGE_CONNECTION_STRING=`az storage account show-connection-string -n $ACI_PERS_STORAGE_ACCOUNT_NAME -g $ACI_PERS_RESOURCE_GROUP -o tsv`

# Create the share
az storage share create -n $ACI_PERS_SHARE_NAME
```

## <a name="acquire-storage-account-access-details"></a>Erhalten der Zugriffsinformationen für das Azure-Speicherkonto

Um eine Azure-Dateifreigabe als Volume in Azure Container Instances einzubinden, benötigen Sie drei Werte: den Namen des Speicherkontos, den Freigabenamen und den Speicherzugriffsschlüssel. 

Wenn Sie das oben angegebene Skript verwendet haben, wurde der Name des Speicherkontos mit einem zufälligen Wert am Ende erstellt. Um die endgültige Zeichenfolge (einschließlich des zufälligen Teils) abzufragen, verwenden Sie die folgenden Befehle:

```azurecli-interactive
STORAGE_ACCOUNT=$(az storage account list --resource-group myResourceGroup --query "[?contains(name,'mystorageaccount')].[name]" -o tsv)
echo $STORAGE_ACCOUNT
```

Der Freigabename ist bereits bekannt (er lautet im oben angegebenen Skript *acishare*), daher verbleibt nur der Speicherkontoschlüssel, der mit dem folgenden Befehl gesucht werden kann:

```azurecli-interactive
$STORAGE_KEY=$(az storage account keys list --resource-group myResourceGroup --account-name $STORAGE_ACCOUNT --query "[0].value" -o tsv)
echo $STORAGE_KEY
```

## <a name="store-storage-account-access-details-with-azure-key-vault"></a>Speichern der Informationen für den Speicherkontozugriff mit einem Azure-Schlüsseltresor

Speicherkontoschlüssel schützen den Zugriff auf Ihre Daten, daher empfiehlt es sich, sie in einem Azure-Schlüsseltresor zu speichern. 

Erstellen eines Schlüsseltresors mit der Azure-Befehlszeilenschnittstelle:

```azurecli-interactive
KEYVAULT_NAME=aci-keyvault
az keyvault create -n $KEYVAULT_NAME --enabled-for-template-deployment -g myResourceGroup
```

Über den Schalter `enabled-for-template-deployment` kann Azure Resource Manager Geheimnisse zum Zeitpunkt der Bereitstellung aus Ihrem Schlüsseltresor entnehmen.

Speichern des Speicherkontoschlüssels als neues Geheimnis im Schlüsseltresor:

```azurecli-interactive
KEYVAULT_SECRET_NAME=azurefilesstoragekey
az keyvault secret set --vault-name $KEYVAULT_NAME --name $KEYVAULT_SECRET_NAME --value $STORAGE_KEY
```

## <a name="mount-the-volume"></a>Einbinden des Volumes

Das Einbinden einer Azure-Dateifreigabe als Volume in einem Container ist ein zweistufiger Vorgang. Zunächst geben Sie die Details der Freigabe als Teil der Definition der Containergruppe an, dann geben Sie an, wie das Volume in einen oder mehrere Container in der Gruppe eingebunden werden soll.

Zum Definieren der Volumes, die für die Einbindung verfügbar gemacht werden sollen, fügen Sie der ausgewählten Containergruppendefinition in der Azure Resource Manager-Vorlage ein `volumes`-Array hinzu und verweisen dann in der Definition der einzelnen Container auf die Volumes.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "type": "string"
    },
    "storageaccountkey": {
      "type": "securestring"
    }
  },
  "resources":[{
    "name": "hellofiles",
    "type": "Microsoft.ContainerInstance/containerGroups",
    "apiVersion": "2017-08-01-preview",
    "location": "[resourceGroup().location]",
    "properties": {
      "containers": [{
        "name": "hellofiles",
        "properties": {
          "image": "seanmckenna/aci-hellofiles",
          "resources": {
            "request": {
              "cpu": 1,
              "memoryInGb": 1.5
            }
          },
          "volumeMounts": [{
            "name": "myvolume",
            "mountPath": "/aci/logs/"
          }]
        }  
      }],
      "osType": "Linux",
      "volumes": [{
        "name": "myvolume",
        "azureFile": {
            "shareName": "acishare",
            "storageAccountName": "[parameters('storageaccountname')]",
            "storageAccountKey": "[parameters('storageaccountkey')]"
        }
      }]
    }
  }]
}
```

Die Vorlage enthält den Namen und den Schlüssel des Speicherkontos als Parameter, die in Form einer separaten Parameterdatei bereitgestellt werden können. Zum Auffüllen der Parameterdatei benötigen Sie drei Werte: den Namen des Speicherkontos, die Ressourcen-ID Ihres Azure-Schlüsseltresors und den geheimen Namen des Schlüsseltresors, den Sie zum Speichern des Speicherschlüssels verwendet haben. Wenn Sie die vorherigen Schritte ausgeführt haben, können Sie diese Werte mit dem folgenden Skript abrufen:

```azurecli-interactive
echo $STORAGE_ACCOUNT
echo $KEYVAULT_SECRET_NAME
az keyvault show --name $KEYVAULT_NAME --query [id] -o tsv
```

Einfügen der Werte in die Parameterdatei:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageaccountname": {
      "value": "<my_storage_account_name>"
    },    
   "storageaccountkey": {
      "reference": {
        "keyVault": {
          "id": "<my_keyvault_id>"
        },
        "secretName": "<my_storage_account_key_secret_name>"
      }
    }
  }
}
```

## <a name="deploy-the-container-and-manage-files"></a>Bereitstellen des Containers und Verwalten von Dateien

Mit der definierten Vorlage können Sie den Container erstellen und das zugehörige Volume mithilfe der Azure-Befehlszeilenschnittstelle einbinden. Wenn die Vorlagendatei den Namen *azuredeploy.json* und die Parameterdatei den Namen *azuredeploy.parameters.json* hat, lautet die Befehlszeile:

```azurecli-interactive
az group deployment create --name hellofilesdeployment --template-file azuredeploy.json --parameters @azuredeploy.parameters.json --resource-group myResourceGroup
```

Nach dem Start des Containers können Sie Dateien in der Freigabe am angegebenen Einbindungspfad verwalten.

>[!NOTE]
> Weitere Informationen zur Verwendung von Azure Resource Manager-Vorlagen und Parameterdateien und zur Bereitstellung mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Bereitstellen von Ressourcen mit Resource Manager-Vorlagen und der Azure-Befehlszeilenschnittstelle](../azure-resource-manager/resource-group-template-deploy-cli.md).

## <a name="next-steps"></a>Nächste Schritte

- Bereitstellen Ihres ersten Containers mithilfe der [Schnellstartanleitung](container-instances-quickstart.md) für Azure Container Instances
- Weitere Informationen über die [Beziehung zwischen Azure Container Instances und Containerorchestratoren](container-instances-orchestrator-relationship.md)

