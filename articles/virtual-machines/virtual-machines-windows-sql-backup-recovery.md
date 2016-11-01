<properties
	pageTitle="Sicherung und Wiederherstellung für SQLServer | Microsoft Azure"
	description="Erläutert Überlegungen zur Sicherung und Wiederherstellung für SQL Server-Datenbanken auf virtuellen Azure-Computern."
	services="virtual-machines-windows"
	documentationCenter="na"
	authors="rothja"
	manager="jhubbard"
	editor=""
	tags="azure-resource-management" />

<tags
	ms.service="virtual-machines-windows"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows-sql-server"
	ms.workload="infrastructure-services"
	ms.date="08/19/2016"
	ms.author="jroth" />

# Sicherung und Wiederherstellung für SQL Server auf virtuellen Azure-Computern

## Übersicht

Das Sichern von Daten in SQL Server-Datenbanken ist ein wichtiger Bestandteil der Strategie zum Schutz vor Datenverlust aufgrund von Anwendungs- oder Benutzerfehlern. Dies gilt in gleichem Maße auch für SQL Server auf virtuellen Azure-Computern (VMs).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Für SQL Server auf Azure-VMs können Sie systemeigene Sicherungs- und Wiederherstellungsverfahren mit angefügten Datenträgern als Ziel der Sicherungsdateien verwenden. Die Anzahl von Datenträgern, die an einen virtuellen Azure-Computer angefügt werden können, ist allerdings je nach [Größe des virtuellen Computers](virtual-machines-linux-sizes.md) begrenzt. Zudem muss der Mehraufwand für die Datenträgerverwaltung berücksichtigt werden.

Ab SQL Server 2014 können Sie Microsoft Azure-Blobspeicher für die Sicherung und Wiederherstellung verwenden. SQL Server 2016 bietet auch Erweiterungen für diese Option. Darüber hinaus bietet SQL Server 2016 für in Microsoft Azure-Blob-Speicher gespeicherte Datenbankdateien eine Option für nahezu sofortige Backups und schnelle Wiederherstellungen mithilfe von Azure-Momentaufnahmen. Dieser Artikel bietet eine Übersicht über diese Optionen. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure Blob Storage-Dienst](https://msdn.microsoft.com/library/jj919148.aspx).

>[AZURE.NOTE] Eine Beschreibung der Optionen zum Sichern sehr großer Datenbanken finden Sie im Blogbeitrag zu den [Sicherungsstrategien für mehrere Terabyte große SQL Server-Datenbanken für virtuelle Azure-Computer](http://blogs.msdn.com/b/igorpag/archive/2015/07/28/multi-terabyte-sql-server-database-backup-strategies-for-azure-virtual-machines.aspx).

Die folgenden Abschnitte enthalten Informationen zu den verschiedenen Versionen von SQL Server, die auf einem virtuellen Azure-Computer unterstützt werden.

## Virtuelle SQL Server-Computer

Wenn die SQL Server-Instanz auf einem virtuellen Azure-Computer ausgeführt wird, befinden sich die Datenbankdateien bereits auf Datenträgern in Azure. Diese Datenträger befinden sich im Azure Blob-Speicher. Die Gründe für das Sichern der Datenbank und der von Ihnen verwendete Ansatz ändern sich also leicht. Stellen Sie sich einmal Folgendes vor:

- Sie müssen keine Datenbanksicherungen mehr durchführen, um Ihre Daten vor Hardwarefehlern oder Fehlern auf Speichermedien zu schützen, da Microsoft Azure diesen Schutz im Rahmen des Microsoft Azure-Diensts bereitstellt.

- Sie müssen jedoch weiterhin Datenbanksicherungen zum Schutz vor Benutzerfehlern oder zu Archivierungszwecken, aus gesetzlichen Gründen oder zu administrativen Zwecken durchführen.

- Sie können die Sicherungsdatei direkt in Azure speichern. Weitere Informationen finden Sie in den folgenden Abschnitten mit Hinweisen für die verschiedenen Versionen von SQL Server.

## SQL Server 2016

Microsoft SQL Server 2016 unterstützt die [Sichern und Wiederherstellen mit Azure-Blobs](https://msdn.microsoft.com/library/jj919148.aspx)-Funktionen aus SQL Server 2014. Die Version enthält aber auch die folgenden Erweiterungen:

| 2016-Erweiterung | Details |
|---------------------|-------------------------------|
| **Striping** | Für Sicherungen in Microsoft Azure-Blob-Speicher unterstützt SQL Server 2016 das Sichern in mehreren Blobs, um die Sicherung großer Datenbanken mit bis zu 12,8 TB zu ermöglichen. |
| **Momentaufnahmesicherung** | Durch die Verwendung von Azure-Momentaufnahmen ermöglicht die Dateimomentaufnahme-Sicherung von SQL Server nahezu sofortige Backups und schnelle Wiederherstellungen für Datenbankdateien, die mit dem Azure-Blob-Speicherdienst gespeichert werden. Sicherungs- und Wiederherstellungsrichtlinien lassen sich dank dieser Funktion vereinfachen. Die Dateimomentaufnahme-Sicherung unterstützt auch Point-in-Time-Wiederherstellungen. Weitere Informationen finden Sie im Artikel zu [Momentaufnahmesicherungen für Datenbankdateien in Azure](https://msdn.microsoft.com/library/mt169363%28v=sql.130%29.aspx). |
| **Verwaltete Sicherungsplanung** | SQL Server Managed Backup für Azure unterstützt jetzt benutzerdefinierte Zeitpläne. Weitere Informationen finden Sie unter [SQL Server Managed Backup für Microsoft Azure](https://msdn.microsoft.com/library/dn449496.aspx). |

Eine praxisnahe Einführung in die Funktionen von SQL Server 2016 für Azure-Blob-Speicher bietet das [Lernprogramm zur Verwendung des Microsoft Azure-Blob-Speicherdiensts mit SQL Server 2016-Datenbanken](https://msdn.microsoft.com/library/dn466438.aspx).

## SQL Server 2014

SQL Server 2014 enthält die folgenden Erweiterungen:

1. **Sichern und Wiederherstellen in Azure**:

 - *SQL Server-URL-Sicherung* wird jetzt in SQL Server Management Studio unterstützt. Die Option zum Sichern in Azure ist jetzt bei der Verwendung der Aufgabe „Sichern oder Wiederherstellen“ oder des Wartungsplanungs-Assistenten in SQL Server Management Studio verfügbar. Weitere Informationen finden Sie unter [SQL Server-URL-Sicherung](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).
 - *SQL Server Managed Backup für Azure* verfügt über neue Funktionen, die eine automatisierte Sicherungsverwaltung ermöglichen. Diese Funktionen erleichtern insbesondere die Automatisierung der Sicherungsverwaltung für SQL Server 2014-Instanzen auf einem Azure-Computer. Weitere Informationen finden Sie unter [SQL Server Managed Backup für Microsoft Azure](https://msdn.microsoft.com/library/dn449496%28v=sql.120%29.aspx).
 - *Automatisierte Sicherung* bietet zusätzliche Automatisierungsfunktionen, mit denen *SQL Server Managed Backup für Azure* automatisch für alle vorhandenen und neuen Datenbanken für einen virtuellen SQL Server-Computer in Azure aktiviert werden kann. Weitere Informationen finden Sie unter [Automatisierte Sicherung für SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md).
 - Eine Übersicht über alle Optionen für die SQL Server 2014-Sicherung in Azure finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure-Blob-Speicherdienst](https://msdn.microsoft.com/library/jj919148%28v=sql.120%29.aspx).

1. **Verschlüsselung**: SQL Server 2014 unterstützt das Verschlüsseln von Daten beim Erstellen einer Sicherung. Es werden mehrere Verschlüsselungsalgorithmen und die Verwendung eines Zertifikats oder asymmetrischen Schlüssels unterstützt. Weitere Informationen finden Sie unter [Sicherungsverschlüsselung](https://msdn.microsoft.com/library/dn449489%28v=sql.120%29.aspx).

## SQL Server 2012

Ausführliche Informationen zur SQL Server-Sicherung und -Wiederherstellung in SQL Server 2012 finden Sie unter [Sichern und Wiederherstellen von SQL Server-Datenbanken (SQL Server 2012)](https://msdn.microsoft.com/library/ms187048%28v=sql.110%29.aspx).

Ab SQL Server 2012 SP1, kumulatives Update 2, ist die Sicherung und Wiederherstellung im bzw. aus dem Azure-Blob-Speicherdienst möglich. Diese Erweiterung kann verwendet werden, um SQL Server-Datenbanken in einer SQL Server-Instanz auf einem virtuellen Azure-Computer oder einer lokalen Instanz zu sichern. Weitere Informationen finden Sie unter [SQL Server-Sicherung und -Wiederherstellung mit dem Microsoft Azure-Blob-Speicherdienst](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Zu den Vorteilen des Azure-Blobspeicherdiensts zählen die Möglichkeit, die Beschränkung auf 16 angefügte Datenträger zu umgehen, die einfache Verwaltung und die direkte Verfügbarkeit der Sicherungsdatei für eine andere SQL Server-Instanz auf einem virtuellen Azure-Computer oder eine lokale Instanz zur Migration oder Notfallwiederherstellung. Eine vollständige Liste der Vorteile, die die Verwendung des Azure-Blob-Speicherdiensts für SQL Server-Sicherungen bietet, finden Sie im Abschnitt *Vorteile* unter [SQL Server-Sicherung und -Wiederherstellung mit dem Azure-Blob-Speicherdienst](https://msdn.microsoft.com/library/jj919148%28v=sql.110%29.aspx).

Bewährte Methoden, Empfehlungen und Informationen zur Problembehandlung finden Sie unter [Bewährte Methoden für die Sicherung und Wiederherstellung (Azure-Blob-Speicherdienst)](https://msdn.microsoft.com/library/jj919149%28v=sql.110%29.aspx).

## SQL Server 2008

Informationen zur SQL Server-Sicherung und -Wiederherstellung in SQL Server 2008 R2 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008 R2)](https://msdn.microsoft.com/library/ms187048%28v=sql.105%29.aspx).

Informationen zur SQL Server-Sicherung und -Wiederherstellung in SQL Server 2008 finden Sie unter [Sichern und Wiederherstellen von Datenbanken in SQL Server (SQL Server 2008)](https://msdn.microsoft.com/library/ms187048%28v=sql.100%29.aspx).

## Nächste Schritte

Wenn Sie an der Planung Ihrer Bereitstellung von SQL Server auf einem virtuellen Azure-Computer arbeiten, finden Sie im folgenden Tutorial Informationen zur Bereitstellung: [Bereitstellen eines virtuellen SQL Server-Computers mit Azure Resource Manager](virtual-machines-windows-portal-sql-server-provision.md).

Obwohl Sie Ihre Daten durch Sicherung und Wiederherstellung migrieren können, sind möglicherweise einfachere Migrationspfade für SQL Server auf einer Azure-VM verfügbar. Eine vollständige Erläuterung der Migrationsoptionen und Empfehlungen finden Sie unter [Migration einer Datenbank zu SQL Server auf einem virtuellen Azure-Computer](virtual-machines-windows-migrate-sql.md).

Lesen Sie auch die weiteren [Ressourcen für die Ausführung von SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-overview.md).

<!---HONumber=AcomDC_0824_2016-->