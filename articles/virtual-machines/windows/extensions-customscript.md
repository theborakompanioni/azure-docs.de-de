---
title: "Azure – benutzerdefinierte Skripterweiterung für Windows | Microsoft-Dokumentation"
description: "Automatisieren von Konfigurationsaufgaben für virtuelle Windows-Computer mithilfe der benutzerdefinierten Skripterweiterung"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: a6f417ea6575b81258998ae3b31c10e9df59b603
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="custom-script-extension-for-windows"></a>CustomScript-Erweiterung für Windows

Die benutzerdefinierte Skripterweiterung lädt Skripts auf virtuelle Azure-Computer herunter und führt sie aus. Diese Erweiterung ist hilfreich bei der Konfiguration nach der Bereitstellung, bei der Softwareinstallation oder bei anderen Konfigurations-/Verwaltungsaufgaben. Skripts können aus Azure Storage oder GitHub heruntergeladen oder dem Azure-Portal zur Laufzeit für die Erweiterung bereitgestellt werden. Die benutzerdefinierte Skripterweiterung kann in Azure Resource Manager-Vorlagen integriert und auch mithilfe der Azure-Befehlszeilenschnittstelle, mithilfe von PowerShell, über das Azure-Portal oder unter Verwendung der REST-API für virtuelle Azure-Computer ausgeführt werden.

In diesem Dokument erfahren Sie, wie Sie die benutzerdefinierte Skripterweiterung über das Azure PowerShell-Modul ausführen. Sie erfahren, wie Sie eine Azure Resource Manager-Vorlage verwenden, und erhalten Informationen zu Problembehandlungsschritten für Windows-Systeme.

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die benutzerdefinierte Skripterweiterung für Windows ist für Windows Server 2008 R2, 2012, 2012 R2 und 2016 geeignet.

### <a name="script-location"></a>Speicherort des Skripts

Das Skript muss im Azure Blob Storage oder an einem anderen Ort gespeichert werden, auf den über eine gültige URL zugegriffen werden kann.

### <a name="internet-connectivity"></a>Internetverbindung

Um die benutzerdefinierte Skripterweiterung für Windows verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-schema"></a>Erweiterungsschema

Der folgende JSON-Code zeigt das Schema für die benutzerdefinierte Skripterweiterung. Die Erweiterung erfordert einen Skriptspeicherort (Azure Storage oder anderen Ort mit gültiger URL) und einen auszuführenden Befehl. Wenn Sie Azure Storage als Skriptquelle verwenden, sind ein Azure-Speicherkontoname und Kontoschlüssel erforderlich. Diese Elemente müssen als vertrauliche Daten behandelt und in der Konfiguration mit den geschützten Einstellungen der Erweiterung angegeben werden. Die geschützten Einstellungsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt.

```json
{
    "apiVersion": "2015-06-15",
    "type": "extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Eigenschaftswerte

| Name | Wert/Beispiel |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Herausgeber | Microsoft.Compute |
| type | Erweiterungen |
| typeHandlerVersion | 1.9 |
| fileUris (Beispiel) | https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1 |
| commandToExecute (Beispiel) | powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 |
| storageAccountName (Beispiel) | examplestorageacct |
| storageAccountKey (Beispiel) | TmJK/1N3AbAZ3q/+hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg== |

**Hinweis**: Bei Eigenschaftennamen wird zwischen Groß- und Kleinschreibung unterschieden. Verwenden Sie die Namen wie oben angegeben, um Probleme bei der Bereitstellung zu vermeiden.

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Das im vorherigen Abschnitt erläuterte JSON-Schema kann in einer Azure Resource Manager-Vorlage zum Ausführen der benutzerdefinierten Skripterweiterung im Rahmen einer Azure Resource Manager-Bereitstellung verwendet werden. Eine Beispielvorlage, die die benutzerdefinierte Skripterweiterung enthält, finden Sie hier auf [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Mit dem `Set-AzureRmVMCustomScriptExtension`-Befehl können Sie die benutzerdefinierte Skripterweiterung einem vorhandenen virtuellen Computer bereitstellen. Weitere Informationen finden Sie unter [Set-AzureRmVMCustomScriptExtension](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe des Azure PowerShell-Moduls abgerufen werden. Führen Sie den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis auf dem virtuellen Zielcomputer protokolliert.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Die angegebenen Dateien werden in das folgende Verzeichnis auf dem virtuellen Zielcomputer heruntergeladen.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
Hierbei ist `<n>` eine dezimale Ganzzahl, die sich zwischen verschiedenen Ausführungen der Erweiterung ändern kann.  Der Wert `1.*` entspricht dem tatsächlichen aktuellen `typeHandlerVersion`-Wert der Erweiterung.  Das tatsächliche Verzeichnis könnte z.B. folgendermaßen lauten: `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Beim Ausführen des Befehls `commandToExecute` legt die Erweiterung dieses Verzeichnis (z.B. `...\Downloads\2`) als aktuelles Arbeitsverzeichnis fest. Dies ermöglicht die Verwendung relativer Pfade, um die heruntergeladenen Dateien mithilfe der `fileURIs`-Eigenschaft aufzufinden. Beispiele hierfür finden Sie in der unten stehenden Tabelle.

Da sich der absolute Downloadpfad im Lauf der Zeit ändern kann, empfiehlt es sich, in der Zeichenfolge `commandToExecute` nach Möglichkeit relative Pfade zu Skripts und Dateien zu verwenden. Beispiel:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Pfadinformationen nach dem ersten URI-Segment werden für Dateien beibehalten, die über die `fileUris`-Eigenschaftenliste heruntergeladen werden.  Wie in der Tabelle unten gezeigt, werden heruntergeladene Dateien zu Unterverzeichnissen für den Download zugeordnet, um die Struktur der `fileUris`-Werte widerzuspiegeln.  

#### <a name="examples-of-downloaded-files"></a>Beispiele für heruntergeladene Dateien

| URI in „fileUris“ | Relativer Speicherort für Downloads | Absoluter Speicherort für Downloads* |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\* Wie oben bereits erwähnt, ändern sich absolute Verzeichnispfade während der Lebensdauer eines virtuellen Computers, nicht jedoch innerhalb einer einzelnen Ausführung der CustomScript-Erweiterung.

### <a name="support"></a>Support

Wenn Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie Azure-Experten im MSDN Azure-Forum oder im Stack Overflow-Forum (https://azure.microsoft.com/en-us/support/forums/) Fragen stellen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/en-us/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/en-us/support/faq/).

