---
title: "Azure Resource Manager-Vorlagenfunktionen – Ressourcen | Microsoft Docs"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Abrufen von Werten zu Ressourcen verwendet werden können."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/26/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 66c71906614e5d0c8e8531271444fc59a5cb779f
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="resource-functions-for-azure-resource-manager-templates"></a>Ressourcenfunktionen für Azure Resource Manager-Vorlagen

Der Ressourcen-Manager stellt die folgenden Funktionen zum Abrufen von Ressourcenwerten bereit:

* [listKeys und list{Value}](#listkeys)
* [providers](#providers)
* [Referenz](#reference)
* [Ressourcengruppe](#resourcegroup)
* [Ressourcen-ID](#resourceid)
* [Abonnement](#subscription)

Informationen zum Abrufen von Werten aus Parametern, Variablen oder der aktuellen Bereitstellung finden Sie unter [Funktionen für Bereitstellungswerte](resource-group-template-functions-deployment.md).

<a id="listkeys" />
<a id="list" />

## <a name="listkeys-and-listvalue"></a>listKeys und list{Value}
`listKeys(resourceName or resourceIdentifier, apiVersion)`

`list{Value}(resourceName or resourceIdentifier, apiVersion)`

Gibt die Werte für einen beliebigen Ressourcentyp zurück, der den list-Vorgang unterstützt. Am häufigsten wird `listKeys` verwendet. 

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| resourceName oder resourceIdentifier |Ja |string |Eindeutiger Bezeichner für die Ressource. |
| apiVersion |Ja |string |API-Version eines Ressourcen-Laufzeitstatus. In der Regel im Format **jjjj-mm-tt**. |

### <a name="remarks"></a>Anmerkungen

Jeder Vorgang, der mit **list** beginnt, kann als Funktion in der Vorlage verwendet werden. Zu den verfügbaren Vorgängen zählen neben „listKeys“ auch Vorgänge wie `list`, `listAdminKeys` und `listStatus`. Um zu bestimmen, welche Ressourcentypen einen list-Vorgang aufweisen, stehen die folgenden Optionen zur Verfügung:

* Zeigen Sie die [REST-API-Vorgänge](/rest/api/) für einen Ressourcenanbieter an, und suchen Sie nach List-Vorgängen. Speicherkonten weisen z. B. den [listKeys-Vorgang](/rest/api/storagerp/storageaccounts#StorageAccounts_ListKeys) auf.
* Verwenden Sie das PowerShell-Cmdlet [Get-AzureRmProviderOperation](/powershell/module/azurerm.resources/get-azurermprovideroperation). Im folgenden Beispiel werden alle List-Vorgänge für Speicherkonten abgerufen:

  ```powershell
  Get-AzureRmProviderOperation -OperationSearchString "Microsoft.Storage/*" | where {$_.Operation -like "*list*"} | FT Operation
  ```
* Verwenden Sie den folgenden Azure-CLI-Befehl und das JSON-Hilfsprogramm [jq](http://stedolan.github.io/jq/download/), um nur die list-Vorgänge zu filtern:

  ```azurecli
  azure provider operations show --operationSearchString */apiapps/* --json | jq ".[] | select (.operation | contains(\"list\"))"
  ```

Geben Sie die Ressource entweder mithilfe der [resourceId-Funktion](#resourceid) oder des Formats `{providerNamespace}/{resourceType}/{resourceName}` an.

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt, wie die Primär- und Sekundärschlüssel von einem Speicherkonto im Abschnitt „outputs“ zurückgegeben werden können:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountId": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "storageKeysOutput": {
            "value": "[listKeys(parameters('storageAccountId'), '2016-01-01')]",
            "type" : "object"
        }
    }
}
``` 

### <a name="return-value"></a>Rückgabewert

Das zurückgegebene Objekt von „listKeys“ hat das folgende Format:

```json
{
  "keys": [
    {
      "keyName": "key1",
      "permissions": "Full",
      "value": "{value}"
    },
    {
      "keyName": "key2",
      "permissions": "Full",
      "value": "{value}"
    }
  ]
}
```

Andere list-Funktionen weisen andere Rückgabeformate auf. Um das Format einer Funktion anzuzeigen, geben Sie es im Abschnitt „outputs“ wie in der Beispielvorlage dargestellt an. 

<a id="providers" />

## <a name="providers"></a>providers
`providers(providerNamespace, [resourceType])`

Gibt Informationen zu einem Ressourcenanbieter und den von ihm unterstützten Ressourcentypen zurück. Wenn Sie keinen Ressourcentyp angeben, gibt die Funktion alle unterstützten Typen für den Ressourcenanbieter zurück.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| providerNamespace |Ja |string |Namespace des Anbieters |
| resourceType |Nein |string |Der Ressourcentyp innerhalb des angegebenen Namespace. |

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt die Nutzungsweise der Anbieterfunktion:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "providerOutput": {
            "value": "[providers('Microsoft.Storage', 'storageAccounts')]",
            "type" : "object"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Jeder unterstützte Typ wird im folgenden Format zurückgegeben: 

```json
{
    "resourceType": "{name of resource type}",
    "locations": [ all supported locations ],
    "apiVersions": [ all supported API versions ]
}
```

Die Arraysortierung der zurückgegebenen Werte ist dabei nicht garantiert.

<a id="reference" />

## <a name="reference"></a>Referenz
`reference(resourceName or resourceIdentifier, [apiVersion])`

Gibt ein Objekt zurück, das den Laufzeitstatus einer Ressource darstellt.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| resourceName oder resourceIdentifier |Ja |string |Name oder eindeutiger Bezeichner einer Ressource |
| apiVersion |Nein |string |API-Version der angegebenen Ressource. Schließen Sie diesen Parameter ein, wenn die Ressource nicht innerhalb der gleichen Vorlage bereitgestellt wird. In der Regel im Format **jjjj-mm-tt**. |

### <a name="remarks"></a>Anmerkungen

Die Verweisfunktion leitet ihren Wert von einem Laufzeitstatus ab und kann somit nicht im Variablenabschnitt verwendet werden. Sie kann in Ausgabeabschnitten einer Vorlage verwendet werden. 

Mithilfe der Referenzfunktion können Sie implizit deklarieren, dass eine Ressource von einer anderen abhängt, wenn die referenzierte Ressource innerhalb der gleichen Vorlage zur Verfügung gestellt wird. Die „dependsOn“-Eigenschaft muss nicht zusätzlich verwendet werden. Die Funktion wird nicht ausgewertet, bis die Ressource, auf die verwiesen wird, die Bereitstellung abgeschlossen hat.

Um die Eigenschaftennamen und Werte für einen Ressourcentyp anzuzeigen, erstellen Sie eine Vorlage, die das Objekt im Abschnitt „outputs“ zurückgibt. Wenn Sie über eine Ressource dieses Typs verfügen, gibt Ihre Vorlage das Objekt zurück, ohne neue Ressourcen bereitzustellen. 

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird auf ein Speicherkonto verwiesen, das nicht in dieser Vorlage bereitgestellt wird, aber in der gleichen Ressourcengruppe vorhanden ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "type": "string"
        }
    },
    "resources": [],
    "outputs": {
        "ExistingStorage": {
            "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01')]",
            "type" : "object"
        }
    }
}
```

Alternativ können Sie die Ressource in der gleichen Vorlage bereitstellen und darauf verweisen.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "storageAccountName": { 
          "type": "string"
      }
  },
  "resources": [
    {
      "name": "[parameters('storageAccountName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-12-01",
      "sku": {
        "name": "Standard_LRS"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {
      "referenceOutput": {
          "type": "object",
          "value": "[reference(parameters('storageAccountName'))]"
      }
    }
}
``` 

In der Regel verwenden Sie die Verweisfunktion, um einen bestimmten Wert aus einem Objekt zurückzugeben, wie z. B. den BLOB-Endpunkt-URI oder den vollständig qualifizierten Domänennamen.

```json
"outputs": {
    "BlobUri": {
        "value": "[reference(concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName')), '2016-01-01').primaryEndpoints.blob]",
        "type" : "string"
    },
    "FQDN": {
        "value": "[reference(concat('Microsoft.Network/publicIPAddresses/', parameters('ipAddressName')), '2016-03-30').dnsSettings.fqdn]",
        "type" : "string"
    }
}
```

### <a name="return-value"></a>Rückgabewert

Jeder Ressourcentyp gibt andere Eigenschaften für die Verweisfunktion zurück. Die Funktion gibt kein einzelnes vordefiniertes Format zurück. Um die Eigenschaften für einen Ressourcentyp anzuzeigen, geben Sie das Objekt wie im Beispiel gezeigt im Abschnitt „outputs“ zurück.

<a id="resourcegroup" />

## <a name="resourcegroup"></a>Ressourcengruppe
`resourceGroup()`

Gibt ein Objekt zurück, das die aktuelle Ressourcengruppe darstellt. 

### <a name="examples"></a>Beispiele

Die folgende Vorlage gibt die Eigenschaften der Ressourcengruppe zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[resourceGroup()]",
            "type" : "object"
        }
    }
}
```

Die Funktion „resourceGroup“ wird häufig verwendet, um Ressourcen am gleichen Speicherort wie die Ressourcengruppe zu erstellen. Das folgende Beispiel nutzt den Speicherort der Ressourcengruppe, um einer Website den Speicherort zuzuweisen.

```json
"resources": [
   {
      "apiVersion": "2014-06-01",
      "type": "Microsoft.Web/sites",
      "name": "[parameters('siteName')]",
      "location": "[resourceGroup().location]",
      ...
   }
]
```

### <a name="return-value"></a>Rückgabewert

Das zurückgegebene Objekt hat das folgende Format:

```json
{
  "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}",
  "name": "{resourceGroupName}",
  "location": "{resourceGroupLocation}",
  "tags": {
  },
  "properties": {
    "provisioningState": "{status}"
  }
}
```

<a id="resourceid" />

## <a name="resourceid"></a>resourceId
`resourceId([subscriptionId], [resourceGroupName], resourceType, resourceName1, [resourceName2]...)`

Gibt den eindeutigen Bezeichner einer Ressource zurück. Diese Funktion wird verwendet, wenn der Ressourcenname zweideutig ist oder nicht innerhalb der gleichen Vorlage zur Verfügung gestellt wird. 

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| subscriptionId |Nein |Zeichenfolge (im GUID-Format) |Der Standardwert ist das aktuelle Abonnement. Geben Sie diesen Wert an, wenn Sie eine Ressource in einem anderen Abonnement abrufen möchten. |
| ResourceGroupName |Nein |string |Der Standardwert ist die aktuelle Ressourcengruppe. Geben Sie diesen Wert an, wenn Sie eine Ressource in einer anderen Ressourcengruppe abrufen möchten. |
| resourceType |Ja |string |Ressourcentyp einschließlich Namespace von Ressourcenanbieter. |
| resourceName1 |Ja |string |Name der Ressource. |
| resourceName2 |Nein |string |Nächstes Ressourcen-Namensegment, wenn die Ressource geschachtelt ist. |

### <a name="examples"></a>Beispiele

Im folgenden Beispiel wird die Ressourcen-ID für ein Speicherkonto in der Ressourcengruppe zurückgegeben:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "resourceIdOutput": {
            "value": "[resourceId('Microsoft.Storage/storageAccounts','examplestorage')]",
            "type" : "string"
        }
    }
}
```

Das folgende Beispiel zeigt, wie die Ressourcen-IDs für eine Website in einer anderen Ressourcengruppe und für eine Datenbank in einer anderen Ressourcengruppe abgerufen werden können:

```json
[resourceId('otherResourceGroup', 'Microsoft.Web/sites', parameters('siteName'))]
[resourceId('otherResourceGroup', 'Microsoft.SQL/servers/databases', parameters('serverName'), parameters('databaseName'))]
```

Sie müssen diese Funktion oft nutzen, wenn Sie ein Speicherkonto oder einen virtuellen Computer in einer alternativen Ressourcengruppe verwenden. Das Speicherkonto oder der virtuelle Computer können über mehrere Ressourcengruppen hinweg genutzt werden. Daher sollten Sie diese beim Löschvorgang für eine einzelne Ressourcengruppe nicht löschen. Das folgende Beispiel zeigt, wie eine Ressource von einer externen Ressourcengruppe leicht verwendet werden kann:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
      "virtualNetworkName": {
          "type": "string"
      },
      "virtualNetworkResourceGroup": {
          "type": "string"
      },
      "subnet1Name": {
          "type": "string"
      },
      "nicName": {
          "type": "string"
      }
  },
  "variables": {
      "vnetID": "[resourceId(parameters('virtualNetworkResourceGroup'), 'Microsoft.Network/virtualNetworks', parameters('virtualNetworkName'))]",
      "subnet1Ref": "[concat(variables('vnetID'),'/subnets/', parameters('subnet1Name'))]"
  },
  "resources": [
  {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[parameters('nicName')]",
      "location": "[parameters('location')]",
      "properties": {
          "ipConfigurations": [{
              "name": "ipconfig1",
              "properties": {
                  "privateIPAllocationMethod": "Dynamic",
                  "subnet": {
                      "id": "[variables('subnet1Ref')]"
                  }
              }
          }]
       }
  }]
}
```

### <a name="return-value"></a>Rückgabewert

Der Bezeichner wird im folgenden Format zurückgeben:

```json
/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
```

<a id="subscription" />

## <a name="subscription"></a>Abonnement
`subscription()`

Gibt Details zum Abonnement für die aktuelle Bereitstellung zurück. 

### <a name="examples"></a>Beispiele

Das folgende Beispiel zeigt ein Abrufen der subscription-Funktion im Abschnitt „outputs“. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[subscription()]",
            "type" : "object"
        }
    }
}
```

### <a name="return-value"></a>Rückgabewert

Die Funktion gibt das folgende Format zurück:

```json
{
    "id": "/subscriptions/{subscription-id}",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}",
    "displayName": "{name-of-subscription}"
}
```

## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


