<properties
   pageTitle="Geschäftskontinuität in der Cloud – Wiederherstellen einer gelöschten Datenbank – SQL-Datenbank | Microsoft Azure"
   description="Hier finden Sie Informationen zur Point-in-Time-Wiederherstellung, mit der Sie ein Rollback für eine Azure SQL-Datenbank durchführen können (bis zu 35 Tage)."
   services="sql-database"
   documentationCenter=""
   authors="stevestein"
   manager="jhubbard"
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="06/09/2016"
   ms.author="sstein"/>

# Übersicht: Wiederherstellen einer gelöschten Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [Übersicht](sql-database-restore-deleted-database.md)
- [Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [PowerShell](sql-database-restore-deleted-database-powershell.md)

Eine gelöschte Datenbank kann innerhalb des Aufbewahrungszeitraums für die [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) wiederhergestellt werden. Sie können dazu entweder das [Azure-Portal](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden.

Wenn Sie eine Datenbank löschen, wird die letzte Sicherung für den normalen Aufbewahrungszeitraum aufbewahrt, sodass Sie die Datenbank in dem Zustand wiederherstellen können, der zum Zeitpunkt der Löschung vorlag.

## Wiederherstellen einer kürzlich gelöschten Datenbank

Bei gelöschten Datenbanken ist als Wiederherstellungszeitpunkt der Zeitpunkt der Datenbanklöschung vorgegeben. Beim Wiederherstellen einer gelöschten Datenbank gilt: Die Wiederherstellung kann nur auf dem Server erfolgen, auf dem sich auch die ursprüngliche Datenbank befand. Berücksichtigen Sie diesen Punkt, wenn Sie einen Server löschen, da Datenbanken, die sich auf diesem Server befunden haben, nach dem Löschen des Servers nicht wiederhergestellt werden können.

> [AZURE.IMPORTANT] Wenn Sie eine Azure SQL-Datenbankserverinstanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden.

## Zusammenfassung

Automatische Sicherungen schützen Ihre Datenbanken vor versehentlichem Löschen. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung.

## Nächste Schritte

- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md)
- [Restore a deleted database using the REST API](https://msdn.microsoft.com/library/azure/mt163685.aspx) (Wiederherstellen einer gelöschten Datenbank mithilfe der REST-API)
- [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)

## Weitere Ressourcen

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0615_2016-->