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
   ms.workload="sqldb-bcdr"
   ms.date="06/09/2016"
   ms.author="sstein"/>

# Übersicht: Wiederherstellen einer gelöschten Azure SQL-Datenbank

> [AZURE.SELECTOR]
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Szenarios für die Geschäftskontinuität](sql-database-business-continuity-scenarios.md)


Eine gelöschte Datenbank kann innerhalb des Aufbewahrungszeitraums für die [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) für Ihre [Dienstebene](sql-database-service-tiers.md) wiederhergestellt werden. Sie können dazu entweder das [Azure-Portal](sql-database-restore-deleted-database-portal.md), [PowerShell](sql-database-restore-deleted-database-powershell.md) oder die [REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx) verwenden.

> [AZURE.SELECTOR]
- [Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [PowerShell](sql-database-restore-deleted-database-powershell.md)

## Wiederherstellen einer kürzlich gelöschten Datenbank

Sie können die gelöschte Datenbank mit dem gleichen oder einem anderen Datenbanknamen auf dem logischen Server wiederherstellen, auf dem die ursprüngliche Datenbank enthalten war. Bei gelöschten Datenbanken ist als Wiederherstellungszeitpunkt der Zeitpunkt der Datenbanklöschung vorgegeben.

> [AZURE.IMPORTANT] Wenn Sie eine Azure SQL-Datenbankserverinstanz löschen, werden auch alle dazugehörigen Datenbanken gelöscht und können nicht wiederhergestellt werden.

## Wiederherstellungsdauer

Die Zeit, die zum Wiederherstellen einer Datenbank benötigt wird, hängt von zahlreichen Faktoren ab. Hierzu zählen unter anderem die Größe der Datenbank, die Anzahl der Transaktionsprotokolle, der ausgewählte Zeitpunkt und die Menge an Aktivitäten, die ausgeführt werden müssen, um den Zustand des ausgewählten Zeitpunkts wiederherzustellen. Bei einer sehr großen und/oder sehr aktiven Datenbank kann die Wiederherstellung mehrere Stunden dauern. Im Zuge der Wiederherstellung einer Datenbank wird auf dem Server mit der ursprünglichen Datenbank immer eine neue Datenbank erstellt. Die wiederhergestellte Datenbank muss daher einen neuen Namen erhalten. Der Großteil der Datenbankwiederherstellungen erfolgt innerhalb von 12 Stunden.

## Zusammenfassung

Automatische Sicherungen schützen Ihre Datenbanken vor versehentlichem Löschen. Diese kostenlose, verwaltungsfreie Lösung steht für alle SQL-Datenbanken zur Verfügung.

## Nächste Schritte

- Ausführliche Schritte zum Wiederherstellen einer gelöschten Datenbank mithilfe des Azure-Portals finden Sie unter [Wiederherstellen einer gelöschten Azure SQL-Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md).
- Ausführliche Schritte zum Wiederherstellen einer gelöschten Datenbank mithilfe von PowerShell finden Sie unter [Wiederherstellen einer gelöschten Azure SQL-Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md).
- Informationen zum Wiederherstellen einer gelöschten Datenbank finden Sie unter [Wiederherstellen einer gelöschten Datenbank mithilfe der REST-API](https://msdn.microsoft.com/library/azure/mt163685.aspx).
- Detaillierte Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).

## Zusätzliche Ressourcen

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)

<!---HONumber=AcomDC_0622_2016-->