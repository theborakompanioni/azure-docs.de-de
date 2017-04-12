---
title: "Erweiterungen und Features für virtuelle Computer für Windows in Azure | Microsoft Docs"
description: "Sie erhalten einen Überblick über die Erweiterungen für virtuelle Azure-Computer, gruppiert nach den bereitgestellten oder verbesserten Funktionen."
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 999d63ee-890e-432e-9391-25b3fc6cde28
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/06/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 09b4a34a45ad676a5279dad85fc84578f40a212a
ms.lasthandoff: 03/31/2017


---
# <a name="virtual-machine-extensions-and-features-for-windows"></a>Erweiterungen und Features für virtuelle Computer für Windows

Erweiterungen für virtuelle Azure-Computer sind kleine Anwendungen, die Konfigurations- und Automatisierungsaufgaben auf virtuellen Azure-Computern nach der Bereitstellung ermöglichen. Wenn z.B. Software auf einem virtuellen Computer installiert werden muss, Virenschutz oder eine Docker-Konfiguration erforderlich ist, kann eine VM-Erweiterung zum Ausführen dieser Aufgaben verwendet werden. Azure VM-Erweiterungen können mithilfe der Azure-CLI, PowerShell, Azure Resource Manager-Vorlagen und dem Azure-Portal ausgeführt werden. Erweiterungen können mit einer neuen Bereitstellung für virtuelle Computer gebündelt oder in Bezug auf ein bestehendes System ausgeführt werden.

Dieses Dokument enthält eine Übersicht der VM-Erweiterungen, erläutert Voraussetzungen für die Verwendung von VM-Erweiterungen und bietet Hilfestellung beim Erkennen, Verwalten und Entfernen von VM-Erweiterungen. Dieses Dokument enthält verallgemeinerte Informationen, da eine Vielzahl von VM-Erweiterungen verfügbar ist, die alle eine potenziell eigene Konfiguration aufweisen. Erweiterungsspezifische Details finden Sie in der für die jeweilige Erweiterung spezifischen Dokumentation.

## <a name="use-cases-and-samples"></a>Anwendungsfälle und Beispiele

Es sind viele verschiedene Azure VM-Erweiterungen für jeweils spezifische Anwendungsfälle verfügbar. Einige Beispiele für Anwendungsfälle:

- Anwenden von gewünschten Statuskonfigurationen mit PowerShell auf einen virtuellen Computer mithilfe der DSC-Erweiterung für Windows. Weitere Informationen finden Sie unter [Azure Desired State configuration extension](extensions-dsc-overview.md) (Azure-Erweiterung für die gewünschte Statuskonfiguration).
- Konfigurieren der Überwachung eines virtuellen Computers mit der Microsoft Monitoring Agent-VM-Erweiterung. Weitere Informationen finden Sie unter [Verbinden von virtuellen Azure-Computern mit Log Analytics](../../log-analytics/log-analytics-azure-vm-extension.md).
- Konfigurieren der Überwachung Ihrer Azure-Infrastruktur mit der Datadog-Erweiterung. Weitere Informationen finden Sie im [Datadog-Blog](https://www.datadoghq.com/blog/introducing-azure-monitoring-with-one-click-datadog-deployment/).
- Konfigurieren eines virtueller Azure-Computers mithilfe von Chef. Weitere Informationen finden Sie unter [Automatisieren der Bereitstellung virtueller Azure-Computer mit Chef](chef-automation.md).

Über prozessspezifische Erweiterungen hinaus ist sowohl für virtuelle Windows- als auch für virtuelle Linux-Computer eine benutzerdefinierte Skripterweiterung verfügbar. Die benutzerdefinierte Skripterweiterung für Windows ermöglicht die Ausführung beliebiger PowerShell-Skripts auf virtuellen Computern. Dies ist beim Entwerfen von Azure-Bereitstellungen nützlich, die Konfiguration über das Maß hinaus erfordern, das mithilfe von Azure-Tools erreicht werden kann. Weitere Informationen finden Sie unter [Benutzerdefinierte Skripterweiterung für Windows-VMs](extensions-customscript.md).

Wenn Sie ein Beispiel für den Einsatz einer VM-Erweiterung in einer End-to-End-Anwendungsbereitstellung durcharbeiten möchten, lesen Sie [Automatisieren von Anwendungsbereitstellungen auf virtuellen Azure-Computern](dotnet-core-1-landing.md).

## <a name="prerequisites"></a>Voraussetzungen

Für jede VM-Erweiterung gilt möglicherweise ein eigener Satz Voraussetzungen. Beispielsweise setzt die Docker-VM-Erweiterung eine unterstützte Linux-Distribution voraus. Die Anforderungen der einzelnen Erweiterungen werden ausführlich in der erweiterungsspezifischen Dokumentation behandelt.

### <a name="azure-vm-agent"></a>Azure-VM-Agent
Der Azure-VM-Agent verwaltet die Interaktion eines virtuellen Azure-Computers mit dem Azure Fabric Controller. Der VM-Agent ist für viele funktionale Aspekte in Bezug auf die Bereitstellung und Verwaltung virtueller Azure-Computer verantwortlich. Dies umfasst auch das Ausführen von VM-Erweiterungen. Der Azure-VM-Agent ist in Images aus dem Azure Marketplace vorinstalliert und kann auf unterstützten Betriebssystemen installiert werden.

Informationen zu unterstützten Betriebssystemen und Installationshinweise finden Sie unter [Informationen zum Agent und zu Erweiterungen für virtuelle Computer](agent-user-guide.md).

## <a name="discover-vm-extensions"></a>Ermitteln von VM-Erweiterungen
Für die Verwendung mit virtuellen Azure-Computern stehen viele verschiedene VM-Erweiterungen zur Verfügung. Um eine vollständige Liste anzuzeigen, führen Sie den folgenden Befehl mit dem Azure Resource Manager-PowerShell-Modul aus. Stellen Sie sicher, dass Sie die gewünschte Position angeben, wenn Sie diesen Befehl ausführen.

```powershell
Get-AzureRmVmImagePublisher -Location WestUS | `
Get-AzureRmVMExtensionImageType | `
Get-AzureRmVMExtensionImage | Select Type, Version
```

## <a name="run-vm-extensions"></a>Ausführen von VM-Erweiterungen

Azure VM-Erweiterungen können auf vorhandenen virtuellen Computern ausgeführt werden, was nützlich ist, um Konfigurationsänderungen vorzunehmen oder die Konnektivität für eine bereits bereitgestellte VM wiederherzustellen. VM-Erweiterungen können darüber hinaus mit der Bereitstellung von Azure Resource Manager-Vorlagen gekoppelt werden. Durch die Verwendung von Erweiterungen mit Resource Manager-Vorlagen können Sie ermöglichen, dass virtuelle Azure-Computer bereitgestellt und konfiguriert werden können, ohne dass Eingriffe nach der Bereitstellung erforderlich werden.

Die folgenden Methoden können verwendet werden, um eine Erweiterung für einen vorhandenen virtuellen Computer auszuführen.

### <a name="powershell"></a>PowerShell

Mehrere PowerShell-Befehle können zum Ausführen einzelner Erweiterungen verwendet werden. Um eine Liste anzuzeigen, führen Sie die folgenden PowerShell-Befehle aus.

```powershell
get-command Set-AzureRM*Extension* -Module AzureRM.Compute
```

Dies erzeugt eine Ausgabe ähnlich der Folgenden:

```powershell
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Set-AzureRmVMAccessExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMADDomainExtension                     2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMAEMExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBackupExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMBginfoExtension                       2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMChefExtension                         2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMCustomScriptExtension                 2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiagnosticsExtension                  2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDiskEncryptionExtension               2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMDscExtension                          2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMExtension                             2.2.0      AzureRM.Compute
Cmdlet          Set-AzureRmVMSqlServerExtension                    2.2.0      AzureRM.Compute
```

Im folgenden Beispiel wird mit der benutzerdefinierten Skripterweiterung ein Skript von einem GitHub-Repository auf den virtuellen Zielcomputer heruntergeladen und dann das Skript ausgeführt. Weitere Informationen zum Verwenden der benutzerdefinierten Skripterweiterung finden Sie unter [Windows-VM – benutzerdefinierte Skripterweiterungen mit Azure Resource Manager-Vorlagen](extensions-customscript.md).

```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" -Name "myCustomScript" `
    -FileUri "https://raw.githubusercontent.com/neilpeterson/nepeters-azure-templates/master/windows-custom-script-simple/support-scripts/Create-File.ps1" `
    -Run "Create-File.ps1" -Location "West US"
```

In diesem Beispiel wird die VM-Zugriffserweiterung zum Zurücksetzen des Administratorkennworts eines virtuellen Windows-Computers verwendet. Weitere Informationen über die VM-Zugriffserweiterung finden Sie unter [Zurücksetzen des Remotedesktopdiensts oder seines Anmeldekennworts in einer Windows-VM](reset-rdp.md).

```powershell
$cred=Get-Credential

Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" -VMName "myVM" -Name "myVMAccess" `
    -Location WestUS -UserName $cred.GetNetworkCredential().Username `
    -Password $cred.GetNetworkCredential().Password -typeHandlerVersion "2.0"
```

Mit dem `Set-AzureRmVMExtension`-Befehl kann jede beliebige VM-Erweiterung gestartet werden. Weitere Informationen finden Sie in der [Set-AzureRmVMExtension-Referenz](https://msdn.microsoft.com/en-us/library/mt603745.aspx).


### <a name="azure-portal"></a>Azure-Portal

Eine VM-Erweiterung kann über das Azure-Portal auf einen vorhandenen virtuellen Computer angewendet werden. Wählen Sie zu diesem Zweck den zu verwendenden virtuellen Computer aus, wählen Sie **Erweiterungen** aus, und klicken Sie auf **Hinzufügen**. Dadurch wird eine Liste der verfügbaren Erweiterungen ausgegeben. Wählen Sie die gewünschte Erweiterung aus, und befolgen Sie die Schritte im Assistenten.

Das folgende Bild zeigt die Installation der Microsoft Antimalware-Erweiterung aus dem Azure-Portal.

![Installieren der Antimalware-Erweiterung](./media/extensions-features/installantimalwareextension.png)

### <a name="azure-resource-manager-templates"></a>Azure-Ressourcen-Manager-Vorlagen

VM-Erweiterungen können einer Azure Resource Manager-Vorlage hinzugefügt und mit der Bereitstellung der Vorlage ausgeführt werden. Das Bereitstellen von Erweiterungen mit einer Vorlage ist nützlich zum Erstellen vollständig konfigurierter Azure-Bereitstellungen. Beispielsweise stammt der folgende JSON-Code aus einer Resource Manager-Vorlage, die einen Satz von virtuellen Computern mit Lastenausgleich und einer Azure SQL-Datenbank bereitstellt und dann auf jedem virtuellen Computer eine .NET Core-Anwendung installiert. Die VM-Erweiterung erledigt die Softwareinstallation.

Weitere Informationen finden Sie in der vollständigen [Resource Manager-Vorlage](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

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
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Weitere Informationen finden Sie unter [Erstellen von Azure Resource Manager-Vorlagen mit Erweiterungen für virtuelle Windows-Computer](extensions-authoring-templates.md).

## <a name="secure-vm-extension-data"></a>Schützen der Daten von VM-Erweiterungen

Beim Ausführen einer VM-Erweiterung ist es möglicherweise erforderlich, vertrauliche Informationen, wie Anmeldeinformationen, Namen von Speicherkonten und Zugriffsschlüssel von Speicherkonten mit aufzunehmen. Viele VM-Erweiterungen beinhalten eine geschützte Konfiguration, die Daten verschlüsselt und sie ausschließlich innerhalb des virtuellen Zielcomputers entschlüsselt. Jede Erweiterung weist ein spezifisches Schema für die geschützte Konfiguration auf, die in der erweiterungsspezifischen Dokumentation ausführlich erläutert wird.

Das folgende Beispiel zeigt eine Instanz der benutzerdefinierten Skripterweiterung für Windows. Beachten Sie, dass der auszuführende Befehl einen Satz Anmeldeinformationen enthält. In diesem Beispiel wird der auszuführende Befehl nicht verschlüsselt.


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
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ],
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

Durch Verschieben der **CommandToExecute**-Eigenschaft in die **protected**-Konfiguration wird die Ausführungszeichenfolge geschützt.

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
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
        "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
        ]
    },
    "protectedSettings": {
        "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
    }
}
```

## <a name="troubleshoot-vm-extensions"></a>Problembehandlung bei VM-Erweiterungen

Für jede VM-Erweiterung können spezifische Schritte zur Problembehandlung verfügbar sein. Wenn Sie beispielsweise die benutzerdefinierte Skripterweiterung verwenden, finden Sie Details zur Skriptausführung lokal auf dem virtuellen Computer, auf dem die Erweiterung ausgeführt wurde. Alle erweiterungsspezifischen Schritte zur Problembehandlung sind ausführlich in der Dokumentation zur Erweiterung erläutert.

Die folgenden Schritte zur Problembehandlung gelten für alle VM-Erweiterungen.

### <a name="view-extension-status"></a>Anzeigen des Erweiterungsstatus

Verwenden Sie nach dem Anwenden einer VM-Erweiterung auf einen virtuellen Computer den folgenden PowerShell-Befehl zum Zurückgeben des Erweiterungsstatus. Ersetzen Sie die Namen der Beispielparameter durch Ihre eigenen Werte. Der `Name`-Parameter nimmt den Namen entgegen, den die Erweiterung zum Zeitpunkt der Ausführung erhält.

```PowerShell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Die Ausgabe sieht wie folgt aus:

```json
ResourceGroupName       : myResourceGroup
VMName                  : myVM
Name                    : myExtensionName
Location                : westus
Etag                    : null
Publisher               : Microsoft.Azure.Extensions
ExtensionType           : DockerExtension
TypeHandlerVersion      : 1.0
Id                      : /subscriptions/mySubscriptionIS/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM/extensions/myExtensionName
PublicSettings          :
ProtectedSettings       :
ProvisioningState       : Succeeded
Statuses                :
SubStatuses             :
AutoUpgradeMinorVersion : False
ForceUpdateTag          :
```

Der Ausführungsstatus von Erweiterungen findet sich ebenfalls im Azure-Portal. Um den Status einer Erweiterung anzuzeigen, wählen Sie den virtuellen Computer aus, wählen Sie **Erweiterungen** und dann die gewünschte Erweiterung aus.

### <a name="rerun-vm-extensions"></a>Erneutes Ausführen von VM-Erweiterungen

In manchen Fällen kann die erneute Ausführung einer VM-Erweiterung erforderlich sein. Das können Sie erreichen, indem Sie die Erweiterung entfernen und sie dann mit einer Ausführungsmethode Ihrer Wahl erneut ausführen. Um eine Erweiterung zu entfernen, führen Sie den folgenden Befehl mit dem Azure PowerShell-Modul aus. Ersetzen Sie die Namen der Beispielparameter durch Ihre eigenen Werte.

```powershell
Remove-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Eine Erweiterung kann auch im Azure-Portal entfernt werden. Gehen Sie dazu wie folgt vor:

1. Wählen Sie einen virtuellen Computer aus.
2. Wählen Sie **Erweiterungen**.
3. Wählen Sie die gewünschte Erweiterung aus.
4. Wählen Sie **Deinstallieren**.

## <a name="common-vm-extensions-reference"></a>Allgemeine VM-Erweiterungsreferenz
| Name der Erweiterung | Beschreibung | Weitere Informationen |
| --- | --- | --- |
| CustomScript-Erweiterung für Windows |Ausführen von Skripts für virtuelle Azure-Computer |[Benutzerdefinierte Skripterweiterung für Windows](extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| DSC-Erweiterung für Windows |PowerShell-DSC-Erweiterung (Desired State Configuration, Konfigurieren des gewünschten Zustands) |[DSC-Erweiterung für Windows](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) |
| Azure-Diagnoseerweiterung |Verwalten der Azure-Diagnose |[Azure-Diagnoseerweiterung](https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/) |
| Erweiterung für den Zugriff auf virtuelle Azure-Computer |Verwalten von Benutzern und Anmeldeinformationen |[Erweiterung für den Zugriff auf virtuelle Computer für Linux](https://azure.microsoft.com/en-us/blog/using-vmaccess-extension-to-reset-login-credentials-for-linux-vm/) |

