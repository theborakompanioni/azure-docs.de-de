<properties
	pageTitle="SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Klassisch) | Microsoft Azure"
	description="In diesem Thema wird die Verwaltung der SQL Server-Agent-Erweiterung beschrieben, mit der bestimmte SQL Server-Verwaltungsaufgaben automatisiert werden. Dazu gehören die automatische Sicherung, das automatische Patchen und die Azure-Schlüsseltresor-Integration. In diesem Thema wird der klassische Bereitstellungsmodus verwendet."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="05/16/2016"
	ms.author="jroth"/>

# SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Klassisch)

> [AZURE.SELECTOR]
- [Ressourcen-Manager](virtual-machines-windows-sql-server-agent-extension.md)
- [Klassisch](virtual-machines-windows-classic-sql-server-agent-extension.md)

Die Erweiterung für SQL Server-IaaS-Agent (SQLIaaSAgent) wird auf virtuellen Azure-Computern zum Automatisieren von Verwaltungsaufgaben ausgeführt. Dieses Thema bietet eine Übersicht über die Dienste, die von der Erweiterung unterstützt werden, sowie Anweisungen zu Installation, Status und Deinstallation.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Ressourcen-Manager-Modell. Die Resource Manager-Version dieses Artikels finden Sie unter [SQL Server-Agent-Erweiterung für virtuelle SQL Server-Computer (Resource Manager)](virtual-machines-windows-sql-server-agent-extension.md).

## Unterstützte Dienste

Die Erweiterung für SQL Server-IaaS-Agent unterstützt die folgenden Verwaltungsaufgaben:

| Verwaltungsfeature | Beschreibung |
|---------------------|-------------------------------|
| **SQL – Automatisierte Sicherung** | Dieser Dienst automatisiert die Planung von Sicherungen für alle Datenbanken für die Standardinstanz von SQL Server auf dem virtuellen Computer. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-sql-automated-backup.md).|
| **SQL – Automatisiertes Patchen** | Mit diesem Dienst können Sie ein Wartungsfenster konfigurieren, in dem Updates für die VM ausgeführt werden. Auf diese Weise werden Updates für Ihre Workload während der Spitzenzeiten vermieden. Weitere Informationen finden Sie unter [Automatisiertes Patchen für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-sql-automated-patching.md).|
| **Azure-Schlüsseltresor-Integration** | Mit diesem Dienst können Sie Azure Key Vault auf Ihrem virtuellen SQL Server-Computer automatisch installieren und konfigurieren. Weitere Informationen finden Sie unter [Konfigurieren der Azure-Schlüsseltresor-Integration für SQL Server auf virtuellen Azure-Computern (klassisch)](virtual-machines-windows-classic-ps-sql-keyvault.md).|

## Voraussetzungen

Anforderungen für die Verwendung der Erweiterung für SQL Server-IaaS-Agent auf Ihrem virtuellen Computer:

**Betriebssystem:**

- Windows Server 2012
- Windows Server 2012 R2

**SQL Server-Versionen:**

- SQL Server 2012
- SQL Server 2014
- SQL Server 2016

**Azure PowerShell:**

- [Herunterladen und Konfigurieren der aktuellen Azure PowerShell-Befehle](../powershell-install-configure.md)

**Gast-Agent des virtuellen Computers:**

- Die BGInfo-Erweiterung wird automatisch auf neuen virtuellen Azure-Computern installiert.

## Installation

Die Erweiterung für SQL Server-IaaS-Agent wird automatisch installiert, wenn Sie eines der Katalogimages für virtuelle SQL Server-Computer bereitstellen.

Wenn Sie einen virtuellen Windows Server-Computer nur mit dem Betriebssystem erstellen, können Sie die Erweiterung manuell mit dem PowerShell-Cmdlet **Set-AzureVMSqlServerExtension** installieren. Verwenden Sie den Befehl, um einen der Agent-Dienste zu konfigurieren, z.B. automatisiertes Patchen. Der virtuelle Computer installiert den Agent, falls er noch nicht installiert wurde. Informationen zum Verwenden von **Set-AzureVMSqlServerExtension** in PowerShell finden Sie in den entsprechenden Themen unter [Unterstützte Dienste](#supported-services) in diesem Artikel.

Bei einer Aktualisierung auf die aktuelle Version der SQL-IaaS-Agent-Erweiterung müssen Sie den virtuellen Computer nach der Aktualisierung neu starten.

>[AZURE.NOTE] Wenn Sie die SQL Server-IaaS-Agent-Erweiterung manuell auf einem virtuellen Computer installieren, müssen Sie ihre Funktionen mithilfe von PowerShell-Befehlen verwenden und verwalten. Die Portalschnittstelle ist in diesem Szenario nicht verfügbar.

## Status

Eine Möglichkeit, zu überprüfen, ob die Erweiterung installiert ist, ist das Anzeigen des Agent-Status im Azure-Portal. Wählen Sie auf dem Blatt für den virtuellen Computer **Alle Einstellungen**, und klicken Sie dann auf **Erweiterungen**. Die Erweiterung **SQLIaaSAgent** sollte aufgeführt werden.

![Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-portal.png)

Sie können auch das Azure PowerShell-Cmdlet **Get-AzureVMSqlServerExtension** verwenden.

	Get-AzureVM –ServiceName "service" –Name "vmname" | Get-AzureVMSqlServerExtension

## Entfernen   

Im Azure-Portal können Sie die Erweiterung deinstallieren, indem Sie in den Eigenschaften des virtuellen Computers auf dem Blatt **Erweiterungen** auf die Auslassungszeichen klicken. Klicken Sie dann auf **Löschen**.

![Deinstallieren der Erweiterung für SQL Server-IaaS-Agent im Azure-Portal](./media/virtual-machines-windows-classic-sql-server-agent-extension/azure-sql-server-iaas-agent-uninstall.png)

Sie können auch das PowerShell-Cmdlet **Remove-AzureVMSqlServerExtension** verwenden.

	Get-AzureVM –ServiceName "service" –Name "vmname" | Remove-AzureVMSqlServerExtension | Update-AzureVM

## Nächste Schritte

Verwenden Sie einen der von der Erweiterung unterstützten Dienste. Weitere Informationen finden Sie in den im Abschnitt [Unterstützte Dienste](#supported-services) in diesem Artikel aufgeführten Themen.

Ausführlichere Informationen zur Verwendung von SQL Server auf virtuellen Azure-Computern finden Sie unter [Übersicht zu SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0525_2016-->