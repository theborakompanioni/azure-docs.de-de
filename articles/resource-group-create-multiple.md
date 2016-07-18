<properties
   pageTitle="Bereitstellen mehrerer Instanzen von Ressourcen | Microsoft Azure"
   description="Verwenden Sie den copy-Vorgang und Arrays in einer Azure-Ressourcen-Manager-Vorlage, um sie beim Bereitstellen von Ressourcen mehrere Male zu durchlaufen."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="wpickett"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/30/2016"
   ms.author="tomfitz"/>

# Erstellen mehrerer Instanzen von Ressourcen im Azure-Ressourcen-Manager

In diesem Thema erfahren Sie, wie Sie die Azure-Ressourcen-Manager-Vorlage durchlaufen können, um mehrere Instanzen einer Ressource zu erstellen.

## "copy", "copyIndex" und "length"

Um innerhalb der Ressource mehrere Instanzen zu erstellen, können Sie ein **copy**-Objekt definieren, das die Anzahl der Iterationen angibt. Die Kopie hat das folgende Format:

    "copy": { 
        "name": "websitescopy", 
        "count": "[parameters('count')]" 
    } 

Mit der **copyIndex()**-Funktion können Sie den aktuellen Iterationswert abrufen, wie nachfolgend in der Verkettungsfunktion gezeigt.

    [concat('examplecopy-', copyIndex())]

Wenn Sie mehrere Ressourcen aus einem Array von Werten erstellen, können Sie die Anzahl über die Funktion **length** angeben. Das Array wird als Parameter für die length-Funktion bereitgestellt.

    "copy": {
        "name": "websitescopy",
        "count": "[length(parameters('siteNames'))]"
    }

## Verwenden des Indexwerts in Namen

Sie können den copy-Vorgang verwenden, um mehrere Instanzen einer Ressource mit einem eindeutigen Namen basierend auf dem inkrementellen Index zu erstellen. Es kann beispielsweise sein, dass Sie am Ende des Namens jeder bereitgestellten Ressource eine eindeutige Zahl hinzufügen möchten. So stellen Sie drei Websites mit den folgenden Namen bereit:

- examplecopy-0
- examplecopy-1
- examplecopy-2

Verwenden Sie die folgende Vorlage:

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

## Versetzen des Indexwerts

Im obigen Beispiel wird Ihnen auffallen, dass der Indexwert von 0 bis 2 reicht. Zum Versetzen des Indexwerts können Sie einen Wert in der **copyIndex()**-Funktion übergeben, z. B. **copyIndex(1)**. Die Anzahl von durchzuführenden Durchläufen wird weiterhin im copy-Element angegeben, aber der Wert von copyIndex wird um den angegebenen Wert versetzt. Wenn Sie dieselbe Vorlage wie im vorherigen Beispiel verwenden, dieses Mal aber **copyIndex(1)** angeben, werden also drei Websites mit den folgenden Namen bereitgestellt:

- examplecopy-1
- examplecopy-2
- examplecopy-3

## Verwenden von „copy“ mit Arrays
   
Der copy-Vorgang ist besonders bei Verwendung von Arrays hilfreich, weil Sie jedes Element im Array durchlaufen können. So stellen Sie drei Websites mit den folgenden Namen bereit:

- examplecopy-Contoso
- examplecopy-Fabrikam
- examplecopy-Coho

Verwenden Sie die folgende Vorlage:

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

Natürlich legen Sie die copy-Anzahl auf einen anderen Wert fest als die Länge des Arrays. Sie können beispielsweise ein Array mit vielen Werten erstellen und dann einen Parameterwert übergeben, der festlegt, wie viele der Arrayelemente bereitgestellt werden sollen. In diesem Fall legen Sie die copy-Anzahl wie im ersten Beispiel gezeigt fest.

## Abhängigkeit von Ressourcen in einer Schleife

Sie können angeben, dass eine Ressource nach einer anderen Ressource bereitgestellt wird, indem Sie das **dependsOn**-Element verwenden. Wenn Sie eine Ressource bereitstellen möchten, die von der Sammlung von Ressourcen in einer Schleife abhängt, können Sie den Namen der Kopierschleife im **dependsOn**-Element angeben. Das folgende Beispiel zeigt, wie 3 Speicherkonten vor dem Bereitstellen des virtuellen Computers bereitgestellt werden. Die vollständige Definition des virtuellen Computers ist dabei nicht angegeben. Beachten Sie, dass **name** für das copy-Element auf **storagecopy** und auch das **dependsOn**-Element für die virtuellen Computer auf **storagecopy** gesetzt ist.

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

## Schleifen an einer geschachtelten Ressource

Sie können keine Kopieren-Schleife für eine geschachtelte Ressource verwenden. Wenn Sie mehrere Instanzen einer Ressource erstellen müssen, die Sie in der Regel als in einer anderen Ressource geschachtelt definieren, müssen Sie die Ressource stattdessen als eine Ressource der obersten Ebene erstellen und die Beziehung mit der übergeordneten Ressource über die **type** -und **name**-Eigenschaften definieren.

Nehmen wir beispielsweise an, dass Sie ein DataSet in der Regel als geschachtelte Ressource innerhalb einer Data Factory definieren.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "string"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
        "resources": [
        {
            "type": "datasets",
            "name": "[parameters('dataSetName')]",
            "dependsOn": [
                "[parameters('dataFactoryName')]"
            ],
            ...
        }
    }]
    
Um mehrere DataSet-Instanzen zu erstellen, müssten Sie die Vorlage ändern, wie unten dargestellt. Beachten Sie den voll qualifizierten Typ und den Namen, der den Namen der Data Factory enthält.

    "parameters": {
        "dataFactoryName": {
            "type": "string"
         },
         "dataSetName": {
            "type": "array"
         }
    },
    "resources": [
    {
        "type": "Microsoft.DataFactory/datafactories",
        "name": "[parameters('dataFactoryName')]",
        ...
    },
    {
        "type": "Microsoft.DataFactory/datafactories/datasets",
        "name": "[concat(parameters('dataFactoryName'), '/', parameters('dataSetName')[copyIndex()])]",
        "dependsOn": [
            "[parameters('dataFactoryName')]"
        ],
        "copy": { 
            "name": "datasetcopy", 
            "count": "[length(parameters('dataSetName'))]" 
        } 
        ...
    }]

## Erstellen mehrerer Instanzen, wenn „copy“ nicht funktioniert

Sie können **copy** nur für Ressourcentypen verwenden, nicht für Eigenschaften innerhalb eines Ressourcentyps. Dies kann problematisch werden, wenn Sie mehrere Instanzen von etwas erstellen möchten, das Teil einer Ressource ist. Ein häufiges Szenario ist die Erstellung mehrerer Datenträger für einen virtuellen Computer. Sie können **copy** nicht für die Datenträger verwenden, da **dataDisks** eine Eigenschaft auf dem virtuellen Computer ist, nicht sein eigener Ressourcentyp. Stattdessen erstellen Sie ein Array mit so vielen Datenträgern, wie Sie benötigen, und übergeben die tatsächliche Anzahl zu erstellender Datenträger. In der Definition des virtuellen Computers verwenden Sie die **take**-Funktion, um nur die Anzahl von Elementen aus dem Array abzurufen, die Sie tatsächlich wünschen.

Ein vollständiges Beispiel für dieses Muster wird finden Sie in der Vorlage [Create a VM with a dynamic selection of data disks](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) (Erstellen eines virtuellen Computers mit der dynamischen Auswahl von Datenträgern).

Die relevanten Abschnitte der Bereitstellungsvorlage sind unten aufgeführt. Ein Großteil der Vorlage wurde entfernt, um die Abschnitte zum dynamischen Erstellen von Datenträgern hervorzuheben. Beachten Sie den Parameter **numDataDisks**, mit dem Sie die Anzahl der zu erstellenden Datenträger übergeben können.

```
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


## Nächste Schritte
- Informationen zu den Abschnitten einer Vorlage finden Sie unter [Erstellen von Azure-Ressourcen-Manager-Vorlagen](./resource-group-authoring-templates.md).
- Unter [Funktionen von Azure-Ressourcen-Manager-Vorlagen](./resource-group-template-functions.md) finden Sie alle Funktionen, die Sie in einer Vorlage verwenden können.
- Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).

<!---HONumber=AcomDC_0706_2016-->