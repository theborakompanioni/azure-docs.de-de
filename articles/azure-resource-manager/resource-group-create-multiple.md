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
ms.date: 02/24/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 04a3866f88b00486c30c578699d34cd6e8e776d7
ms.openlocfilehash: 056ee5e67b9a6d396586c53b04d50f89e6fbb560
ms.lasthandoff: 02/27/2017


---
# <a name="deploy-multiple-instances-of-resources-in-azure-resource-manager-templates"></a>Bereitstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager-Vorlagen
In diesem Thema erfahren Sie, wie Sie die Azure-Ressourcen-Manager-Vorlage durchlaufen können, um mehrere Instanzen einer Ressource zu erstellen.

## <a name="copy-copyindex-and-length"></a>"copy", "copyIndex" und "length"
Um innerhalb der Ressource mehrere Instanzen zu erstellen, können Sie ein **copy** -Objekt definieren, das die Anzahl der Iterationen angibt. Die Kopie hat das folgende Format:

```json
"copy": { 
    "name": "websitescopy", 
    "count": "[parameters('count')]" 
} 
```

Sie können mit der **copyIndex()**-Funktion auf den aktuellen Iterationswert zugreifen. Im folgenden Beispiel wird „copyIndex“ mit der „concat“-Funktion verwendet, um einen Namen zu erstellen.

```json
[concat('examplecopy-', copyIndex())]
```

Wenn Sie mehrere Ressourcen aus einem Array von Werten erstellen, können Sie die Anzahl über die Funktion **length** angeben. Das Array wird als Parameter für die length-Funktion bereitgestellt.

```json
"copy": {
    "name": "websitescopy",
    "count": "[length(parameters('siteNames'))]"
}
```

Das „copy“-Objekt kann nur auf eine Ressource auf oberster Ebene angewendet werden. Es kann nicht auf eine Eigenschaft für einen Ressourcentyp oder auf eine untergeordnete Ressource angewendet werden. In diesem Thema wird jedoch veranschaulicht, wie mehrere Elemente für eine Eigenschaft angegeben und mehrere Instanzen einer untergeordneten Ressource erstellt werden. Im folgenden Beispiel mit Pseudocode wird gezeigt, wo „copy“ angewendet werden kann:

```json
"resources": [
  {
    "type": "{provider-namespace-and-type}",
    "name": "parentResource",
    "copy": {  
      /* yes, copy can be applied here */
    },
    "properties": {
      "exampleProperty": {
        /* no, copy cannot be applied here */
      }
    },
    "resources": [
      {
        "type": "{provider-type}",
        "name": "childResource",
        /* copy can be applied if resource is promoted to top level */ 
      }
    ]
  }
] 
```

Wenngleich Sie **copy** nicht auf eine Eigenschaft anwenden können, ist diese Eigenschaft weiter Teil der Iterationen der Ressource, die die Eigenschaft enthält. Aus diesem Grund können Sie **copyIndex()** in der Eigenschaft verwenden, um Werte anzugeben.

Es gibt mehrere Szenarien, bei denen Sie eine Iteration für eine Eigenschaft in einer Ressource durchführen möchten. Dies ist z.B. der Fall, wenn Sie mehrere Datenträger für einen virtuellen Computer angeben möchten. Informationen zum Durchführen einer Iteration für eine Eigenschaft finden Sie unter [Erstellen mehrerer Instanzen, wenn „copy“ nicht funktioniert](#create-multiple-instances-when-copy-wont-work). 

Informationen zum Arbeiten mit untergeordneten Ressourcen finden Sie unter [Erstellen mehrerer Instanzen einer untergeordneten Ressource](#create-multiple-instances-of-a-child-resource).

## <a name="use-index-value-in-name"></a>Verwenden des Indexwerts in Namen
Sie können den copy-Vorgang verwenden, um mehrere Instanzen einer Ressource mit einem eindeutigen Namen basierend auf dem inkrementellen Index zu erstellen. Es kann beispielsweise sein, dass Sie am Ende des Namens jeder bereitgestellten Ressource eine eindeutige Zahl hinzufügen möchten. So stellen Sie drei Websites mit den folgenden Namen bereit:

* examplecopy-0
* examplecopy-1
* examplecopy-2

Verwenden Sie die folgende Vorlage:

```json
"parameters": { 
  "count": { 
    "type": "int", 
    "defaultValue": 3 
  } 
}, 
"resources": [ 
  { 
      "name": "[concat('examplecopy-', copyIndex())]", 
      "type": "Microsoft.Web/sites", 
      "location": "East US", 
      "apiVersion": "2015-08-01",
      "copy": { 
         "name": "websitescopy", 
         "count": "[parameters('count')]" 
      }, 
      "properties": {
          "serverFarmId": "hostingPlanName"
      }
  } 
]
```

## <a name="offset-index-value"></a>Versetzen des Indexwerts
Im vorherigen Beispiel reicht der Indexwert von 0 bis 2. Zum Versetzen des Indexwerts können Sie einen Wert in der **copyIndex()**-Funktion übergeben, z. B. **copyIndex(1)**. Die Anzahl von durchzuführenden Durchläufen wird weiterhin im copy-Element angegeben, aber der Wert von copyIndex wird um den angegebenen Wert versetzt. Wenn Sie dieselbe Vorlage wie im vorherigen Beispiel verwenden, dieses Mal aber **copyIndex(1)** angeben, werden also drei Websites mit den folgenden Namen bereitgestellt:

* examplecopy-1
* examplecopy-2
* examplecopy-3

## <a name="use-copy-with-array"></a>Verwenden von „copy“ mit Arrays
Der „copy“-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. So stellen Sie drei Websites mit den folgenden Namen bereit:

* examplecopy-Contoso
* examplecopy-Fabrikam
* examplecopy-Coho

Verwenden Sie die folgende Vorlage:

```json
"parameters": { 
  "org": { 
     "type": "array", 
     "defaultValue": [ 
         "Contoso", 
         "Fabrikam", 
         "Coho" 
      ] 
  }
}, 
"resources": [ 
  { 
      "name": "[concat('examplecopy-', parameters('org')[copyIndex()])]", 
      "type": "Microsoft.Web/sites", 
      "location": "East US", 
      "apiVersion": "2015-08-01",
      "copy": { 
         "name": "websitescopy", 
         "count": "[length(parameters('org'))]" 
      }, 
      "properties": {
          "serverFarmId": "hostingPlanName"
      } 
  } 
]
```

Natürlich können Sie die Anzahl für „copy“ auf einen anderen Wert als die Länge des Arrays festlegen. Sie können beispielsweise ein Array mit vielen Werten erstellen und dann einen Parameterwert übergeben, der festlegt, wie viele der Arrayelemente bereitgestellt werden sollen. In diesem Fall legen Sie die copy-Anzahl wie im ersten Beispiel gezeigt fest. 

## <a name="depend-on-resources-in-a-loop"></a>Abhängigkeit von Ressourcen in einer Schleife
Sie können angeben, dass eine Ressource nach einer anderen Ressource bereitgestellt wird, indem Sie das **dependsOn**-Element verwenden. Um eine Ressource bereitzustellen, die von der Sammlung von Ressourcen in einer Schleife abhängt, geben Sie den Namen der Kopierschleife im **dependsOn**-Element an. Das folgende Beispiel zeigt, wie drei Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Die vollständige Definition des virtuellen Computers ist dabei nicht angegeben. Beachten Sie, dass **name** für das „copy“-Element auf **storagecopy** und auch das **dependsOn**-Element für die virtuellen Computer auf** storagecopy** festgelegt ist.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "resources": [
        {
            "apiVersion": "2015-06-15",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id), copyIndex())]",
            "location": "[resourceGroup().location]",
            "properties": {
                "accountType": "Standard_LRS"
            },
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
Für eine untergeordnete Ressource kann keine Kopierschleife verwendet werden. Um mehrere Instanzen einer Ressource zu erstellen, die Sie in der Regel als innerhalb einer anderen Ressource geschachtelt definieren, müssen Sie diese Ressource stattdessen als Ressource oberster Ebene erstellen. Sie definieren die Beziehung zur übergeordneten Ressource mithilfe der Eigenschaften **type** und **name**.

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

Um mehrere Instanzen von Datasets zu erstellen, verschieben Sie sie außerhalb der Data Factory. Das Dataset muss sich auf der gleichen Ebene wie die Data Factory befinden, ist aber immer noch eine untergeordnete Ressource der Data Factory. Sie erhalten die Beziehung zwischen Dataset und Data Factory mithilfe der Eigenschaften **type** und **name** bei. Da „type“ nicht mehr von seiner Position in der Vorlage abgeleitet werden kann, müssen Sie den vollqualifizierten Typ im folgenden Format angeben: `{resource-provider-namespace}/{parent-resource-type}/{child-resource-type}`.

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

## <a name="create-multiple-instances-when-copy-wont-work"></a>Erstellen mehrerer Instanzen, wenn „copy“ nicht funktioniert
Sie können **copy** nur für Ressourcentypen verwenden, nicht für Eigenschaften innerhalb eines Ressourcentyps. Diese Anforderung kann problematisch werden, wenn Sie mehrere Instanzen von etwas erstellen möchten, das Teil einer Ressource ist. Ein häufiges Szenario ist die Erstellung mehrerer Datenträger für einen virtuellen Computer. Sie können **copy** nicht für die Datenträger verwenden, da **dataDisks** eine Eigenschaft des virtuellen Computers und nicht sein eigener Ressourcentyp ist. Stattdessen erstellen Sie ein Array mit so vielen Datenträgern wie benötigt, und übergeben die tatsächliche Anzahl zu erstellender Datenträger. In der Definition des virtuellen Computers verwenden Sie die **take** -Funktion, um nur die Anzahl von Elementen aus dem Array abzurufen, die Sie tatsächlich wünschen.

Ein vollständiges Beispiel für dieses Muster wird finden Sie in der Vorlage [Create a VM with a dynamic selection of data disks](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) (Erstellen eines virtuellen Computers mit der dynamischen Auswahl von Datenträgern).

Die relevanten Abschnitte der Bereitstellungsvorlage sind im folgenden Beispiel aufgeführt. Ein Großteil der Vorlage wurde entfernt, um die Abschnitte zum dynamischen Erstellen von Datenträgern hervorzuheben. Beachten Sie den Parameter **numDataDisks** , mit dem Sie die Anzahl der zu erstellenden Datenträger übergeben können. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numDataDisks": {
      "type": "int",
      "maxValue": 64,
      "metadata": {
        "description": "This parameter allows you to select the number of disks you want"
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'dynamicdisk')]",
    "sizeOfDataDisksInGB": 100,
    "diskCaching": "ReadWrite",
    "diskArray": [
      {
        "name": "datadisk1",
        "lun": 0,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk1.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk2",
        "lun": 1,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk2.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk3",
        "lun": 2,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk3.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk4",
        "lun": 3,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk4.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      ...
      {
        "name": "datadisk63",
        "lun": 62,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk63.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      },
      {
        "name": "datadisk64",
        "lun": 63,
        "vhd": {
          "uri": "[concat('http://', variables('storageAccountName'),'.blob.core.windows.net/vhds/', 'datadisk64.vhd')]"
        },
        "createOption": "Empty",
        "caching": "[variables('diskCaching')]",
        "diskSizeGB": "[variables('sizeOfDataDisksInGB')]"
      }
    ]
  },
  "resources": [
    ...
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        ...
        "storageProfile": {
          ...
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```

Sie können die **take**-Funktion und das **copy**-Element zusammen verwenden, wenn Sie mehrere Instanzen einer Ressource mit einer variablen Anzahl von Elementen für eine Eigenschaft benötigen. Angenommen, Sie müssen mehrere virtuelle Computer erstellen, doch jeder davon hat eine andere Anzahl von Datenträgern. Um jeden Datenträger mit einem Namen zu versehen, der den zugehörigen virtuellen Computer bestimmt, legen Sie Ihr Array mit Datenträgern in einer getrennten Vorlage ab. Fügen Sie Parameter für den Namen des virtuellen Computers und die zurückzugebende Anzahl von Datenträgern hinzu. Geben Sie im Abschnitt „outputs“ die Anzahl angegebener Elemente zurück.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    "numDataDisks": {
      "type": "int",
      "maxValue": 16,
      "metadata": {
        "description": "This parameter allows the user to select the number of disks they want"
      }
    }
  },
  "variables": {
    "diskArray": [
      {
        "name": "[concat(parameters('vmName'), '-datadisk1')]",
        "vhd": {
          "uri": "[concat('http://', parameters('storageAccountName'),'.blob.core.windows.net/vhds/', parameters('vmName'), '-datadisk1.vhd')]"
        },
        ...
      },
      ...
    ],
  },
  "resources": [
  ],
  "outputs": {
    "result": {
      "type": "array",
      "value": "[take(variables('diskArray'),parameters('numDataDisks'))]"
    }
  }
}
``` 

Fügen Sie in der übergeordneten Vorlage Parameter für die Anzahl der virtuellen Computer und ein Array für die Anzahl der Datenträger jedes virtuellen Computers hinzu.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    ...
    "numberOfInstances": {
      "type": "int",
      "defaultValue": 2,
      "metadata": {
        "description": "Number of VMs to deploy"
      }
    },
    "numberOfDataDisksPerVM": {
      "type": "array",
      "defaultValue": [1,2]
    }
  },
```

Stellen Sie im Abschnitt „resources“ mehrere Instanzen der Vorlage bereit, die die Datenträger definiert. 

```json
{
  "apiVersion": "2016-09-01",
  "name": "[concat('nested-', copyIndex())]",
  "type": "Microsoft.Resources/deployments",
  "copy": {
    "name": "deploycopy",
    "count": "[parameters('numberOfInstances')]"
  },
  "properties": {
    "mode": "incremental",
    "templateLink": {
      "uri": "{data-disk-template-uri}",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "vmName": { "value": "[concat('myvm', copyIndex())]" },
      "storageAccountName": { "value": "[variables('storageAccountName')]" },
      "numDataDisks": { "value": "[parameters('numberOfDataDisksPerVM')[copyIndex()]]" }
    }
  }
},
```

Stellen Sie im Abschnitt „resources“ mehrere Instanzen des virtuellen Computers bereit. Verweisen Sie für die Datenträger auf die geschachtelte Bereitstellung, die die richtige Anzahl von Datenträgern und richtigen Namen der Datenträger enthält.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat('myvm', copyIndex())]",
  "copy": {
    "name": "virtualMachineLoop",
      "count": "[parameters('numberOfInstances')]"
  },
  "properties": {
    "storageProfile": {
      ...
      "dataDisks": "[reference(concat('nested-', copyIndex())).outputs.result.value]"
    },
    ...
  },
  ...
}
```

## <a name="return-values-from-a-loop"></a>Zurückgeben von Werten aus einer Schleife
Während das Erstellen mehrerer Instanzen eines Ressourcentyps praktisch ist, kann die Rückgabe von Werten aus der Schleife schwierig sein. Eine Möglichkeit zum Beibehalten und Zurückgeben von Werten ist die Verwendung von **copy** mit einer geschachtelten Vorlage und ein Roundtrip eines Arrays, das alle die zurückzugebenden Werte enthält. Angenommen, Sie möchten mehrere Speicherkonten erstellen und den primären Endpunkt jedes Kontos zurückgeben. 

Erstellen Sie zunächst die geschachtelten Vorlage, die das Speicherkonto erstellt. Beachten Sie, dass sie einen „array“-Parameter für die Blob-URIs akzeptiert. Sie verwenden diesen Parameter für einen Roundtrip aller Werte aus vorherigen Bereitstellungen. Die Ausgabe der Vorlage ist ein Array, das den neuen Blob-URI mit den vorherigen URIs verkettet.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "indexValue": {
      "type":"int"
    },
    "blobURIs": {
        "type": "array",
      "defaultValue": []
    }
  },
    "variables": {
    "storageName": "[concat('storage', uniqueString(resourceGroup().id), parameters('indexValue'))]"
  },
    "resources": [
    {
        "apiVersion": "2016-01-01",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageName')]",
      "location": "[resourceGroup().location]",
      "sku": {
          "name": "Standard_LRS"
      },
      "kind": "Storage",
      "properties": {  
      }
    }
    ],
    "outputs": {
      "result": {
        "type": "array",
      "value": "[concat(parameters('blobURIs'),split(reference(variables('storageName')).primaryEndpoints.blob, ','))]"
    }
  }
}
```

Erstellen Sie nun die übergeordneten Vorlage, die eine statische Instanz der geschachtelten Vorlage aufweist und in einer Schleife die verbleibenden Instanzen der geschachtelten Vorlage durchläuft. Übergeben Sie für jede Instanz der schleifengestützten Bereitstellung ein Array, das die Ausgabe der vorherigen Bereitstellung darstellt.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "numberofStorage": { "type": "int", "minValue": 2 }
  },
  "resources": [
    {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate0",
      "type": "Microsoft.Resources/deployments",
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "{storage-template-uri}",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "indexValue": {"value": 0}
        }
      }
    },
    {
      "apiVersion": "2016-09-01",
      "name": "[concat('nestedTemplate', copyIndex(1))]",
      "type": "Microsoft.Resources/deployments",
      "copy": {
        "name": "storagecopy",
        "count": "[sub(parameters('numberofStorage'), 1)]"
      },
      "properties": {
        "mode": "incremental",
        "templateLink": {
          "uri": "{storage-template-uri}",
          "contentVersion": "1.0.0.0"
        },
        "parameters": {
          "indexValue": {"value": "[copyIndex(1)]"},
          "blobURIs": {"value": "[reference(concat('nestedTemplate', copyIndex())).outputs.result.value]"}
        }
      }
    }
  ],
  "outputs": {
    "result": {
      "type": "object",
      "value": "[reference(concat('nestedTemplate', sub(parameters('numberofStorage'), 1))).outputs.result]"
    }
  }
}
```

## <a name="next-steps"></a>Nächste Schritte
* Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
* Unter [Funktionen von Azure Resource Manager-Vorlagen](resource-group-template-functions.md) finden Sie alle Funktionen, die Sie in einer Vorlage verwenden können.
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).


