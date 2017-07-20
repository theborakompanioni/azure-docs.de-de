---
title: "Verwendung verwalteter Datenträger mit Azure Resource Manager-Vorlagen | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zur Verwendung verwalteter Datenträger mit Azure Resource Manager-Vorlagen"
services: storage
documentationcenter: 
author: jboeshart
manager: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/01/2017
ms.author: jaboes
ms.translationtype: Human Translation
ms.sourcegitcommit: 74f34bdbf5707510c682814716aa0b95c19a5503
ms.openlocfilehash: f2c0355068bc6dfd9a4e1aab52e4f4f9f23a9512
ms.contentlocale: de-de
ms.lasthandoff: 06/09/2017

---

# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Verwenden verwalteter Datenträger mit Resource Manager-Vorlagen

In diesem Dokument erhalten Sie Informationen über die Unterschiede zwischen verwalteten und nicht verwalteten Datenträgern bei der Verwendung von Azure Resource Manager-Vorlagen zur Bereitstellung virtueller Computer. Hierdurch werden Sie bei der Aktualisierung vorhandener Vorlagen auf verwaltete Datenträger unterstützt, die vorher nicht verwaltete Datenträger verwendet haben. Zu Referenzzwecken verwenden wir die Vorlage [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) als Leitfaden. Sie können die Vorlage mit [verwalteten Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) und einer vorherigen Version mit [nicht verwalteten Datenträgern](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) anzeigen lassen, wenn Sie sie direkt miteinander vergleichen möchten.

## <a name="unmanaged-disks-template-formatting"></a>Formatieren von Vorlagen mit nicht verwalteten Datenträgern

Für den Anfang sehen wir uns die Bereitstellung nicht verwalteter Datenträger an. Für das Erstellen nicht verwalteter Datenträger benötigen Sie ein Speicherkonto, um die VHD-Dateien zu speichern. Sie können ein neues Speicherkonto erstellen oder ein bereits vorhandenes verwenden. In diesem Artikel erfahren Sie, wie Sie ein neues Speicherkonto erstellen. Hierfür benötigen Sie wie unten dargestellt eine Speicherkontoressource im Ressourcenblock.

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

Innerhalb eines Objekts für einen virtuellen Computer benötigen Sie eine Abhängigkeit vom Speicherkonto, um sicherzustellen, dass es vor dem virtuellen Computer erstellt wird. Geben Sie anschließend im Bereich `storageProfile` den vollständigen URI des VHD-Speicherorts an, der auf das Speicherkonto verweist und für den Betriebssystem- und andere Datenträger benötigt wird. 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": "1023",
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Formatieren von Vorlagen mit verwalteten Datenträgern

In Azure Managed Disks wird der Datenträger zu einer Ressource der obersten Ebene und benötigt kein Speicherkonto mehr, um vom Benutzer erstellt zu werden. Verwaltete Datenträger werden mit der API-Version `2016-04-30-preview` zur Verfügung gestellt und sind jetzt der Standard-Datenträgertyp. Die folgenden Abschnitte bieten eine Einführung in die standardmäßigen Einstellungen und erklären, wie Sie Ihre Datenträger weiter anpassen können.

### <a name="default-managed-disk-settings"></a>Standardeinstellungen für verwaltete Datenträger

Sie brauchen zum Erstellen eines virtuellen Computers mit verwalteten Datenträgern keine Speicherkontoressource mehr. Außerdem können Sie die Ressource Ihres virtuellen Computers wie folgt aktualisieren. Beachten Sie insbesondere, dass `apiVersion` `2016-04-30-preview` angibt und `osDisk` und `dataDisks` nicht mehr auf einen spezifischen URI für die VHD-Datei verweisen. Bei der Bereitstellung ohne Angabe von zusätzlichen Eigenschaften verwendet der Datenträger [Standard-LRS-Speicher](storage-redundancy.md). Wenn kein Name angegeben wird, wird das Format `<VMName>_OsDisk_1_<randomstring>` für den Betriebssystemdatenträger und `<VMName>_disk<#>_<randomstring>` für jeden anderen Datenträger übernommen. Standardmäßig ist Azure Disk Encryption deaktiviert; Zwischenspeichern für den Betriebssystemdatenträger erfolgt mit Lese-/Schreibzugriff und für Datenträger ohne Zugriff. Wie Sie im unteren Beispiel sehen können, besteht immer noch eine Speicherkontoabhängigkeit. Diese wird allerdings nur für Diagnosespeicher und nicht für Datenträgerspeicher benötigt.

```
{
    "apiVersion": "2016-04-30-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": "1023",
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Verwenden einer verwalteten Datenträgerressource auf oberster Ebene

Anstatt die Datenträgerkonfiguration im Objekt des virtuellen Computers anzugeben können Sie eine Datenträgerressource auf oberster Ebene erstellen und sie als Teil des Erstellungsprozesses des virtuellen Computers anfügen. Wir können beispielsweise eine Datenträgerressource zur Benutzung als Datenträger wie folgt erstellen.

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2016-04-30-preview",
    "location": "[resourceGroup().location]",
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "accountType": "Standard_LRS",
        "diskSizeGB": 1023
    }
}
```

Innerhalb des Objekt des virtuellen Computers können wir dann auf dieses Datenträgerobjekt verweisen, um es anzufügen. Durch Angabe der Ressourcen-ID des verwalteten Datenträgers, den wir in der Eigenschaft `managedDisk` erstellt haben, kann der Datenträger hinzugefügt werden, während der virtuelle Computer erstellt wird. Damit diese Funktion verwendet werden kann, muss die `apiVersion` für die VM-Ressource `2016-04-30-preview` sein. Beachten Sie außerdem, dass wir eine Abhängigkeit von der Datenträgerressource erstellt haben, um sicherzustellen, dass sie erfolgreich vor der Erstellung des virtuellen Computers erstellt wird. 

```
{
    "apiVersion": "2016-04-30-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Erstellen verwalteter Verfügbarkeitsgruppen mit virtuellen Computern mit verwalteten Datenträgern

Fügen Sie zum Erstellen verwalteter Verfügbarkeitsgruppen mit virtuellen Computern, die verwaltete Datenträger verwenden, das Objekt `sku` zur Ressource der Verfügbarkeitsgruppe hinzu, und legen Sie die Eigenschaft `name` auf `Aligned` fest. Dadurch wird sichergestellt, dass die Datenträger für jeden virtuellen Computer ausreichend voneinander isoliert sind, um einzelne Fehlerquellen zu vermeiden. Die `apiVersion` für die Ressource der Verfügbarkeitsgruppe muss ebenfalls `2016-04-30-preview` sein, um diese Funktion verwenden zu können.

```
{
    "apiVersion": "2016-04-30-preview",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": "3",
        "PlatformFaultDomainCount": "2"
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a>Zusätzliche Szenarios und Anpassungen

Lesen Sie bitte die REST-API-Dokumentation zum Erstellen eines verwalteten Datenträgers [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update) für vollständige Informationen zu REST-API-Spezifikationen. Hier finden Sie zusätzliche Szenarios sowie standardmäßige und gültige Werte, die durch Bereitstellung von Vorlagen an die API übermittelt werden können. 

## <a name="next-steps"></a>Nächste Schritte

* Vollständige Vorlagen, die verwaltete Datenträger verwenden, finden Sie unter den folgenden Links des Repositorys für Azure-Schnellstartvorlagen.
    * [Windows VM with managed disk (Virtueller Windows-Computer mit verwalteten Datenträgern)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Linux VM with managed disk (Virtueller Linux-Computer mit verwalteten Datenträgern)](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Full list of managed disk templates (Vollständige Liste der Vorlagen mit verwalteten Datenträgern)](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Im Dokument [Azure Managed Disks – Übersicht](storage-managed-disks-overview.md) finden Sie weitere Informationen zu verwalteten Datenträgern.
* Lesen Sie die Referenzdokumentation zu den Vorlagen für VM-Ressourcen. Diese finden Sie im Dokument [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines).
* Lesen Sie die Referenzdokumentation zu den Vorlagen für Datenträgerressourcen. Diese finden Sie im Dokument [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks).
 

