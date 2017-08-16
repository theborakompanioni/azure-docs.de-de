---
title: Automatisieren der Verwaltungsaufgaben auf virtuellen SQL-Computern (Resource Manager) | Microsoft-Dokumentation
description: "In diesem Thema wird die Verwaltung der SQL Server-Agent-Erweiterung beschrieben, mit der bestimmte SQL Server-Verwaltungsaufgaben automatisiert werden. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure-Schlüsseltresor-Integration. In diesem Thema wird der Resource Manager-Bereitstellungsmodus verwendet."
services: virtual-machines-windows
documentationcenter: 
author: rothja
manager: jhubbard
editor: 
tags: azure-resource-manager
ms.assetid: effe4e2f-35b5-490a-b5ef-b06746083da4
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: caaf10d385c8df8f09a076d0a392ca0d5df64ed2
ms.openlocfilehash: 03cc1d2d47ce8194b293824b29a07cef423be34d
ms.contentlocale: de-de
ms.lasthandoff: 08/08/2017

---
# <a name="automate-management-tasks-on-azure-virtual-machines-with-the-sql-server-agent-extension-resource-manager"></a>Automatisieren von Verwaltungsaufgaben auf virtuellen Azure-Computern mit der SQL Server-Agent-Erweiterung (Resource Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](../classic/sql-server-agent-extension.md)
> 
> 

Die Erweiterung für SQL Server-IaaS-Agent (SQLIaaSExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieses Thema bietet eine Übersicht über die Dienste, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](../classic/sql-server-agent-extension.md).

## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | Beschreibung |
| --- | --- |
| **SQL – Automatisierte Sicherung** |Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **SQL – Automatisiertes Patchen** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure-Schlüsseltresor-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

Nach der Installation und Ausführung stellt die SQL Server-IaaS-Agent-Erweiterung diese Verwaltungsfeatures im Bereich „SQL Server“ des virtuellen Computers im Azure-Portal bereit. Außerdem stehen sie für SQL Server-Marketplace-Images über Azure PowerShell und bei der manuellen Installation der Erweiterung über Azure PowerShell zur Verfügung. 

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

**Betriebssystem:**

* Windows Server 2012
* Windows Server 2012 R2
* Windows Server 2016

**SQL Server-Versionen:**

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell:**

* [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](/powershell/azure/overview)

## <a name="installation"></a>Installation
Die Erweiterung für SQL Server-IaaS-Agent wird automatisch installiert, wenn Sie eines der Katalogimages für virtuelle SQL Server-Computer bereitstellen. Falls Sie die Erweiterung auf einer dieser SQL Server-VMs manuell neu installieren müssen, verwenden Sie den folgenden PowerShell-Befehl:

```powershell
Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"
```

Es ist auch möglich, die SQL Server-IaaS-Agent-Erweiterung auf einem virtuellen Computer mit ausschließlich dem Betriebssystem Windows Server zu installieren. Dies wird nur unterstützt, wenn Sie auch SQL Server manuell auf diesem Computer installiert haben. Verwenden Sie zum manuellen Installieren der Erweiterung ebenfalls das PowerShell-Cmdlet **Set-AzureVMSqlServerExtension**.

> [!NOTE]
> Wenn Sie die SQL Server-IaaS-Agent-Erweiterung manuell auf einer VM mit ausschließlich dem Betriebssystem Windows Server installieren, können Sie die SQL Server-Konfigurationseinstellungen nicht im Azure-Portal verwalten. In diesem Szenario müssen Sie alle Änderungen mit PowerShell vornehmen.

## <a name="status"></a>Status
Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie auf dem Blatt des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SQLIaaSExtension** sollte aufgeführt sein.

![Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzureVMSqlServerExtension** verwenden.

    Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Mit dem Befehl oben wird überprüft, ob der Agent installiert ist. Zudem werden allgemeine Statusinformationen angegeben. Mit den folgenden Befehlen können Sie zudem bestimmte Statusinformationen zur automatischen Sicherung und zum automatischen Patchen abrufen.

    $sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
    $sqlext.AutoPatchingSettings
    $sqlext.AutoBackupSettings

## <a name="removal"></a>Entfernen
Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie in den Eigenschaften des virtuellen Computers auf dem Blatt **Erweiterungen** auf die Auslassungspunkte klicken. Klicken Sie dann auf **Löschen**.

![Deinstallieren der Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzureRmVMSqlServerExtension** verwenden.

    Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie einen der von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie in den im Abschnitt [Unterstützte Dienste](#supported-services) in diesem Artikel aufgeführten Themen.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).


