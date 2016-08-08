<properties
	pageTitle="Verwalten von Azure SQL-Datenbanken über das Azure-Portal"
	description="Erfahren Sie, wie Sie das Azure-Portal verwenden, um eine relationale Datenbank mithilfe des Azure-Portals in der Cloud zu verwalten."
	services="sql-database"
	documentationCenter=""
	authors="stevestein"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"
	ms.date="05/20/2016"
	ms.author="sstein"/>


# Verwalten von Azure SQL-Datenbanken über das Azure-Portal


> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-manage-portal.md)
- [SSMS](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

Das [Azure-Portal](https://portal.azure.com/) ermöglicht das Erstellen, Überwachen und Verwalten von Azure SQL-Datenbanken und logischen Servern. In diesem Artikel werden einige der häufigen Aufgaben beschrieben.

![Datenbankübersicht](./media/sql-database-manage-portal/sqldatabase_annotated.png)

## 1\. Aktionen zur Datenbankverwaltung

![Datenbankverwaltungsaktionen](./media/sql-database-manage-portal/sqldatabase_actions.png)

Das Azure-Portal bietet eine Reihe von allgemeinen Datenbankaktionen, auf die oben auf dem Blatt „Datenbank“ zugegriffen werden kann. Sie können eine Datenbank zu einem früheren Zeitpunkt wiederherstellen, eine Datenbank in Visual Studio öffnen, eine Datenbank auf einen neuen Server kopieren und die Datenbank in ein Azure-Speicherkonto exportieren.

- [Wiederherstellen einer SQL-­Datenbank](sql-database-recovery-using-backups.md)
- [Öffnen einer SQL-Datenbank in Visual Studio](sql-database-connect-query.md)
- [Exportieren einer SQL-­Datenbank](sql-database-export.md)

## 2\. Datenbanküberwachung

![Datenbanküberwachung](./media/sql-database-manage-portal/sqldatabase_monitoring.png)

Azure SQL-Datenbanken verfügen standardmäßig über Überwachungsdiagramme für die Datenbanktransaktionseinheit (DTU), die Größe der Datenbank und die Verbindungsintegrität. Diese Überwachungsdiagramme können angepasst und erweitert werden, um außerdem die CPU-Nutzung, den Daten-E/A-Prozentsatz, Deadlocks, den Protokoll-E/A-Prozentsatz oder sogar den Prozentsatz der Anforderungen anzuzeigen, die von der Firewall blockiert werden.

Darüber hinaus können Regeln für Warnhinweise eingerichtet werden, um eine angegebene Metrik zu überwachen und einen bestimmten Administrator und Co-Administrator zu warnen, wenn vordefinierte Grenzwerte erreicht werden.

## 3\. Datenbanksicherheit und Überwachung

![Datenbanksicherheit](./media/sql-database-manage-portal/sqldatabase_security.png)

Azure SQL-Datenbanken können so konfiguriert werden, dass alle Datenbankereignisse verfolgt werden und in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto geschrieben werden. Diese Funktion kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

- [SQL-Datenbanküberwachung](sql-database-auditing-get-started.md)

Azure SQL-Datenbanken können auch zum Maskieren vertraulicher Daten für nicht berechtigte Benutzer konfiguriert werden.

- [Dynamische Datenmaskierung](sql-database-dynamic-data-masking-get-started.md)


## 4\. Georeplikation

![Georeplikation](./media/sql-database-manage-portal/sqldatabase_georeplication.png)

Azure SQL-Datenbanken können konfiguriert werden, um bereits übergebene Transaktionen asynchron in eine sekundäre Datenbank zu replizieren. Mit der Georeplikation im Portal können Sie die Azure-Region wählen, in der die sekundäre Datenbank gespeichert werden soll.

- [Georeplikation](sql-database-geo-replication-overview.md)



## Zusätzliche Ressourcen

- [SQL-Datenbank](sql-database-technical-overview.md)

<!---HONumber=AcomDC_0727_2016-->