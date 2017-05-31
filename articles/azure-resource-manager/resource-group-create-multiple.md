---
title: Bereitstellen mehrerer Instanzen von Azure-Ressourcen | Microsoft-Dokumentation
description: Verwenden Sie den copy-Vorgang und Arrays in einer Azure-Ressourcen-Manager-Vorlage, um sie beim Bereitstellen von Ressourcen mehrere Male zu durchlaufen.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 94d95810-a87b-460f-8e82-c69d462ac3ca
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/12/2017
ms.author: tomfitz
ms.translationtype: Human Translation
ms.sourcegitcommit: 9568210d4df6cfcf5b89ba8154a11ad9322fa9cc
ms.openlocfilehash: a8e35456af8c9f2cf1bf9e9c364e33641f29a477
ms.contentlocale: de-de
ms.lasthandoff: 05/15/2017


---
# <a name="deploy-multiple-instances-of-a-resource-or-property-in-azure-resource-manager-templates"></a>Bereitstellen mehrerer Instanzen einer Ressource oder Eigenschaft in Azure Resource Manager-Vorlagen
In diesem Thema erfahren Sie, wie Sie die Azure-Ressourcen-Manager-Vorlage durchlaufen können, um mehrere Instanzen einer Ressource zu erstellen.

## <a name="resource-iteration"></a>Ressourceniteration
Fügen Sie zum Erstellen mehrerer Instanzen eines Ressourcentyps ein `copy`-Element zum Ressourcentyp hinzu. Im copy-Element geben Sie die Anzahl von Iterationen und einen Namen für diese Schleife an. Der count-Wert muss eine positive ganze Zahl sein und darf 800 nicht überschreiten. Resource Manager erstellt die Ressourcen gleichzeitig. Aus diesem Grund ist die Reihenfolge, in der sie erstellt werden, nicht garantiert. Um Ressourcen durch mehrfaches Durchlaufen zu erstellen, finden Sie unter [Serielle Kopie](#serial-copy). 

Die Ressource zum mehrfachen Erstellen übernimmt das folgende Format:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        }
    ],
    "outputs": {}
}
```

Beachten Sie, dass der Name der einzelnen Ressourcen die `copyIndex()`-Funktion enthält, die die aktuelle Iteration in der Schleife zurückgibt. `copyIndex()` ist nullbasiert. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex())]",
```

Namen:

* storage0
* storage1
* storage2

Zum Versetzen des Indexwerts können Sie einen Wert in der copyIndex()-Funktion übergeben. Die Anzahl von durchzuführenden Durchläufen wird weiterhin im copy-Element angegeben, aber der Wert von copyIndex wird um den angegebenen Wert versetzt. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"name": "[concat('storage', copyIndex(1))]",
```

Namen:

* storage1
* storage2
* storage3

Der „copy“-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. Verwenden Sie die Funktion `length` für das Array, um die Anzahl von Iterationen anzugeben, und `copyIndex`, um den aktuellen Index im Array abzurufen. Im folgenden Beispiel werden die unten aufgeführten Namen erstellt:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "contoso", 
         "fabrikam", 
         "coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('storage', parameters('org')[copyIndex()])]", 
      "copy": { 
         "name": "storagecopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      ...
  } 
]
```

Namen:

* storagecontoso
* storagefabrikam
* storagecoho

## <a name="serial-copy"></a>Serielle Kopie

Wenn Sie das copy-Element verwenden, um mehrere Instanzen eines Ressourcentyps zu erstellen, stellt Resource Manager diese Instanzen standardmäßig parallel bereit. Es ist aber möglicherweise sinnvoll, anzugeben, dass die Ressource sequenziell bereitgestellt werden. Wenn Sie z.B. eine Produktionsumgebung aktualisieren, möchten Sie die Updates möglicherweise staffeln, sodass nur eine bestimmte Anzahl von Ressourcen gleichzeitig aktualisiert wird.

Resource Manager bietet Eigenschaften für das copy-Element, mit denen Sie mehrere Instanzen seriell bereitstellen können. Legen Sie im copy-Element `mode` auf **serial** und `batchSize` auf die Anzahl von Instanzen fest, die gleichzeitig bereitgestellt werden sollen. Im seriellen Modus erstellt Resource Manager eine Abhängigkeit von früheren Instanzen in der Schleife, sodass ein Batch erst dann gestartet wird, wenn das vorherige Batch abgeschlossen wurde.

```json
"copy": {
    "name": "iterator",
    "count": "[parameters('numberToDeploy')]",
    "mode": "serial",
    "batchSize": 2
},
```

Die mode-Eigenschaft akzeptiert auch **parallel**, wobei es sich um den Standardwert handelt.

Um das serielle Kopieren zu testen, ohne tatsächlich Ressourcen zu erstellen, verwenden Sie folgende Vorlage, die leere geschachtelte Vorlagen bereitstellt:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberToDeploy": {
      "type": "int",
      "minValue": 2,
      "defaultValue": 5
    }
  },
  "resources": [
    {
      "apiVersion": "2015-01-01",
      "type": "Microsoft.Resources/deployments",
      "name": "[concat('loop-', copyIndex())]",
      "copy": {
        "name": "iterator",
        "count": "[parameters('numberToDeploy')]",
        "mode": "serial",
        "batchSize": 1
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [],
          "outputs": {
          }
        }
      }
    }
  ],
  "outputs": {
  }
}
```

Beachten Sie im Bereitstellungsverlauf, dass die geschachtelten Bereitstellungen sequenziell verarbeitet werden.

![Serielle Bereitstellung](./media/resource-group-create-multiple/serial-copy.png)

Das folgende Beispiel bietet ein etwas realistischeres Szenario und stellt gleichzeitig zwei Instanzen eines virtuellen Linux-Computers aus einer geschachtelten Vorlage bereit:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "User name for the Virtual Machine."
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Password for the Virtual Machine."
            }
        },
        "dnsLabelPrefix": {
            "type": "string",
            "metadata": {
                "description": "Unique DNS Name for the Public IP used to access the Virtual Machine."
            }
        },
        "ubuntuOSVersion": {
            "type": "string",
            "defaultValue": "16.04.0-LTS",
            "allowedValues": [
                "12.04.5-LTS",
                "14.04.5-LTS",
                "15.10",
                "16.04.0-LTS"
            ],
            "metadata": {
                "description": "The Ubuntu version for the VM. This will pick a fully patched image of this given Ubuntu version."
            }
        }
    },
    "variables": {
        "templatelink": "https://raw.githubusercontent.com/rjmax/Build2017/master/Act1.TemplateEnhancements/Chapter03.LinuxVM.json"
    },
    "resources": [
        {
            "apiVersion": "2015-01-01",
            "name": "[concat('nestedDeployment',copyIndex())]",
            "type": "Microsoft.Resources/deployments",
            "copy": {
                "name": "myCopySet",
                "count": 4,
                "mode": "serial",
                "batchSize": 2
            },
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "uri": "[variables('templatelink')]",
                    "contentVersion": "1.0.0.0"
                },
                "parameters": {
                    "adminUsername": {
                        "value": "[parameters('adminUsername')]"
                    },
                    "adminPassword": {
                        "value": "[parameters('adminPassword')]"
                    },
                    "dnsLabelPrefix": {
                        "value": "[parameters('dnsLabelPrefix')]"
                    },
                    "ubuntuOSVersion": {
                        "value": "[parameters('ubuntuOSVersion')]"
                    },
                    "index":{
                        "value": "[copyIndex()]"
                    }
                }
            }
        }
    ]
}
```

## <a name="depend-on-resources-in-a-loop"></a>Abhängigkeit von Ressourcen in einer Schleife
Sie geben an, dass eine Ressource nach einer anderen Ressource bereitgestellt wird, indem Sie das `dependsOn`-Element verwenden. Um eine Ressource bereitzustellen, die von der Sammlung von Ressourcen in einer Schleife abhängt, geben Sie den Namen der Kopierschleife im dependsOn-Element an. Das folgende Beispiel zeigt, wie drei Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Die vollständige Definition des virtuellen Computers ist dabei nicht angegeben. Beachten Sie, dass „name“ für das copy-Element auf `storagecopy` und auch das dependsOn-Element für die virtuellen Computer auf `storagecopy` festgelegt ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2016-01-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat(copyIndex(),'storage', uniqueString(resourceGroup().id))]",
            "location": "[resourceGroup().location]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {},
            "copy": {
                "name": "storagecopy",
                "count": 3
            }
        },
        {
            "apiVersion": "2015-06-15", 
            "type": "Microsoft.Compute/virtualMachines", 
            "name": "[concat('VM', uniqueString(resourceGroup().id))]",  
            "dependsOn": ["storagecopy"],
            ...
        }
    ],
    "outputs": {}
}
```

## <a name="create-multiple-instances-of-a-child-resource"></a>Erstellen mehrerer Instanzen einer untergeordneten Ressource
Für eine untergeordnete Ressource kann keine Kopierschleife verwendet werden. Um mehrere Instanzen einer Ressource zu erstellen, die Sie in der Regel als innerhalb einer anderen Ressource geschachtelt definieren, müssen Sie diese Ressource stattdessen als Ressource oberster Ebene erstellen. Sie definieren die Beziehung zur übergeordneten Ressource mithilfe der Eigenschaften „type“ und „name“.

Angenommen, Sie definieren ein Dataset als untergeordnete Ressource innerhalb einer Data Factory.

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
    "resources": [
    {
        "type": "datasets",
        "name": "exampleDataSet",
        "dependsOn": [
            "exampleDataFactory"
        ],
        ...
    }
}]
```

Um mehrere Instanzen von Datasets zu erstellen, verschieben Sie sie außerhalb der Data Factory. Das Dataset muss sich auf der gleichen Ebene wie die Data Factory befinden, ist aber immer noch eine untergeordnete Ressource der Data Factory. Sie erhalten die Beziehung zwischen Dataset und Data Factory mithilfe der Eigenschaften „type“ und „name“ bei. Da „type“ nicht mehr von seiner Position in der Vorlage abgeleitet werden kann, müssen Sie den vollqualifizierten Typ im folgenden Format angeben: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

Um eine Über-/Unterordnungsbeziehung mit einer Instanz der Data Factory herzustellen, geben Sie einen Namen für das Dataset an, das den Namen der übergeordneten Ressource enthält. Verwenden Sie das folgende Format: `{parent-resource-name}/{child-resource-name}`.  

Das folgende Beispiel zeigt die Implementierung:

```json
"resources": [
{
    "type": "Microsoft.DataFactory/datafactories",
    "name": "exampleDataFactory",
    ...
},
{
    "type": "Microsoft.DataFactory/datafactories/datasets",
    "name": "[concat('exampleDataFactory', '/', 'exampleDataSet', copyIndex())]",
    "dependsOn": [
        "exampleDataFactory"
    ],
    "copy": { 
        "name": "datasetcopy", 
        "count": "3" 
    } 
    ...
}]
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).


