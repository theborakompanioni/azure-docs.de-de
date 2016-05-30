<properties
	pageTitle="SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager) | Microsoft Azure"
	description="In diesem Thema wird die Verwaltung der SQL Server-Agent-Erweiterung beschrieben, mit der bestimmte SQL Server-Verwaltungsaufgaben automatisiert werden. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure-Schlüsseltresor-Integration. In diesem Thema wird der Resource Manager-Bereitstellungsmodus verwendet."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="05/16/2016"
	ms.author="jroth"/>

# SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassisch](virtual-machines-windows-classic-sql-server-agent-extension.md)

Die Erweiterung für SQL Server-IaaS-Agent (SQLIaaSExtension) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieses Thema bietet eine Übersicht über die Dienste, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]Klassisches Bereitstellungsmodell. Den Artikel zum klassischen Bereitstellungsmodell finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Klassisch)](virtual-machines-windows-classic-sql-server-agent-extension.md).

## Unterstützte Dienste

Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | Beschreibung |
|---------------------|-------------------------------|
| **SQL – Automatisierte Sicherung** | Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer.|
| **SQL – Automatisiertes Patchen** | Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für den virtuellen Computer ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden.|
| **Azure-Schlüsseltresor-Integration** | Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (Resource Manager)](virtual-machines-windows-ps-sql-keyvault.md).|

## Voraussetzungen

Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

- Windows Server 2012, Windows Server 2012 R2 oder höher
- SQL Server 2012, SQL Server 2014 oder höher

Anforderungen für die Verwendung von Powershell-Cmdlets:

- Neueste Azure PowerShell-Version [hier verfügbar](../powershell-install-configure.md)

## Installation

Die Erweiterung für SQL Server-IaaS-Agent wird automatisch installiert, wenn Sie eines der Katalogimages für virtuelle SQL Server-Computer bereitstellen.

Wenn Sie einen virtuellen Windows Server-Computer nur mit dem Betriebssystem erstellen, können Sie die Erweiterung manuell mit dem PowerShell-Cmdlet **Set-AzureVMSqlServerExtension** installieren. Mit dem folgenden Befehl wird die Erweiterung beispielsweise auf einem virtuellen Windows Server-Computer nur mit dem Betriebssystem erstellt und der Computer mit dem Namen „SQLIaaSExtension“ versehen.

	Set-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension" -Version "1.2"

## Status

Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie auf dem Blatt des virtuellen Computers die Option **Alle Einstellungen** aus, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SQLIaaSExtension** sollte aufgeführt sein.

![Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzureVMSqlServerExtension** verwenden.

	Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"

Mit dem Befehl oben wird überprüft, ob der Agent installiert ist. Zudem werden allgemeine Statusinformationen angegeben. Mit den folgenden Befehlen können Sie zudem bestimmte Statusinformationen zur automatischen Sicherung und zum automatischen Patchen abrufen.

	$sqlext = Get-AzureRmVMSqlServerExtension -VMName "vmname" -ResourceGroupName "resourcegroupname"
	$sqlext.AutoPatchingSettings
	$sqlext.AutoBackupSettings

## Entfernen   

Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie auf die Auslassungszeichen auf dem Blatt **Erweiterungen** für die Eigenschaften des virtuellen Computers klicken. Klicken Sie dann auf **Löschen**.

![Deinstallieren der Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-sql-server-agent-extension/azure-rm-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzureRmVMSqlServerExtension** verwenden.

	Remove-AzureRmVMSqlServerExtension -ResourceGroupName "resourcegroupname" -VMName "vmname" -Name "SQLIaasExtension"

## Nächste Schritte

Verwenden Sie einen der von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie in den im Abschnitt [Unterstützte Dienste](#supported-services) in diesem Artikel aufgeführten Themen.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0518_2016-->