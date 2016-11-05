---
title: Benutzerdefinierte Skripts auf virtuellen Windows-Computern unter Verwendung von Vorlagen | Microsoft Docs
description: Automatisieren Sie Konfigurationsaufgaben für virtuelle Windows-Computer mithilfe der benutzerdefinierten Skripterweiterung unter Verwendung von Ressourcen-Manager-Vorlagen.
services: virtual-machines-windows
documentationcenter: ''
author: kundanap
manager: timlt
editor: ''
tags: azure-resource-manager

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap

---
# Windows-VM – benutzerdefinierte Skripterweiterungen mit Azure Resource Manager-Vorlagen
[!INCLUDE [virtual-machines-common-extensions-customscript](../../includes/virtual-machines-common-extensions-customscript.md)]

## Vorlagenbeispiel für einen virtuellen Windows-Computer
Definieren Sie die folgende Ressource im Resource-Abschnitt der Vorlage.

       {
       "type": "Microsoft.Compute/virtualMachines/extensions",
       "name": "MyCustomScriptExtension",
       "apiVersion": "2015-05-01-preview",
       "location": "[parameters('location')]",
       "dependsOn": [
           "[concat('Microsoft.Compute/virtualMachines/',parameters('vmName'))]"
       ],
       "properties": {
           "publisher": "Microsoft.Compute",
           "type": "CustomScriptExtension",
           "typeHandlerVersion": "1.7",
           "autoUpgradeMinorVersion":true,
           "settings": {
               "fileUris": [
               "http://Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
           ],
           "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
         }
       }
     }

Ersetzen Sie im obigen Beispiel die Datei-URL und den Dateinamen durch Ihre eigenen Einstellungen. Die erstellte Vorlage können Sie anschließend über Azure PowerShell bereitstellen.

Wenn Skript-URLs und Parameter privat bleiben sollen, können Sie die Skript-URL als **private** festlegen. Wenn die Skript-URL als **private** festgelegt wird, ist der Zugriff darauf nur mit Speicherkontoname und -schlüssel möglich, die als geschützte Einstellungen gesendet werden. Die Skriptparameter können auch mit Version 1.7 oder höher der benutzerdefinierten Skripterweiterung als geschützte Einstellungen bereitgestellt werden.

## Vorlagenbeispiel für einen virtuellen Windows-Computer mit geschützten Einstellungen
        {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.7",
        "settings": {
        "fileUris": [
        "http: //Yourstorageaccount.blob.core.windows.net/customscriptfiles/start.ps1"
        ]
        },
        "protectedSettings": {
        "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -start.ps1",
        "storageAccountName": "yourStorageAccountName",
        "storageAccountKey": "yourStorageAccountKey"
        }
        }
Informationen über das Schema der neuesten Versionen der benutzerdefinierten Skripterweiterung finden Sie unter [Konfigurationsbeispiele für Windows-VM-Erweiterungen in Azure](virtual-machines-windows-extensions-configuration-samples.md).

Beispiele zum Konfigurieren einer Anwendung auf einem virtuellen Computer mithilfe der benutzerdefinierten Skripterweiterung finden Sie unter [Custom Script extension on a Windows VM](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/) (Benutzerdefinierte Skripterweiterung auf einer Windows-VM).

<!---HONumber=AcomDC_0824_2016-->