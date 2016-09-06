<properties
	pageTitle="Übersicht über SQL Server auf virtuellen Azure-Computern | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie vollwertige SQL Server-Editionen auf virtuellen Azure-Computern ausführen. Außerdem finden Sie hier direkte Links zu allen SQL Server-VM-Images sowie zu verwandten Inhalten."
	services="virtual-machines-windows"
	documentationCenter=""
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-service-management"/>

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="08/29/2016"
	ms.author="jroth"/>

# Übersicht über SQL Server auf virtuellen Azure-Computern

Dieses Thema beschreibt die Optionen im Zusammenhang mit der Ausführung von SQL Server auf virtuellen Azure-Computern und enthält [Links zu Portal-Images](#option-1-deploy-a-sql-vm-per-minute-licensing) sowie eine Übersicht über [allgemeine Aufgaben](#manage-your-sql-vm).

>[AZURE.NOTE] Wenn Sie bereits mit SQL Server vertraut sind und nur Informationen zum Bereitstellen eines virtuellen Computers mit SQL Server benötigen, wechseln Sie zu [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).

## Übersicht
Es kann beispielsweise sein, dass Sie ein Datenbankadministrator sind und Ihre lokalen SQL Server-Workloads in die Cloud verschieben möchten. Vielleicht sind Sie auch Entwickler und möchten die Funktionen relationaler Datenbanken von SQL Server für Ihre Azure-Anwendung nutzen. Welchen Vorteil hat es, SQL Server-Workloads auf virtuellen Azure-Computern auszuführen? Im folgenden Übersichtsvideo werden die Vorteile dargestellt, und außerdem erhalten Sie eine technische Übersicht.

> [AZURE.VIDEO data-driven-sql-server-2016-azure-vm-is-the-best-platform-for-sql-server-2016]

## Evaluieren der Vorteile

Evaluieren Sie zunächst, welche Vorteile sich für Sie durch die Verwendung von SQL Server auf Azure VMs ergeben.

Wenn Sie andere Workloads nach Azure verschieben (beispielsweise eine Unternehmensanwendung), ist es sinnvoll, auch alle abhängigen SQL Server-Datenbanken nach Azure zu verschieben. So erzielen Sie eine bessere Leistung. Das Hosten von SQL Server auf Azure VMs hat aber noch weitere Vorteile. Beispielsweise haben Sie automatisch Zugriff auf mehrere Rechenzentren, um weltweite Präsenz und die Notfallwiederherstellung zu ermöglichen. Eine vollständige Liste mit den Szenarien und Vorteilen finden Sie auf der Produktseite [SQL Server auf virtuellen Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/).

> [AZURE.NOTE] Sehen Sie sich beim Evaluieren von SQL Server auf virtuellen Azure-Computern auch die anderen Speicher- und SQL-Optionen unter Azure an – etwa [SQL-Datenbank](../sql-database/sql-database-technical-overview.md), [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) und [SQL Server Stretch-Datenbank](../sql -server-stretch-database/sql-server-stretch-database-overview.md). Einen ausführlichen Vergleich finden Sie unter [Wählen Sie eine SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

Nachdem Sie sich für die Ausführung von SQL Server auf Azure VMs entschieden haben, müssen Sie unter anderem zuerst entscheiden, ob Sie ein VM-Image verwenden, in dem die SQL Server-Lizenzkosten enthalten sind. Eine andere Möglichkeit ist „Bring Your Own License“ (BYOL). Hierbei zahlen Sie nur für die VM selbst. Diese Optionen werden in den nächsten beiden Abschnitten beschrieben.

## Option 1: Bereitstellen einer SQL-VM (minutenbezogene Lizenzierung)
Die folgende Tabelle enthält eine Matrix der SQL Server-Images, die im Katalog der virtuellen Computer verfügbar sind. Klicken Sie auf einen beliebigen Link, um mit dem Erstellen einer neuen SQL-VM mit Ihren Angaben zu Version, Edition und Betriebssystem zu beginnen. Alle Images enthalten [SQL Server-Lizenzierungskosten](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

Eine Schritt-für-Schritt-Anleitung ist im Tutorial [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md) verfügbar. Sehen Sie sich auch den Artikel [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md) an. Darin wird beschrieben, wie Sie bei der Bereitstellung die richtige Computergröße und andere verfügbare Features auswählen.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQL 2016**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2)|
|**SQL 2014 SP1**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2)|
|**SQL 2014**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2)|
|**SQL 2012 SP3**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2)|
|**SQL 2012 SP2**|Windows Server 2012|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012)|
|**SQL 2008 R2 SP3**|Windows Server 2008 R2|[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2)|
|**SQL 2008 R2 SP3**|Windows Server 2012|[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012)|

## Option 2: Bereitstellen einer SQL-VM (BYOL)
Die andere Option besteht darin, „Bring Your Own License“ (BYOL) zu verwenden. In diesem Fall zahlen Sie nur für die VM, ohne dass zusätzliche Gebühren für die SQL Server-Lizenzierung anfallen. Nutzen Sie unten die Matrix mit SQL Server-Versionen, -Editionen und Betriebssystemen, wenn Sie Ihre eigene Lizenz verwenden möchten. Im Portal haben die Imagenamen das Präfix **{BYOL}**.

> [AZURE.IMPORTANT] Zum Verwenden von BYOL-VM-Images müssen Sie über ein Enterprise Agreement mit [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/) verfügen. Außerdem benötigen Sie eine gültige Lizenz für die Version/Edition von SQL Server, die Sie verwenden möchten. Sie müssen [gegenüber Microsoft die erforderlichen BYOL-Informationen angeben](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf). Dies muss innerhalb von **zehn Tagen** ab Bereitstellung Ihres virtuellen Computers geschehen.

In diesem Zusammenhang gilt die Anleitung im [Tutorial zur Bereitstellung](virtual-machines-windows-portal-sql-server-provision.md), Sie müssen jedoch eine der folgenden **BYOL**-Imageoptionen verwenden. Sehen Sie sich auch den Artikel [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md) an. Darin wird beschrieben, wie Sie bei der Bereitstellung die richtige Computergröße und andere verfügbare Features auswählen.

|Version|Betriebssystem|Edition|
|---|---|---|
|**SQL Server 2016**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2)|
|**SQL Server 2014 SP1**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2)|
|**SQL Server 2012 SP2**|Windows Server 2012 R2|[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2)|

## Verwalten der SQL-VM
Nach der Bereitstellung der SQL Server-VM stehen mehrere optionale Verwaltungsaufgaben an. Größtenteils konfigurieren und verwalten Sie SQL Server genauso wie in der lokalen Umgebung. Einige Aufgaben gelten aber speziell für Azure. In den folgenden Abschnitten wird auf einige dieser Bereiche mit Links zu weiteren Informationen näher eingegangen.

### Migrieren Ihrer Daten

Wenn Sie über eine vorhandene Datenbank verfügen, empfiehlt sich die Verschiebung auf die neu bereitgestellte SQL-VM. Eine Liste mit Migrationsoptionen und Anleitungen finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

### Konfigurieren hoher Verfügbarkeit

Wenn Sie hohe Verfügbarkeit benötigen, sollten Sie SQL Server-Verfügbarkeitsgruppen konfigurieren. Dies beinhaltet mehrere Azure-VMs in einem virtuellen Netzwerk. Das Azure-Portal bietet eine Vorlage, die diese Konfiguration für Sie einrichtet. Weitere Informationen finden Sie unter [Konfigurieren einer AlwaysOn-Verfügbarkeitsgruppe auf Azure Resource Manager-VMs (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Wenn Sie Ihre Verfügbarkeitsgruppe und den ihr zugeordneten Listener manuell konfigurieren möchten, helfen Ihnen die Informationen im Artikel [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md) weiter.

Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md).

### Sichern Ihrer Daten
Für virtuelle Azure-Computer kann die [automatisierte Sicherung](virtual-machines-windows-sql-automated-backup.md) verwendet werden, bei der regelmäßig Sicherungen Ihrer Datenbank im Blobspeicher erstellt werden. Sie können dieses Verfahren auch manuell verwenden. Weitere Informationen finden Sie unter [Verwenden von Azure Storage für SQL Server-Sicherung und -Wiederherstellung](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Eine Übersicht über alle Sicherungs- und Wiederherstellungsoptionen finden Sie unter [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

### Automatisieren von Updates
Für virtuelle Azure-Computer kann das [automatisierte Patchen](virtual-machines-windows-sql-automated-patching.md) verwendet werden, um ein Wartungsfenster zum automatischen Installieren wichtiger Windows- und SQL Server-Updates zu planen.

### Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program, CEIP)
Das Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program, CEIP) ist standardmäßig aktiviert. Dies ist keine Verwaltungsaufgabe, es sei denn, Sie möchten CEIP nach der Bereitstellung deaktivieren. Sie können CEIP anpassen oder deaktivieren, indem Sie eine Verbindung mit der VM per Remotedesktop herstellen. Führen Sie anschließend das SQL Server-Hilfsprogramm **Fehler- und Verwendungsberichterstellung** aus. Befolgen Sie die Anleitung, um die Berichterstellung zu deaktivieren.

## Nächste Schritte
[Sehen Sie sich den Lernpfad für SQL Server auf virtuellen Azure-Computern an](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/).

Haben Sie noch eine Frage? Lesen Sie sich zunächst [SQL Server auf virtuellen Azure-Computern – FAQ](virtual-machines-windows-sql-server-iaas-faq.md) durch. Darüber hinaus können Sie Ihre Fragen und Kommentare auch am Ende eines SQL-VM-Themas hinzufügen, um mit Microsoft und der Community zu interagieren.

<!---HONumber=AcomDC_0831_2016-->