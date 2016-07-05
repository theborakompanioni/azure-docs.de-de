<properties
	pageTitle="Geschäftskontinuitätsszenarien für Azure SQL-Datenbank | Microsoft Azure"
	description="Geschäftskontinuitätsszenarien für Azure SQL-Datenbank"
	services="sql-database"
	documentationCenter=""
	authors="carlrabeler"
	manager="jhubbard"
	editor=""/>

<tags
	ms.service="sql-database"
	ms.devlang="NA"
	ms.date="06/16/2016"
	ms.author="carlrab"
	ms.workload="data-management"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>



# Geschäftskontinuitätsszenarien für Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [Geschäftskontinuität](sql-database-business-continuity.md)
- [Szenarien](sql-database-business-continuity-scenarios.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-retore.md)
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication)

In diesem Artikel lernen Sie verschiedene Geschäftskontinuitätsszenarien für Azure SQL-Datenbank kennen.

## Wiederherstellen nach einem Ausfall

Unter [Wiederherstellen einer Azure SQL-Datenbank oder Failover auf eine sekundäre Datenbank](sql-database-disaster-recovery.md) wird beschrieben, wie Sie die Wiederherstellung nach einem Ausfall mithilfe der folgenden Funktionen durchführen können:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)

In diesem Artikel wird erläutert, wann die Wiederherstellung gestartet werden muss, wie die Wiederherstellung mit den einzelnen Funktionen durchgeführt wird, wie die Datenbank nach der Wiederherstellung konfiguriert wird und wie die Anwendung nach der Wiederherstellung konfiguriert wird.

## Wiederherstellen nach einem Benutzerfehler

Unter [Wiederherstellen einer Azure SQL-Datenbank nach einem Benutzerfehler](sql-database-user-error-recovery.md) wird beschrieben, wie Sie die Wiederherstellung nach Benutzerfehlern oder unbeabsichtigten Datenänderungen mithilfe der folgenden Funktionen durchführen können:

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md) 
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)

## Ausführen von Notfallwiederherstellungsverfahren

Unter [Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md) wird beschrieben, wie Sie Notfallwiederherstellungsverfahren mithilfe der folgenden Funktionen durchführen können:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)

Es wird empfohlen, in regelmäßigen Abständen Überprüfungen der Anwendungsbereitschaft für den Wiederherstellungsworkflow durchzuführen. Überprüfungen des Anwendungsverhaltens und der Auswirkungen von Datenverlusten und/oder Unterbrechungen durch Failover gehören zu einem angemessenen Entwicklungsverfahren. Solche Überprüfungen werden auch für die meisten Branchenstandards im Zuge von Zertifizierungen zur Geschäftskontinuität vorausgesetzt.

Notfallwiederherstellungsverfahren umfassen Folgendes:

- Simulieren von Ausfällen auf Datenebene
- Wiederherstellen 
- Überprüfen der Anwendungsintegrität nach der Wiederherstellung

## Verwalten der Sicherheit nach der Notfallwiederherstellung

Unter [Verwalten der Sicherheit nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md) werden die Authentifizierungsanforderungen zum Konfigurieren und Steuern der [aktiven Georeplikation](sql-database-geo-replication-overview.md) sowie die erforderlichen Schritte zum Einrichten des Benutzerzugriffs auf die sekundäre Datenbank beschrieben. Außerdem wird beschrieben, wie nach der [Geowiederherstellung](sql-database-geo-restore.md) der Zugriff auf die wiederhergestellte Datenbank aktiviert wird.

## Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation

Unter [Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation von SQL-Datenbank](sql-database-manage-application-rolling-upgrade.md) wird beschrieben, wie Sie mit der [Georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank parallele Upgrades Ihrer Cloudanwendung ermöglichen. Da ein Upgrade einen unterbrechenden Vorgang darstellt, sollte es Teil der Planung und des Designs Ihrer Geschäftskontinuität sein. In diesem Artikel werden zwei unterschiedliche Methoden zum Orchestrieren des Upgradevorgangs betrachtet und die Vor- und Nachteile jeder Option erörtert.

## Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation

Unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation in SQL-Datenbank](sql-database-designing-cloud-solutions-for-disaster-recovery.md) wird beschrieben, wie Sie mit der [aktiven Georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank Datenbankanwendungen entwerfen können, die vor regionalen Fehlern und schwerwiegenden Ausfällen geschützt sind. In diesem Artikel werden die Anwendungsbereitstellungstopologie, die vorgesehene Vereinbarung zum Servicelevel, die Latenz des Datenverkehrs und die Kosten betrachtet. Anschließend werden allgemeine Anwendungsmuster behandelt, einschließlich der jeweiligen Vor- und Nachteile.

## Strategien für die Notfallwiederherstellung für Anwendungen mit Pools für elastische Datenbanken

Unter [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) werden Strategien für die Notfallwiederherstellung mithilfe von [Pools für elastische Datenbanken](sql-database-elastic-pool.md) beschrieben.

## Nächste Schritte

- Informationen zur Verwendung und Konfiguration der aktiven Georeplikation für die Notfallwiederherstellung finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zur Verwendung der Geowiederherstellung für die Notfallwiederherstellung finden Sie unter [Geowiederherstellung](sql-database-geo-restore.md).

## Weitere Ressourcen

- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0622_2016-->