---
title: Mehrere Kopien der Azure-Ressourceneigenschaft | Microsoft-Dokumentation
description: "Erstellen Sie in der Azure Resource Manager-Vorlage mehrere Instanzen einer Eigenschaft für einen Ressourcentyp. Sie müssen anstelle der copy-Schleife für dieses Szenario die take-Funktion verwenden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: 
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/17/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: da79ff58e892bde5a35456992105de9bdbe36364
ms.lasthandoff: 04/18/2017


---
# <a name="create-multiple-instances-of-property-on-resource-type"></a>Erstellen mehrerer Instanzen einer Eigenschaft für einen Ressourcentyp
Sie können [copy](resource-group-create-multiple.md) nur für Ressourcentypen verwenden, nicht für Eigenschaften innerhalb eines Ressourcentyps. Diese Anforderung kann problematisch werden, wenn Sie mehrere Instanzen von etwas erstellen möchten, das Teil einer Ressource ist. Ein häufiges Szenario ist die Erstellung mehrerer Datenträger für einen virtuellen Computer. Sie können „copy“ nicht für die Datenträger verwenden, da „dataDisks“ eine Eigenschaft des virtuellen Computers und nicht sein eigener Ressourcentyp ist. Stattdessen erstellen Sie ein Array mit so vielen Datenträgern wie benötigt, und übergeben die tatsächliche Anzahl zu erstellender Datenträger. In der Definition des virtuellen Computers verwenden Sie die `take`-Funktion, um nur die Anzahl von Elementen aus dem Array abzurufen, die Sie tatsächlich wünschen.

Ein vollständiges Beispiel für dieses Muster wird finden Sie in der Vorlage [Create a VM with a dynamic selection of data disks](https://azure.microsoft.com/documentation/templates/201-vm-dynamic-data-disks-selection/) (Erstellen eines virtuellen Computers mit der dynamischen Auswahl von Datenträgern).

Die relevanten Abschnitte der Bereitstellungsvorlage sind in diesem Artikel aufgeführt. Teile der Vorlage wurden entfernt, um die Abschnitte zum dynamischen Erstellen von Datenträgern hervorzuheben. 

## <a name="define-template-with-variable-for-the-property"></a>Definieren einer Vorlage mit einer Variablen für die Eigenschaft

Fügen Sie eine Arrayvariable hinzu, die mehr Datenträger definiert als Sie benötigen, um einen virtuellen Computer mit mehreren Datenträgern zu erstellen. Übergeben Sie im Parameter `numDataDisks` die tatsächliche Anzahl der zu erstellenden Datenträger. Verwenden Sie `take`, um die genaue Anzahl der Elemente im Array anzugeben, die dem virtuellen Computer zugewiesen werden.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
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
      ... /* not all elements shown */
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
    {
      "type": "Microsoft.Compute/virtualMachines",
      "properties": {
        "storageProfile": {
          "dataDisks": "[take(variables('diskArray'),parameters('numDataDisks'))]"
        },
        ...
      }
      ...
    }
  ]
}
```

## <a name="create-multiple-virtual-machines-with-multiple-data-disks"></a>Erstellen mehrerer virtueller Computer mit mehreren Datenträgern

Sie können die **take**-Funktion und das **copy**-Element zusammen verwenden, wenn Sie mehrere Instanzen einer Ressource mit einer variablen Anzahl von Elementen für eine Eigenschaft benötigen. Angenommen, Sie müssen mehrere virtuelle Computer erstellen, doch jeder davon hat eine andere Anzahl von Datenträgern. 

Um jeden Datenträger mit einem Namen zu versehen, der den zugehörigen virtuellen Computer bestimmt, legen Sie Ihr Array mit Datenträgern in einer getrennten Vorlage ab. Fügen Sie Parameter für den Namen des virtuellen Computers und die zurückzugebende Anzahl von Datenträgern hinzu. Geben Sie im Abschnitt „outputs“ die Anzahl angegebener Elemente zurück.

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

## <a name="next-steps"></a>Nächste Schritte
* Eine Einführung zum Erstellen mehrerer Instanzen einer Ressource finden Sie unter [Bereitstellen mehrerer Instanzen von Ressourcen in Azure Resource Manager-Vorlagen](resource-group-create-multiple.md).
* Informationen zum Erstellen iterierter Ressourcen in Folge finden Sie unter [Sequenzielle Schleifen für Azure Resource Manager-Vorlagen](resource-manager-sequential-loop.md).
* Informationen zum Bereitstellen Ihrer Vorlage finden Sie unter [Bereitstellen einer Anwendung mit einer Azure-Ressourcen-Manager-Vorlage](resource-group-template-deploy.md).


