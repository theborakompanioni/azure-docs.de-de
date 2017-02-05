---
title: "OMS-Azure-VM-Erweiterung für Windows | Microsoft-Dokumentation"
description: Stellen Sie den OMS-Agent mithilfe einer VM-Erweiterung auf einem virtuellen Windows-Computer bereit.
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: feae6176-2373-4034-b5d9-a32c6b4e1f10
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: d65505182267bec053fca0ab5045cbcae9381ac7
ms.openlocfilehash: 245e49d36142ef8e927a5e494113e4dd7809fc16


---
# <a name="oms-virtual-machine-extension-for-windows"></a>OMS-Azure-VM-Erweiterung für Windows

## <a name="overview"></a>Übersicht

Die Operations Management Suite (OMS) bietet Überwachungs- und Warnungsfunktionen sowie Funktionen zum Beheben von Warnungen für cloudbasierte und lokale Ressourcen. Die OMS-Agent-VM-Erweiterung für Windows wird von Microsoft veröffentlicht und unterstützt. Die Erweiterung installiert den OMS-Agent auf virtuellen Azure-Computern und registriert virtuelle Computer in einem vorhandenen OMS-Arbeitsbereich. Dieses Dokument enthält ausführliche Informationen zu den unterstützten Plattformen, Konfigurationen und Bereitstellungsoptionen für die OMS-VM-Erweiterung für Windows.

Allgemeine Informationen zu Azure-VM-Erweiterungen finden Sie in der [Übersicht über VM-Erweiterungen](./virtual-machines-windows-extensions-features.md).

Weitere Informationen zur Operations Management Suite finden Sie in der [Übersicht über die Operations Management Suite](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite).

## <a name="prerequisites"></a>Voraussetzungen

### <a name="operating-system"></a>Betriebssystem

Die OMS-Agent-Erweiterung für Windows ist für Windows Server 2012, 2012 R2 und 2016 geeignet.

### <a name="connectivity"></a>Konnektivität

Um die OMS-Agent-Erweiterung für Windows verwenden zu können, muss der virtuelle Zielcomputer mit dem Internet verbunden sein. 

## <a name="extension-configuration"></a>Konfiguration der Erweiterung

Für die OMS-Agent-VM-Erweiterung für Windows werden die Arbeitsbereichs-ID und der Arbeitsbereichsschlüssel aus dem OMS-Zielarbeitsbereich benötigt. Da der Arbeitsbereichsschlüssel als vertrauliche Information behandelt werden muss, wird er in einer geschützten Konfiguration gespeichert. Die geschützten Konfigurationsdaten der Azure-VM-Erweiterung werden verschlüsselt und nur auf dem virtuellen Zielcomputer entschlüsselt. Die öffentliche und die private Konfiguration werden zum Zeitpunkt der Bereitstellung angegeben, wie in den folgenden Abschnitten dieses Dokuments erläutert.

### <a name="public-configuration"></a>Öffentliche Konfiguration

Schema für die öffentliche Konfiguration:

- workspaceId – erforderliche Zeichenfolge: Die ID des OMS-Arbeitsbereichs, in den der virtuelle Computer integriert werden soll.

```json
{
  "workspaceId": "myWorkspaceId"
}
```

### <a name="private-configuration"></a>Private Konfiguration

Schema für die öffentliche Konfiguration:

- workspaceKey – erforderliche Zeichenfolge: Der primäre/sekundäre gemeinsam verwendete Schlüssel des Arbeitsbereichs.

```json
{
  "workspaceKey": "myWorkSpaceKey"
}
```

## <a name="template-deployment"></a>Bereitstellung von Vorlagen

Azure-VM-Erweiterungen können mithilfe von Azure Resource Manager-Vorlagen bereitgestellt werden. Vorlagen sind ideal, wenn Sie virtuelle Computer bereitstellen, die nach der Bereitstellung konfiguriert werden müssen (beispielsweise, um sie in OMS zu integrieren). Eine Resource Manager-Beispielvorlage mit der OMS-Agent-VM-Erweiterung finden Sie im [Azure-Schnellstartkatalog](https://github.com/Azure/azure-quickstart-templates/tree/master/201-oms-extension-windows-vm). 

Das Beispiel kann über dieses Dokument mithilfe der folgenden Schaltfläche bereitgestellt werden:

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-oms-extension-windows-vm%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Der JSON-Code zum Bereitstellen der OMS-Agent-VM-Erweiterung sieht wie im folgenden JSON-Beispiel aus:

```json
{
    "type": "extensions",
    "name": "Microsoft.EnterpriseCloud.Monitoring",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.EnterpriseCloud.Monitoring",
        "type": "MicrosoftMonitoringAgent",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "workspaceId": "myWorkSpaceKey"
        },
        "protectedSettings": {
            "workspaceKey": "myWorkspaceId"
        }
    }
}
```

## <a name="powershell-deployment"></a>PowerShell-Bereitstellung

Mit dem Befehl `Set-AzureRmVMExtension` können Sie die OMS-Agent-VM-Erweiterung auf einem vorhandenen virtuellen Computer bereitstellen. Vor dem Ausführen des Befehls müssen die öffentliche und die private Konfiguration in einer PowerShell-Hashtabelle gespeichert werden. 

```powershell
$PublicSettings = @{"workspaceId" = "myWorkspaceId"}
$ProtectedSettings = @{"workspaceKey" = "myWorkspaceKey"}

Set-AzureRmVMExtension -ExtensionName "Microsoft.EnterpriseCloud.Monitoring" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.EnterpriseCloud.Monitoring" `
    -ExtensionType "MicrosoftMonitoringAgent" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS ` 
```

## <a name="troubleshoot-and-support"></a>Problembehandlung und Support

### <a name="troubleshoot"></a>Problembehandlung

Daten zum Status von Erweiterungsbereitstellungen können über das Azure-Portal und mithilfe der Azure-Befehlszeilenschnittstelle abgerufen werden. Führen Sie über die Azure-Befehlszeilenschnittstelle den folgenden Befehl aus, um den Bereitstellungsstatus von Erweiterungen für einen bestimmten virtuellen Computer anzuzeigen.

```azurecli
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Die Ausgabe der Erweiterungsausführung wird in Dateien im folgenden Verzeichnis protokolliert:

`
C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\
`

### <a name="support"></a>Support

Sollten Sie beim Lesen dieses Artikels feststellen, dass Sie weitere Hilfe benötigen, können Sie sich über das [MSDN Azure-Forum oder über das Stack Overflow-Forum](https://azure.microsoft.com/en-us/support/forums/) mit Azure-Experten in Verbindung setzen. Alternativ dazu haben Sie die Möglichkeit, einen Azure-Supportfall zu erstellen. Rufen Sie die [Azure-Support-Website](https://azure.microsoft.com/en-us/support/options/) auf, und wählen Sie „Support erhalten“ aus. Informationen zur Nutzung von Azure-Support finden Sie unter [Microsoft Azure-Support-FAQ](https://azure.microsoft.com/en-us/support/faq/).



<!--HONumber=Dec16_HO1-->


