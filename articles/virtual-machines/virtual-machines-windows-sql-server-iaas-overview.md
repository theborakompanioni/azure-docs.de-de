---
title: Übersicht über SQL Server auf virtuellen Azure-Computern | Microsoft Docs
description: Hier erfahren Sie, wie Sie vollwertige SQL Server-Editionen auf virtuellen Azure-Computern ausführen. Außerdem finden Sie hier direkte Links zu allen SQL Server-VM-Images sowie zu verwandten Inhalten.
services: virtual-machines-windows
documentationcenter: ''
author: rothja
manager: jhubbard
editor: ''
tags: azure-service-management

ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: jroth

---
# <a name="overview-of-sql-server-on-azure-virtual-machines"></a>Übersicht über SQL Server auf virtuellen Azure-Computern
Dieses Thema beschreibt die Optionen im Zusammenhang mit der Ausführung von SQL Server auf virtuellen Azure-Computern (VMs) und enthält [Links zu Portal-Images](#option-1-create-a-sql-vm-with-per-minute-licensing) sowie eine Übersicht über [allgemeine Aufgaben](#manage-your-sql-vm).

> [!NOTE]
> Wenn Sie bereits mit SQL Server vertraut sind und nur Informationen zum Bereitstellen eines virtuellen Computers mit SQL Server benötigen, wechseln Sie zu [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md).
> 
> 

## <a name="overview"></a>Übersicht
Wenn Sie ein Datenbankadministrator oder Entwickler sind, stellen Azure-VMs eine Möglichkeit zum Verschieben Ihrer lokalen SQL Server-Workloads und -Anwendungen in die Cloud dar. Das folgende Video enthält eine technische Übersicht über SQL Server Azure-VMs.

> [!VIDEO https://channel9.msdn.com/Events/DataDriven/SQLServer2016/Azure-VM-is-the-best-platform-for-SQL-Server-2016/player]
> 
> 

Im Video geht es um die folgenden Bereiche:

| Zeit | Bereich |
| --- | --- |
| 00:21 |Was sind Azure-VMs? |
| 01:45 |Sicherheit |
| 02:50 |Konnektivität |
| 03:30 |Speicherzuverlässigkeit und -leistung |
| 05:20 |VM-Größen |
| 05:54 |Hohe Verfügbarkeit und SLA |
| 07:30 |Konfigurationsunterstützung |
| 08:00 |Überwachung |
| 08:32 |Demo: Erstellen einer SQL Server 2016-VM |

> [!NOTE]
> Im Video geht es um die Verwendung von SQL Server 2016, aber unter Azure werden VM-Images für viele Versionen von SQL Server bereitgestellt, z.B. 2008, 2012, 2014 und 2016. 
> 
> 

## <a name="understand-your-options"></a>Informationen zu Ihren Optionen
Es gibt viele Gründe, die für das Hosten von Daten in Azure sprechen. Wenn Sie Ihre Anwendung nach Azure verschieben, wird die Leistung verbessert, indem Sie auch die Daten verschieben. Aber es gibt noch weitere Vorteile. Sie haben automatisch Zugriff auf mehrere Rechenzentren, um weltweite Präsenz und die Notfallwiederherstellung zu ermöglichen. Außerdem sind die Daten dauerhaft stark geschützt.

Die Ausführung von SQL Server auf Azure-VMs ist eine Möglichkeit zum Speichern Ihrer relationalen Daten in Azure. Die folgende Tabelle enthält eine kurze Zusammenfassung der SQL-Angebote in Azure.

| &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; | SQL-Angebot | Beschreibung |
| ---:| --- | --- |
| ![SQL Server auf virtuellen Azure-Computern](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-virtual-machine.png) |[SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) |Führen Sie SQL Server auf virtuellen Azure-Computern aus (Schwerpunkt dieses Themas). Direkte Verwaltung des virtuellen Computers und Ausführung Ihrer Datenbank auf Verkaufsversionen von SQL Server. |
| ![SQL-Datenbank](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-database.png) |[SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) |Verwenden Sie den SQL-Datenbankdienst, um auf Ihre Datenbank zuzugreifen und sie zu skalieren, ohne die zugrunde liegende Infrastruktur verwalten zu müssen. |
| ![SQL Data Warehouse](./media/virtual-machines-windows-sql-server-iaas-overview/azure-sql-data-warehouse.png) |[SQL Data Warehouse](https://azure.microsoft.com/en-us/services/sql-data-warehouse/) |Verwenden Sie Azure SQL Data Warehouse, um große Mengen an relationalen und nicht relationalen Daten zu verarbeiten. Bietet skalierbare Data Warehousing-Funktionen als Dienst. |
| ![SQL Server Stretch-Datenbank](./media/virtual-machines-windows-sql-server-iaas-overview/sql-server-stretch-database.png) |[SQL Server Stretch-Datenbank](https://azure.microsoft.com/en-us/services/sql-server-stretch-database/) |Dynamisches lokales Stretchen von Transaktionsdaten aus Microsoft SQL Server 2016 in Azure. |

Mit diesen unterschiedlichen Optionen ist die Ausführung von SQL Server auf Azure-VMs für mehrere Szenarien eine gute Wahl. Beispielsweise kann es sein, dass Sie die Azure-VM so konfigurieren möchten, dass sie einem lokalen SQL Server-Computer möglichst genau ähnelt. Unter Umständen möchten Sie auch weitere Anwendungen und Dienste auf demselben Datenbankserver ausführen. Es gibt zwei Ressourcen, die für das Durchdenken von weiteren Entscheidungsfaktoren hilfreich sind:

* [SQL Server auf virtuellen Azure-Computern](https://azure.microsoft.com/services/virtual-machines/sql-server/) enthält eine Übersicht über die besten Szenarien für die Verwendung von SQL Server auf Azure-VMs. 
* Einen vollständigen Vergleich zwischen SQL-Datenbank und SQL Server auf einer VM finden Sie unter [Auswählen einer SQL Server-Cloudoption: Azure SQL-Datenbank (PaaS) oder SQL Server auf Azure-VMs (IaaS)](../sql-database/sql-database-paas-vs-sql-server-iaas.md).

## <a name="create-a-new-sql-vm"></a>Erstellen einer neuen SQL-VM
Die folgenden Abschnitte enthalten direkte Links zum Azure-Portal für die Katalogimages für virtuelle SQL Server-Computer. Je nach ausgewähltem Image fallen entweder Kosten für die SQL Server-Lizenzierung pro Minute an, oder Sie nutzen eine eigene Lizenz (Bring Your Own License, BYOL).

Eine Schritt-für-Schritt-Anleitung für diesen Prozess finden Sie im Tutorial [Bereitstellen eines virtuellen Computers mit SQL Server im Azure-Portal](virtual-machines-windows-portal-sql-server-provision.md). Sehen Sie sich auch den Artikel [Optimale Verfahren für die Leistung für SQL Server auf virtuellen Computern in Azure](virtual-machines-windows-sql-performance.md) an. Darin wird beschrieben, wie Sie bei der Bereitstellung die richtige Computergröße und andere verfügbare Features auswählen.

## <a name="option-1:-create-a-sql-vm-with-per-minute-licensing"></a>Option 1: Erstellen einer SQL-VM mit minutenbezogener Lizenzierung
Die folgende Tabelle enthält eine Matrix der SQL Server-Images, die im Katalog der virtuellen Computer verfügbar sind. Klicken Sie auf einen beliebigen Link, um mit dem Erstellen einer neuen SQL-VM mit Ihren Angaben zu Version, Edition und Betriebssystem zu beginnen.

| Version | Betriebssystem | Edition |
| --- | --- | --- |
| **SQL 2016** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMEnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMStandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMWebWindowsServer2012R2), [Dev](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMDeveloperWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2016RTMExpressWindowsServer2012R2) |
| **SQL 2014 SP1** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2014SP1ExpressWindowsServer2012R2) |
| **SQL 2014** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2014EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2014StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2014WebWindowsServer2012R2) |
| **SQL 2012 SP3** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3WebWindowsServer2012R2), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP3ExpressWindowsServer2012R2) |
| **SQL 2012 SP2** |Windows Server 2012 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012R2) |
| **SQL 2012 SP2** |Windows Server 2012 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2EnterpriseWindowsServer2012), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2StandardWindowsServer2012), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2WebWindowsServer2012), [Express](https://portal.azure.com/#create/Microsoft.SQLServer2012SP2ExpressWindowsServer2012) |
| **SQL 2008 R2 SP3** |Windows Server 2008 R2 |[Enterprise](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3EnterpriseWindowsServer2008R2), [Standard](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3StandardWindowsServer2008R2), [Web](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3WebWindowsServer2008R2) |
| **SQL 2008 R2 SP3** |Windows Server 2012 |[Express](https://portal.azure.com/#create/Microsoft.SQLServer2008R2SP3ExpressWindowsServer2012) |

## <a name="option-2:-create-a-sql-vm-with-an-existing-license"></a>Option 2: Erstellen einer SQL-VM mit einer vorhandenen Lizenz
Sie können auch Ihre eigene Lizenz nutzen (BYOL). In diesem Fall zahlen Sie nur für die VM, ohne dass zusätzliche Gebühren für die SQL Server-Lizenzierung anfallen. Nutzen Sie unten die Matrix mit SQL Server-Versionen, -Editionen und Betriebssystemen, wenn Sie Ihre eigene Lizenz verwenden möchten. Im Portal haben diese Imagenamen das Präfix **{BYOL}**.

| Version | Betriebssystem | Edition |
| --- | --- | --- |
| **SQL Server 2016** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2016RTMStandardWindowsServer2012R2) |
| **SQL Server 2014 SP1** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2014SP1StandardWindowsServer2012R2) |
| **SQL Server 2012 SP2** |Windows Server 2012 R2 |[Enterprise BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3EnterpriseWindowsServer2012R2), [Standard BYOL](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2012SP3StandardWindowsServer2012R2) |

> [!IMPORTANT]
> Zum Verwenden von BYOL-VM-Images müssen Sie über ein Enterprise Agreement mit [Lizenzmobilität durch Software Assurance für Azure](https://azure.microsoft.com/pricing/license-mobility/) verfügen. Außerdem benötigen Sie eine gültige Lizenz für die Version/Edition von SQL Server, die Sie verwenden möchten. Sie müssen [gegenüber Microsoft die erforderlichen BYOL-Informationen angeben](http://d36cz9buwru1tt.cloudfront.net/License_Mobility_Customer_Verification_Guide.pdf). Dies muss innerhalb von **zehn Tagen** ab Bereitstellung Ihres virtuellen Computers geschehen.
> 
> 

## <a name="manage-your-sql-vm"></a>Verwalten der SQL-VM
Nach der Bereitstellung der SQL Server-VM stehen mehrere optionale Verwaltungsaufgaben an. In vielerlei Hinsicht konfigurieren und verwalten Sie SQL Server genauso wie eine lokale SQL Server-Instanz. Einige Aufgaben gelten aber speziell für Azure. In den folgenden Abschnitten wird auf einige dieser Bereiche mit Links zu weiteren Informationen näher eingegangen.

### <a name="connect-to-the-vm"></a>Herstellen der Verbindung zur VM
Einer der grundlegendsten Verwaltungsschritte ist die Herstellung einer Verbindung mit der SQL Server-VM über Tools, z.B. SQL Server Management Studio (SSMS). Eine Anleitung zum Herstellen der Verbindung mit Ihrer neuen SQL Server-VM finden Sie unter [Verbinden mit SQL Server-Instanzen auf virtuellen Azure-Computern](virtual-machines-windows-sql-connect.md).

### <a name="migrate-your-data"></a>Migrieren Ihrer Daten
Wenn Sie über eine vorhandene Datenbank verfügen, empfiehlt sich die Verschiebung auf die neu bereitgestellte SQL-VM. Eine Liste mit Migrationsoptionen und Anleitungen finden Sie unter [Migrieren einer SQL Server-Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

### <a name="configure-high-availability"></a>Konfigurieren hoher Verfügbarkeit
Wenn Sie hohe Verfügbarkeit benötigen, sollten Sie SQL Server-Verfügbarkeitsgruppen konfigurieren. Dies beinhaltet mehrere Azure-VMs in einem virtuellen Netzwerk. Das Azure-Portal bietet eine Vorlage, die diese Konfiguration für Sie einrichtet. Weitere Informationen finden Sie unter [Konfigurieren einer AlwaysOn-Verfügbarkeitsgruppe auf Azure Resource Manager-VMs (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups.md). Wenn Sie Ihre Verfügbarkeitsgruppe und den ihr zugeordneten Listener manuell konfigurieren möchten, helfen Ihnen die Informationen im Artikel [Konfigurieren von AlwaysOn-Verfügbarkeitsgruppen in einem virtuellen Azure-Computer (GUI)](virtual-machines-windows-portal-sql-alwayson-availability-groups-manual.md)weiter.

Weitere Informationen finden Sie unter [Hochverfügbarkeit und Notfallwiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-high-availability-dr.md).

### <a name="back-up-your-data"></a>Sichern Ihrer Daten
Für virtuelle Azure-Computer kann die [automatisierte Sicherung](virtual-machines-windows-sql-automated-backup.md)verwendet werden, bei der regelmäßig Sicherungen Ihrer Datenbank im Blobspeicher erstellt werden. Sie können dieses Verfahren auch manuell verwenden. Weitere Informationen finden Sie unter [Verwenden von Azure Storage für SQL Server-Sicherung und -Wiederherstellung](virtual-machines-windows-use-storage-sql-server-backup-restore.md). Eine Übersicht über alle Sicherungs- und Wiederherstellungsoptionen finden Sie unter [Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern](virtual-machines-windows-sql-backup-recovery.md).

### <a name="automate-updates"></a>Automatisieren von Updates
Für virtuelle Azure-Computer kann das [automatisierte Patchen](virtual-machines-windows-sql-automated-patching.md) verwendet werden, um ein Wartungsfenster zum automatischen Installieren wichtiger Windows- und SQL Server-Updates zu planen.

### <a name="customer-experience-improvement-program-(ceip)"></a>Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program, CEIP)
Das Programm zur Verbesserung der Benutzerfreundlichkeit (Customer Experience Improvement Program, CEIP) ist standardmäßig aktiviert. Es sendet in regelmäßigen Abständen Berichte an Microsoft, damit die Nutzung von SQL Server verbessert werden kann. Für CEIP ist nur dann eine Verwaltungsaufgabe erforderlich, wenn Sie das Programm nach der Bereitstellung deaktivieren möchten. Sie können CEIP anpassen oder deaktivieren, indem Sie eine Verbindung mit der VM per Remotedesktop herstellen. Führen Sie anschließend das SQL Server-Hilfsprogramm **Fehler- und Verwendungsberichterstellung** aus. Befolgen Sie die Anleitung, um die Berichterstellung zu deaktivieren. 

Weitere Informationen finden Sie im Abschnitt „CEIP“ des Themas [Akzeptieren von Lizenzbedingungen](https://msdn.microsoft.com/library/ms143343.aspx). 

## <a name="next-steps"></a>Nächste Schritte
[Sehen Sie sich den Lernpfad für SQL Server auf virtuellen Azure-Computern an](https://azure.microsoft.com/documentation/learning-paths/sql-azure-vm/) .

Haben Sie noch eine Frage? Lesen Sie sich zunächst [SQL Server auf virtuellen Azure-Computern – FAQ](virtual-machines-windows-sql-server-iaas-faq.md)durch. Darüber hinaus können Sie Ihre Fragen und Kommentare auch am Ende eines SQL-VM-Themas hinzufügen, um mit Microsoft und der Community zu interagieren.

<!--HONumber=Oct16_HO2-->


