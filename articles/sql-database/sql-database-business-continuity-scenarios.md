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
   ms.workload="sqldb-bcdr"
	ms.topic="article"
	ms.tgt_pltfrm="NA"/>



# Geschäftskontinuitätsszenarien für Azure SQL-Datenbank

In diesem Artikel werden verschiedene Szenarien für die Notfallwiederherstellung und den Anwendungsentwurf im Zusammenhang mit der Geschäftskontinuität vorgestellt.

## Wiederherstellen nach einem Ausfall

Wenn ein Ausfall aufgetreten ist, lesen Sie im Artikel [Wiederherstellen einer Azure SQL-Datenbank nach einem Ausfall](sql-database-disaster-recovery.md), wie Sie mithilfe einer der beiden folgenden Geschäftskontinuitätslösungen eine Wiederherstellung nach einem Ausfall durchführen:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geowiederherstellung](sql-database-recovery-using.backups.md#geo-restore)

Die einzelnen Schritte und die zum Wiederherstellen benötigte Zeit hängen von der ausgewählten Geschäftskontinuitätslösung ab und variieren entsprechend. Unabhängig von der Geschäftskontinuitätslösung müssen Sie jedoch folgende Dinge wissen: wann die Wiederherstellung ausgelöst werden soll, welche Schritte zum Wiederherstellen der Datenbank für die jeweilige Geschäftskontinuitätslösung nötig sind, wie Sie Ihre Datenbank nach der Wiederherstellung konfigurieren müssen und wie Sie Ihre Anwendung nach der Wiederherstellung konfigurieren müssen, um die Wiederherstellung nach dem Ausfall abzuschließen.

## Wiederherstellen nach einem Benutzerfehler

Wenn Benutzer einen Fehler gemacht haben oder auf andere Weise unbeabsichtigt Daten geändert wurden, lesen Sie im Artikel [Wiederherstellen einer Azure SQL-Datenbank nach einem Benutzerfehler](sql-database-user-error-recovery.md), wie Sie mithilfe einer der beiden folgenden Geschäftskontinuitätslösungen eine Wiederherstellung nach einem Benutzerfehler durchführen:

- [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore)
- [Wiederherstellen einer gelöschten Datenbank](sql-database-recovery-using-backups.md#deleted-database-restore)

Die einzelnen Schritte und die zum Wiederherstellen benötigte Zeit hängen von der ausgewählten Geschäftskontinuitätslösung ab und variieren entsprechend. Sie müssen auf jeden Fall wissen, wie Sie bei der verwendeten Geschäftskontinuitätslösung eine Wiederherstellung nach einem Benutzerfehler ausführen.

## Ausführen von Notfallwiederherstellungsübungen als Vorbereitung auf einen Ausfall

Damit Ihre Geschäftskontinuitätslösung effektiv funktioniert, wird empfohlen, in regelmäßigen Abständen die Anwendungsbereitschaft für den Wiederherstellungsworkflow zu überprüfen. Überprüfungen des Anwendungsverhaltens und der Auswirkungen von Datenverlusten und/oder Unterbrechungen durch Failover gehören zu einem angemessenen Entwicklungsverfahren. Solche Überprüfungen werden auch für die meisten Branchenstandards im Zuge von Zertifizierungen zur Geschäftskontinuität vorausgesetzt.

Notfallwiederherstellungsübungen umfassen Folgendes:

- Simulieren von Ausfällen auf Datenebene
- Wiederherstellen
- Überprüfen der Anwendungsintegrität nach der Wiederherstellung

Unter [Ausführen von Notfallwiederherstellungsverfahren](sql-database-disaster-recovery-drills.md) wird beschrieben, wie Sie Notfallwiederherstellungsübungen mithilfe der folgenden Geschäftskontinuitätslösungen durchführen können:

- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Geowiederherstellung](sql-database-recovery-using-backups.md#geo-restore)

## Verwalten von parallelen Upgrades von Cloudanwendungen mithilfe der aktiven Georeplikation

Beim Aktualisieren einer Cloudanwendung mit einer SQL-Datenbank wird der normale Anwendungsbetrieb unterbrochen. Deshalb müssen Sie dieses Szenario in Ihre Planung für die Geschäftskontinuität aufnehmen und durch ein entsprechendes Design vorsorgen. Im Artikel zum [Verwalten von Anwendungsupgrades](sql-database-manage-application-rolling-upgrade.md) erfahren Sie, wie Sie die [Georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank verwenden, um parallele Upgrades Ihrer Cloudanwendung zu ermöglichen. In dem Artikel werden zwei unterschiedliche Methoden zum Orchestrieren des Upgradevorgangs betrachtet und die Vor- und Nachteile jeder Option erörtert.

## Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung mithilfe der aktiven Georeplikation

Unter [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md) wird beschrieben, wie Sie mit der [aktiven Georeplikation](sql-database-geo-replication-overview.md) in SQL-Datenbank Datenbankanwendungen entwerfen können, die vor regionalen Fehlern und schwerwiegenden Ausfällen geschützt sind. In diesem Artikel werden die Anwendungsbereitstellungstopologie, die vorgesehene Vereinbarung zum Servicelevel, die Latenz des Datenverkehrs und die Kosten betrachtet. Anschließend werden allgemeine Anwendungsmuster behandelt, einschließlich der jeweiligen Vor- und Nachteile.

## Strategien für die Notfallwiederherstellung für Anwendungen mit Pools für elastische Datenbanken

Unter [Strategien für die Notfallwiederherstellung für Anwendungen mit elastischem SQL-Datenbankpool](sql-database-disaster-recovery-strategies-for-applications-with-elastic-pool.md) werden Strategien für die Notfallwiederherstellung mithilfe von [Pools für elastische Datenbanken](sql-database-elastic-pool.md) beschrieben.

## Nächste Schritte

- Eine Übersicht zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
- Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
- Informationen über Entwurfs- und Wiederherstellungsszenarien für die Geschäftskontinuität finden Sie unter [Geschäftskontinuitätsszenarien](sql-database-business-continuity-scenarios.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
- Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0629_2016-->