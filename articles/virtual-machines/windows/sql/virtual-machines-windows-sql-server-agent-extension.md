---
title: "SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager) | Microsoft Docs"
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
ms.date: 01/09/2017
ms.author: jroth
translationtype: Human Translation
ms.sourcegitcommit: 407b189af12116d633ed505facf4bcfde9be5822
ms.openlocfilehash: 9480c6e2d7f58bfa3934c5895dca8c6a82acbfe8


---
# <a name="sql-server-agent-extension-for-sql-server-vms-resource-manager"></a>SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md)
> 
> 

Die Erweiterung für SQL Server-IaaS-Agent (SQLIaaSExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieses Thema bietet eine Übersicht über die Dienste, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (klassisch)](../sqlclassic/virtual-machines-windows-classic-sql-server-agent-extension.md).

## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | Beschreibung |
| --- | --- |
| **SQL – Automatisierte Sicherung** |Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-backup.md). |
| **SQL – Automatisiertes Patchen** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-sql-automated-patching.md). |
| **Azure-Schlüsseltresor-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure Key Vault-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

**Betriebssystem:**

* Windows Server 2012
* Windows Server 2012 R2

**SQL Server-Versionen:**

* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

**Azure PowerShell:**

* [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](/powershell/azureps-cmdlets-docs)

## <a name="installation"></a>Installation
Die Erweiterung für SQL Server-IaaS-Agent wird automatisch installiert, wenn Sie eines der Katalogimages für virtuelle SQL Server-Computer bereitstellen.

Wenn Sie einen virtuellen Windows Server-Computer nur mit dem Betriebssystem erstellen, können Sie die Erweiterung manuell mit dem PowerShell-Cmdlet **Set-AzureVMSqlServerExtension** installieren. Mit dem folgenden Befehl wird die Erweiterung beispielsweise auf einem virtuellen Windows Server-Computer nur mit dem Betriebssystem erstellt und der Computer mit dem Namen „SQLIaaSExtension“ versehen.

    Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2" -Location "East US 2"

Bei einer Aktualisierung auf die aktuelle Version der SQL-IaaS-Agent-Erweiterung müssen Sie den virtuellen Computer nach der Aktualisierung neu starten.

> [!NOTE]
> Wenn Sie die SQL Server-IaaS-Agent-Erweiterung manuell auf einem virtuellen Windows Server-Computer installieren, müssen Sie ihre Funktionen mithilfe von PowerShell-Befehlen verwenden und verwalten. Die Portaloberfläche steht nur für SQL Server-Katalogimages zur Verfügung.
> 
> 

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




<!--HONumber=Jan17_HO2-->


