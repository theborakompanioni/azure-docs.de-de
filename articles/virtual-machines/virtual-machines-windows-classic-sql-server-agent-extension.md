---
title: SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Klassisch) | Microsoft Docs
description: In diesem Thema wird die Verwaltung der SQL Server-Agent-Erweiterung beschrieben, mit der bestimmte SQL Server-Verwaltungsaufgaben automatisiert werden. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure-Schlüsseltresor-Integration. In diesem Thema wird der klassische Bereitstellungsmodus verwendet.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: jhubbard
editor: ''
tags: azure-service-management

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: jroth

---
# <a name="sql-server-agent-extension-for-sql-server-vms-classic"></a>SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Klassisch)
> [!div class="op_single_selector"]
> * [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
> * [Klassisch](virtual-machines-windows-classic-sql-server-agent-extension.md)
> 
> 

Die Erweiterung für SQL Server-IaaS-Agent (SQLIaaSAgent) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieses Thema bietet eine Übersicht über die Dienste, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

Die Resource Manager-Version dieses Artikels finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager)](virtual-machines-windows-sql-server-agent-extension.md).

## <a name="supported-services"></a>Unterstützte Dienste
Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | Beschreibung |
| --- | --- |
| **SQL – Automatisierte Sicherung** |Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-sql-automated-backup.md). |
| **SQL – Automatisiertes Patchen** |Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-sql-automated-patching.md). |
| **Azure-Schlüsseltresor-Integration** |Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-ps-sql-keyvault.md). |

## <a name="prerequisites"></a>Voraussetzungen
Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

### <a name="operating-system"></a>Betriebssystem:
* Windows Server 2012
* Windows Server 2012 R2

### <a name="sql-server-versions"></a>SQL Server-Versionen:
* SQL Server 2012
* SQL Server 2014
* SQL Server 2016

### <a name="azure-powershell"></a>Azure PowerShell:
[Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](../powershell-install-configure.md)

Starten Sie Windows PowerShell, und verknüpfen Sie dann mithilfe des Befehls **Add-AzureAccount** PowerShell mit Ihrem Azure-Abonnement.

    Add-AzureAccount

Wenn Sie über mehrere Abonnements verfügen, verwenden Sie **Select-AzureSubscription** , um das Abonnement auszuwählen, das die klassische Ziel-VM enthält.

    Select-AzureSubscription -SubscriptionName <subscriptionname>

Zu diesem Zeitpunkt können Sie mit dem Befehl **Get-AzureVM** eine Liste der klassischen virtuellen Computer und der zugeordneten Dienstnamen abrufen.

    Get-AzureVM

## <a name="installation"></a>Installation
Bei klassischen virtuellen Computern müssen Sie PowerShell für die Installation der Erweiterung für SQL Server-IaaS-Agent verwenden und die zugehörigen Dienste konfigurieren. Verwenden Sie zum Installieren der Erweiterung das PowerShell-Cmdlet **Set-AzureVMSqlServerExtension** . Mit dem folgenden Befehl wird die Erweiterung beispielsweise auf einem virtuellen Windows Server-Computer (klassisch) erstellt und „SQLIaaSExtension“ genannt.

    Get-AzureVM -ServiceName <vmservicename> -Name <vmname> | Set-AzureVMSqlServerExtension -ReferenceName "SQLIaasExtension" -Version "1.2" | Update-AzureVM

Bei einer Aktualisierung auf die aktuelle Version der SQL-IaaS-Agent-Erweiterung müssen Sie den virtuellen Computer nach der Aktualisierung neu starten.

> [!NOTE]
> Auf klassischen virtuellen Computern ist keine Option zum Installieren und Konfigurieren der Erweiterung für SQL-IaaS-Agent über das Portal verfügbar.
> 
> 

## <a name="status"></a>Status
Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie auf dem Blatt des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SQLIaaSAgent** sollte aufgeführt werden.

![Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzureVMSqlServerExtension** verwenden.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## <a name="removal"></a>Entfernen
Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie in den Eigenschaften des virtuellen Computers auf dem Blatt **Erweiterungen** auf die Auslassungspunkte klicken. Klicken Sie dann auf **Löschen**.

![Deinstallieren der Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzureVMSqlServerExtension** verwenden.

    Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## <a name="next-steps"></a>Nächste Schritte
Verwenden Sie einen der von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie in den im Abschnitt [Unterstützte Dienste](#supported-services) in diesem Artikel aufgeführten Themen.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=Oct16_HO2-->


