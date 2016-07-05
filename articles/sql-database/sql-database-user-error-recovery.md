<properties 
   pageTitle="Wiederherstellung der SQL-Datenbank nach einem Benutzerfehler | Microsoft Azure" 
   description="Erfahren Sie, wie Sie die Datenbankwiederherstellung nach einem Benutzerfehler, unbeabsichtigter Datenbeschädigungen oder bei einer gelöschten Datenbank mithilfe der Zeitpunktwiederherstellungsfunktion (PITR) von Azure SQL-Datenbank durchführen." 
   services="sql-database" 
   documentationCenter="" 
   authors="carlrabeler" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="06/16/2016"
   ms.author="carlrab"/>

# Wiederherstellen einer Azure-SQL-Datenbank nach einem Benutzerfehler

Eine Azure SQL-Datenbank bietet zwei Kernfunktionen zur Wiederherstellung nach einem Benutzerfehlern oder unbeabsichtigter Datenänderung.

- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md) 
- [Wiederherstellen einer gelöschten Datenbank](sql-database-restore-deleted-database.md)

Eine Azure-SQL-Datenbank wird immer in eine neue Datenbank wiederhergestellt. Diese Wiederherstellungsfunktionen sind für alle Basic-, Standard- und Premium-Datenbanken verfügbar.

##Point-in-Time-Wiederherstellung

Bei einem Benutzerfehler oder einer unbeabsichtigten Datenänderung kann die Point-in-Time-Wiederherstellung verwendet werden, um den Zustand der Datenbank zu einem beliebigen Zeitpunkt innerhalb der Datenbankaufbewahrungsdauer wiederherzustellen.

Basic-Datenbanken haben eine Aufbewahrungsdauer von 7 Tagen, Standard-Datenbanken eine Aufbewahrungsdauer von 14 Tagen, und Premium-Datenbanken eine Aufbewahrungsdauer von 35 Tagen. Weitere Informationen zur Aufbewahrungsdauer von Datenbanksicherungen finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).

Informationen zum Durchführen einer Point-in-Time-Wiederherstellung finden Sie unter:

- [Point-in-Time-Wiederherstellung über das Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [Point-in-Time-Wiederherstellung mit PowerShell](sql-database-point-in-time-restore-powershell.md)
- [Point-in-Time-Wiederherstellung mit der REST-API (createmode=PointInTimeRestore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 


## Wiederherstellen einer gelöschten Datenbank

Falls eine Datenbank gelöscht wird, bietet Ihnen die Azure-SQL-Datenbank die Möglichkeit, die gelöschte Datenbank zum Zeitpunkt des Löschens wiederherzustellen. Die Azure-SQL-Datenbank speichert die gelöschte Datenbanksicherung während der Aufbewahrungsdauer der Datenbank.

Die Aufbewahrungsdauer einer gelöschten Datenbank wird anhand der Dienstebene der Datenbank bestimmt, während sie vorhanden war, oder der Anzahl der Tage, während denen die Datenbank vorhanden ist, je nachdem, welcher Wert kleiner ist. Weitere Informationen zur Datenbankaufbewahrungsdauer finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).

So stellen Sie eine gelöschte Datenbank wieder her

- [Wiederherstellen einer gelöschten Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [Wiederherstellen einer gelöschten Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md)
- [Wiederherstellen einer gelöschten Datenbank mit der REST-API (createmode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)


## Nächste Schritte

- Informationen zur Verwendung und Konfiguration der aktiven Georeplikation für die Notfallwiederherstellung finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
- Informationen zur Verwendung der Geowiederherstellung für die Notfallwiederherstellung finden Sie unter [Geowiederherstellung](sql-database-geo-restore.md).

## Zusätzliche Ressourcen

- [Geschäftskontinuität und Notfallwiederherstellung mit SQL-Datenbank](sql-database-business-continuity.md)
- [Point-in-Time-Wiederherstellung](sql-database-point-in-time-restore.md)
- [Geografische Wiederherstellung](sql-database-geo-restore.md)
- [Aktive Georeplikation](sql-database-geo-replication-overview.md)
- [Entwerfen einer Anwendung für die cloudbasierte Notfallwiederherstellung](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [Abschließen der wiederhergestellten Azure SQL-Datenbank](sql-database-recovered-finalize.md)
- [Sicherheitskonfiguration für die Georeplikation](sql-database-geo-replication-security-config.md)
- [BCDR in SQL-Datenbank – Häufig gestellte Fragen](sql-database-bcdr-faq.md)

<!---HONumber=AcomDC_0622_2016-->