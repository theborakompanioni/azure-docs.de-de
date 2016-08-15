<properties
   pageTitle="Benutzerdefinierte Skripts auf virtuellen Windows-Computern unter Verwendung von Vorlagen | Microsoft Azure"
   description="Automatisieren Sie Konfigurationsaufgaben für virtuelle Windows-Computer mithilfe der benutzerdefinierten Skripterweiterung unter Verwendung von Ressourcen-Manager-Vorlagen."
   services="virtual-machines-windows"
   documentationCenter=""
   authors="kundanap"
   manager="timlt"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="03/29/2016"
   ms.author="kundanap"/>

# Verwenden der benutzerdefinierten Skripterweiterung für virtuelle Windows-Computer mit Azure Resource Manager-Vorlagen

[AZURE.INCLUDE [virtual-machines-common-extensions-customscript](../../includes/virtual-machines-common-extensions-customscript.md)]

## Vorlagenbeispiel für einen virtuellen Windows-Computer

Definieren Sie die folgende Ressource im "Resource"-Abschnitt der Vorlage.

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

In vielen Szenarien möchten Kunden die Skript-URLs und Parameter privat halten. Dies kann erreicht werden, indem die Skript-URL privat gehalten wird, sodass nur mit Speicherkontonamen und -schlüsseln darauf zugegriffen werden kann, die als geschützte Einstellungen gesendet werden. Darüber hinaus können auch die Skriptparameter mit Version 1.7 oder höher für die benutzerdefinierte Windows-Skripterweiterung als geschützte Einstellungen bereitgestellt werden.

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
Informationen über das Schema der neuesten Versionen der benutzerdefinierten Skripterweiterung finden Sie in der Dokumentation [hier](virtual-machines-windows-extensions-configuration-samples.md)

Im nachstehenden Beispiel finden Sie ein vollständiges Beispiel zum Konfigurieren von Anwendungen auf einem virtuellen Computer mithilfe der benutzerdefinierten Skripterweiterung.

* [Benutzerdefinierte Skripterweiterung auf einem virtuellen Windows-Computer](https://github.com/Azure/azure-quickstart-templates/blob/b1908e74259da56a92800cace97350af1f1fc32b/201-list-storage-keys-windows-vm/azuredeploy.json/)

<!---HONumber=AcomDC_0803_2016-->