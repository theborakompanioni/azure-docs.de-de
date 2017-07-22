---
title: "Azure Resource Manager-Vorlagenfunktionen – Bereitstellung | Microsoft Docs"
description: "Hier werden die Funktionen beschrieben, die in einer Azure Resource Manager-Vorlage zum Abrufen von Informationen zur Bereitstellung verwendet werden können."
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
ms.date: 06/13/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 8021f8641ff3f009104082093143ec8eb087279e
ms.openlocfilehash: d7e6bcd669d40cb19de44b646505856ecd8f51a0
ms.contentlocale: de-de
ms.lasthandoff: 07/21/2017

---
# <a name="deployment-functions-for-azure-resource-manager-templates"></a>Bereitstellungsfunktionen für Azure Resource Manager-Vorlagen 

Der Ressourcen-Manager stellt die folgenden Funktionen zum Abrufen von Werten aus Vorlagenabschnitten sowie von bereitstellungsbezogenen Werten bereit:

* [deployment](#deployment)
* [parameters](#parameters)
* [Variablen](#variables)

Informationen zum Abrufen von Werten aus Ressourcen, Ressourcengruppen oder Abonnements finden Sie unter [Ressourcenfunktionen](resource-group-template-functions-resource.md).

<a id="deployment" />

## <a name="deployment"></a>Bereitstellung
`deployment()`

Gibt Informationen zum aktuellen Bereitstellungsvorgang zurück.

### <a name="return-value"></a>Rückgabewert

Diese Funktion gibt das Objekt zurück, das während der Bereitstellung übergeben wird. Die Eigenschaften im zurückgegebenen Objekt hängen davon ab, ob das Bereitstellungsobjekt als Link oder als Inlineobjekt übergeben wird. Wenn das Bereitstellungsobjekt als Inlineobjekt übergeben wird, z.B. bei Verwendung des **-TemplateFile**-Parameters in Azure PowerShell zum Verweisen auf eine lokale Datei, hat das zurückgegebene Objekt das folgende Format:

```json
{
    "name": "",
    "properties": {
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [
            ],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

Wenn das Objekt als Link übergeben wird, z.B. bei Verwendung des **-TemplateUri**-Parameters zum Verweisen auf ein Remoteobjekt, hat das zurückgegebene Objekt folgendes Format: 

```json
{
    "name": "",
    "properties": {
        "templateLink": {
            "uri": ""
        },
        "template": {
            "$schema": "",
            "contentVersion": "",
            "parameters": {},
            "variables": {},
            "resources": [],
            "outputs": {}
        },
        "parameters": {},
        "mode": "",
        "provisioningState": ""
    }
}
```

### <a name="remarks"></a>Anmerkungen

Sie können „deployment()“ verwenden, um basierend auf dem URI der übergeordneten Vorlage eine Verknüpfung mit einer anderen Vorlage zu erstellen.

```json
"variables": {  
    "sharedTemplateUrl": "[uri(deployment().properties.templateLink.uri, 'shared-resources.json')]"  
}
```  

### <a name="example"></a>Beispiel

Im folgenden Beispiel wird das Bereitstellungsobjekt zurückgegeben:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [],
    "outputs": {
        "subscriptionOutput": {
            "value": "[deployment()]",
            "type" : "object"
        }
    }
}
```

Im vorherigen Beispiel wird das folgende Objekt zurückgegeben:

```json
{
  "name": "deployment",
  "properties": {
    "template": {
      "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "resources": [],
      "outputs": {
        "subscriptionOutput": {
          "type": "Object",
          "value": "[deployment()]"
        }
      }
    },
    "parameters": {},
    "mode": "Incremental",
    "provisioningState": "Accepted"
  }
}
```

<a id="parameters" />

## <a name="parameters"></a>parameters
`parameters(parameterName)`

Gibt einen Parameterwert zurück. Der spezifizierte Parametername muss im Parameterabschnitt der Vorlage definiert werden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| parameterName |Ja |string |Der Name des zurückzugebenden Parameter. |

### <a name="return-value"></a>Rückgabewert

Der Wert des angegebenen Parameters.

### <a name="remarks"></a>Anmerkungen

In der Regel verwenden Sie Parameter zum Festlegen von Ressourcenwerten. Im folgenden Beispiel wird der Name der Website mit dem Parameterwert festgelegt, der während der Bereitstellung übergeben wird.

```json
"parameters": { 
  "siteName": {
      "type": "string"
  }
},
"resources": [
   {
      "apiVersion": "2016-08-01",
      "name": "[parameters('siteName')]",
      "type": "Microsoft.Web/Sites",
      ...
   }
]
```

### <a name="example"></a>Beispiel

Die folgenden Beispiele zeigen eine vereinfachte Nutzungsweise der Parameterfunktion.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "stringParameter": {
            "type" : "string",
            "defaultValue": "option 1"
        },
        "intParameter": {
            "type": "int",
            "defaultValue": 1
        },
        "objectParameter": {
            "type": "object",
            "defaultValue": {"one": "a", "two": "b"}
        },
        "arrayParameter": {
            "type": "array",
            "defaultValue": [1, 2, 3]
        },
        "crossParameter": {
            "type": "string",
            "defaultValue": "[parameters('stringParameter')]"
        }
    },
    "variables": {},
    "resources": [],
    "outputs": {
        "stringOutput": {
            "value": "[parameters('stringParameter')]",
            "type" : "string"
        },
        "intOutput": {
            "value": "[parameters('intParameter')]",
            "type" : "int"
        },
        "objectOutput": {
            "value": "[parameters('objectParameter')]",
            "type" : "object"
        },
        "arrayOutput": {
            "value": "[parameters('arrayParameter')]",
            "type" : "array"
        },
        "crossOutput": {
            "value": "[parameters('crossParameter')]",
            "type" : "string"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| stringOutput | String | option 1 |
| intOutput | int | 1 |
| objectOutput | Objekt | {"one": "a", "two": "b"} |
| arrayOutput | Array | [1, 2, 3] |
| crossOutput | String | option 1 |

<a id="variables" />

## <a name="variables"></a>variables
`variables(variableName)`

Gibt den Wert der Variablen zurück. Der angegebene Variablenname muss im Variablenabschnitt der Vorlage definiert werden.

### <a name="parameters"></a>Parameter

| Parameter | Erforderlich | Typ | Beschreibung |
|:--- |:--- |:--- |:--- |
| variableName |Ja |String |Der Name der zurückzugebenden Variable. |

### <a name="return-value"></a>Rückgabewert

Der Wert der angegebenen Variablen.

### <a name="remarks"></a>Anmerkungen

In der Regel verwenden Sie Variablen, um Ihre Vorlage zu vereinfachen, indem Sie komplexe Werte nur einmal erstellen. Das folgende Beispiel erstellt einen eindeutigen Namen für ein Speicherkonto.

```json
"variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id))]"
},
"resources": [
    {
        "type": "Microsoft.Storage/storageAccounts",
        "name": "[variables('storageName')]",
        ...
    },
    {
        "type": "Microsoft.Compute/virtualMachines",
        "dependsOn": [
            "[variables('storageName')]"
        ],
        ...
    }
],
```

### <a name="example"></a>Beispiel

Die Beispielvorlage gibt unterschiedliche Variablenwerte zurück.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {
        "var1": "myVariable",
        "var2": [ 1,2,3,4 ],
        "var3": "[ variables('var1') ]",
        "var4": {
            "property1": "value1",
            "property2": "value2"
        }
    },
    "resources": [],
    "outputs": {
        "exampleOutput1": {
            "value": "[variables('var1')]",
            "type" : "string"
        },
        "exampleOutput2": {
            "value": "[variables('var2')]",
            "type" : "array"
        },
        "exampleOutput3": {
            "value": "[variables('var3')]",
            "type" : "string"
        },
        "exampleOutput4": {
            "value": "[variables('var4')]",
            "type" : "object"
        }
    }
}
```

Die Ausgabe aus dem vorherigen Beispiel mit den Standardwerten lautet:

| Name | Typ | Wert |
| ---- | ---- | ----- |
| exampleOutput1 | String | myVariable |
| exampleOutput2 | Array | [1, 2, 3, 4] |
| exampleOutput3 | String | myVariable |
| exampleOutput4 |  Objekt | {"property1": "value1", "property2": "value2"} |

## <a name="next-steps"></a>Nächste Schritte
* Eine Beschreibung der Abschnitte in einer Azure Resource Manager-Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Zusammenführen mehrerer Vorlagen finden Sie unter [Verwenden von verknüpften Vorlagen bei der Bereitstellung von Azure-Ressourcen](resource-group-linked-templates.md).
* Informationen dazu, wie Sie beim Erstellen eines Ressourcentyps eine bestimmte Anzahl von Durchläufen ausführen, finden Sie unter [Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager](resource-group-create-multiple.md).
* Informationen zum Bereitstellen der erstellten Vorlage finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen ](resource-group-template-deploy.md).


