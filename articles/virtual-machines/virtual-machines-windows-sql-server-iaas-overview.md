<properties
	pageTitle="Übersicht über SQL Server auf virtuellen Computern | Microsoft Azure"
	description="Erste Schritte mit der Ausführung von SQL Server-Datenbanken in der Cloud auf Azure Virtual Machines. Dieses Infrastructure-as-a-Service-Modell (IaaS) ermöglicht Ihnen, SQL Server-Workloads in Azure auszuführen."
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
	ms.date="05/18/2016"
	ms.author="jroth"/>

# Übersicht zu SQL Server auf virtuellen Azure-Computern

[SQL Server auf virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) ermöglicht Ihnen, SQL Server-Datenbanken in der Cloud zu hosten. Beispielsweise können Sie eine lokale Datenbank zu einer Azure-VM migrieren, vorkonfiguriert mit Windows Server 2012 R2 und SQL Server 2014 Enterprise-Edition. Aber es gibt viele andere mögliche Szenarien, z.B. Konfigurationen mit mehreren Computern, die hohe Verfügbarkeit oder Hybridarchitekturen mit Verbindung zu lokalen Netzwerken unterstützen.

Eine umfassende Übersicht erhalten Sie im folgenden Video:

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## SQL-Angebote

Die Ausführung von SQL Server auf einem virtuellen Azure-Computer stellt eine Möglichkeit zum Speichern von relationalen Daten in Azure dar. Die verschiedenen Angebote sind in der folgenden Tabelle zusammengefasst.

|&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;| SQL-Angebot | Beschreibung |
|---:|---|---|
|![SQL Server auf virtuellen Azure-Computern](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-virtual-machine.png)|[SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/)|Ausführen von SQL Server auf virtuellen Azure-Computern Direkte Verwaltung des virtuellen Computers und Ausführung Ihrer Datenbank auf Verkaufsversionen von SQL Server. |
|![SQL-Datenbank](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-database.png)|[SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)|Verwenden Sie den SQL-Datenbankdienst, um auf Ihre Datenbank zuzugreifen und sie zu skalieren, ohne die zugrunde liegende Infrastruktur verwalten zu müssen.|
|![SQL Data Warehouse](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-data-warehouse.png)|[SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)|Verwenden Sie Azure SQL Data Warehouse, um große Mengen an relationalen und nicht relationalen Daten zu verarbeiten. Bietet skalierbare Data Warehousing-Funktionen als Dienst.|
|![SQL Server Stretch-Datenbank](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-stretch-database.png)|[SQL Server Stretch-Datenbank](https://azure.microsoft.com/services/sql-server-stretch-database/)|Dynamisches lokales Stretchen von Transaktionsdaten aus Microsoft SQL Server 2016 in Azure.|

>[AZURE.NOTE] Einen vollständigen Vergleich zwischen SQL-VMs und SQL-Datenbank finden Sie unter [Auswählen einer SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../sql-database/data-management-azure-sql-database-and-sql-server-iaas.md).

## Bereitstellen einer SQL Server-VM

Um einen virtuellen Computer mit SQL Server in Azure zu erstellen, müssen Sie zunächst ein Abonnement für die Azure-Plattform erwerben. Sie können ein Azure-Abonnement über [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/) erwerben. Wenn Sie es kostenlos testen möchten, besuchen Sie [kostenlose Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/).

Nachdem Sie sich für ein Abonnement angemeldet haben, besteht die einfachste Möglichkeit zum Bereitstellen eines virtuellen SQL Server-Computers in Azure darin, [aus dem Imagekatalog ein Image eines virtuellen SQL Server-Computers im Azure-Portal bereitzustellen](virtual-machines-windows-portal-sql-server-provision.md). Diese Images enthalten in den Preisen für den virtuellen Computer SQL Server-Lizenzen.

Dabei müssen Sie beachten, dass es zwei Modelle zum Erstellen und Verwalten von virtuellen Azure-Computern gibt: klassisch und mit Ressourcen-Manager. Microsoft empfiehlt für die meisten neuen Bereitstellungen die Verwendung des Ressourcen-Manager-Modells. Weitere Informationen finden Sie unter [Grundlegendes zur Bereitstellung über den Ressourcen-Manager im Vergleich zur klassischen Bereitstellung](../resource-manager-deployment-model.md). Jedes Thema sollte sein Zielmodell deutlich angeben, sofern es nicht sowohl der klassischen Methode als auch dem Resource Manager entspricht, so wie in diesem Artikel.

## Auswählen eines SQL-VM-Image
Die folgende Tabelle enthält eine Matrix der SQL Server-Images, die im Katalog der virtuellen Computer verfügbar sind. Klicken Sie zum Erstellen eines virtuellen Computers für diese Version, Edition und dieses Betriebssystem auf einen der Links in der Tabelle.

|SQL Server-Version|Betriebssystem|SQL Server-Edition|
|---|---|---|
|**SQL Server 2016 RC**|Windows Server 2012 R2|[Auswertung](https://azure.microsoft.com/marketplace/partners/microsoft/sqlserver2016rc3evaluationwindowsserver2012r2/)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014sp1enterprisewindowsserver2012r2/), [Standard](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014sp1standardwindowsserver2012r2/), [Web](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014sp1webwindowsserver2012r2/), [Express](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014sp1expresswindowsserver2012r2/)|
|**SQL Server 2014**|Windows Server 2012 R2|[Enterprise](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014enterprisewindowsserver2012r2/), [Standard](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014standardwindowsserver2012r2/), [Web](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2014webwindowsserver2012r2/)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012r2/), [Standard](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012r2/), [Web](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012r2/)|
|**SQL Server 2012 SP2**|Windows Server 2012|[Enterprise](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2enterprisewindowsserver2012/), [Standard](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2standardwindowsserver2012/), [Web](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2webwindowsserver2012/), [Express](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2012sp2expresswindowsserver2012/)|
|**SQL Server 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2008r2sp3enterprisewindowsserver2008r2/), [Standard](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2008r2sp3standardwindowsserver2008r2/), [Web](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2008r2sp3webwindowsserver2008r2/)|
|**SQL Server 2008 R2 SP3**|Windows Server 2012|[Express](https://azure.microsoft.com//marketplace/partners/microsoft/sqlserver2008r2sp3expresswindowsserver2012/)|

>[AZURE.NOTE] Das Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program, CEIP) ist standardmäßig aktiviert. Bei Bedarf können Sie das CEIP nach der Bereitstellung des virtuellen Computers anpassen oder deaktivieren. Stellen Sie über Remotedesktop eine Verbindung mit dem virtuellen Computer her, und führen Sie das Hilfsprogramm **Fehler- und Verwendungsberichterstellung von SQL Server** aus.

Zusätzlich zu diesen vorkonfigurierten Images können Sie auch [einen virtuellen Azure-Computer](virtual-machines-windows-hero-tutorial.md) ohne vorinstallierte SQL Server-Instanz erstellen. Sie können eine beliebige SQL Server-Instanz darauf installieren, für die Sie über eine Lizenz verfügen. Sie migrieren Ihre Lizenz nach Azure, um SQL Server auf einem virtuellen Azure-Computer auszuführen. Diese Migration erfolgt gemäß [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/). In diesem Szenario bezahlen Sie lediglich für die Compute- und Speicher[kosten](https://azure.microsoft.com/pricing/details/virtual-machines/) in Azure, die im Zusammenhang mit dem virtuellen Computer anfallen.

Um die besten VM-Konfigurationseinstellungen für Ihr SQL Server-Image zu bestimmen, lesen Sie sich [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md) durch. Für Produktionsworkloads ist **DS3** die empfohlene Mindestgröße des virtuellen Computers für SQL Server Enterprise Edition, und **DS2** für Standard Edition.

## Migrieren Ihrer Daten

Nachdem Ihr virtueller SQL Server-Computer eingerichtet wurde und ausgeführt wird, empfiehlt es sich, vorhandene Datenbanken auf den Computer zu migrieren. Eine Liste von Migrationsoptionen und Anleitungen finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

## Hohe Verfügbarkeit

Wenn Sie hohe Verfügbarkeit benötigen, sollten Sie SQL Server-Verfügbarkeitsgruppen konfigurieren. Dies beinhaltet mehrere Azure-VMs in einem virtuellen Netzwerk. Das Azure-Portal bietet eine Vorlage, die diese Konfiguration für Sie einrichtet. Weitere Informationen finden Sie unter [Konfigurieren einer AlwaysOn-Verfügbarkeitsgruppe auf Azure Resource Manager-VMs (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md).

Wenn Sie Ihre Verfügbarkeitsgruppe und den ihr zugeordneten Listener manuell konfigurieren möchten, lesen Sie den Artikel [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md).

Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md).

## Sichern und Wiederherstellen
Bei lokalen Datenbanken kann Azure als sekundäres Datencenter zum Speichern von SQL Server-Sicherungsdateien fungieren. Eine Übersicht der Sicherungs- und Wiederherstellungsoptionen finden Sie unter [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

Die [SQL Server-URL-Sicherung](https://msdn.microsoft.com/library/dn435916.aspx) speichert Azure-Sicherungsdateien im Azure-Blob-Speicher. Mithilfe von [SQL Server Managed Backup](https://msdn.microsoft.com/library/dn449496.aspx) können Sie eine Sicherung und Archivierung in Azure planen. Diese Dienste können mit lokalen SQL Server-Instanzen oder SQL Server auf virtuellen Azure-Computern verwendet werden. Azure-VMs können auch [automatisierte Sicherungen](virtual-machines-windows-classic-sql-automated-backup.md) und [automatische Anwendung von Patches](virtual-machines-windows-classic-sql-automated-patching.md) für SQL Server nutzen.

## Nächste Schritte

Zuerst [erstellen Sie Ihre eigene SQL Server-VM im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

Überprüfen Sie dann [bewährte Methoden für Leistung](virtual-machines-windows-sql-performance.md) und [Migrationstechniken](virtual-machines-windows-migrate-sql.md), wenn Sie erwägen, Ihre SQL Server-Workloads auf virtuelle Azure-Computer zu verschieben.

Wenn Sie weitere Informationen zu SQL Server auf virtuellen Azure-Computern wünschen, lesen Sie den Artikel [SQL Server auf virtuellen Azure-Computern – FAQ](virtual-machines-windows-sql-server-iaas-faq.md). Oder fügen Sie Ihre Kommentare zur Interaktion mit Microsoft und der Community beliebigen SQL-VM-Themen an.

<!---HONumber=AcomDC_0518_2016-->