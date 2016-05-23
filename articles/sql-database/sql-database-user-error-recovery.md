<properties 
   pageTitle="Wiederherstellung der SQL-Datenbank nach Benutzerfehler" 
   description="Erfahren Sie, wie Sie die Datenbankwiederherstellung nach einem Benutzerfehler, unbeabsichtigter Datenbeschädigungen oder bei einer gelöschten Datenbank mithilfe der Zeitpunktwiederherstellungsfunktion (PITR) von Azure SQL-Datenbank durchführen." 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="05/10/2016"
   ms.author="elfish"/>

# Wiederherstellen einer Azure-SQL-Datenbank nach einem Benutzerfehler

Eine Azure SQL-Datenbank bietet zwei Kernfunktionen zur Wiederherstellung nach einem Benutzerfehlern oder unbeabsichtigter Datenänderung.

- [Zeitpunktwiederherstellung](sql-database-point-in-time-restore.md) 
- [Wiederherstellen einer gelöschten Datenbank](sql-database-point-in-time-restore.md#restoring-a-recently-deleted-database)


Eine Azure-SQL-Datenbank wird immer in eine neue Datenbank wiederhergestellt. Diese Wiederherstellungsfunktionen sind für alle Basic-, Standard- und Premium-Datenbanken verfügbar.

##Point-in-Time-Wiederherstellung

Bei einem Benutzerfehler oder unbeabsichtigter Datenänderung kann die Zeitpunktwiederherstellung verwendet werden, um die Datenbank bis zu einem beliebigen Zeitpunkt innerhalb der Datenbankaufbewahrungsdauer wiederherzustellen.

Basic-Datenbanken haben eine Aufbewahrungsdauer von 7 Tagen, Standard-Datenbanken eine Aufbewahrungsdauer von 14 Tagen, und Premium-Datenbanken eine Aufbewahrungsdauer von 35 Tagen. Weitere Informationen zur Datenbankaufbewahrungsdauer finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).

Durchführen einer Point-in-Time-Wiederherstellung:

- [Point-in-Time-Wiederherstellung über das Azure-Portal](sql-database-point-in-time-restore-portal.md)
- [Point-in-Time-Wiederherstellung mit PowerShell](sql-database-point-in-time-restore-powershell.md)
- [Point-in-Time-Wiederherstellung mit der REST-API (createmode=PointInTimeRestore)](https://msdn.microsoft.com/library/azure/mt163685.aspx) 


## Wiederherstellen einer gelöschten Datenbank

Falls eine Datenbank gelöscht wird, bietet Ihnen die Azure-SQL-Datenbank die Möglichkeit, die gelöschte Datenbank zum Zeitpunkt des Löschens wiederherzustellen. Die Azure-SQL-Datenbank speichert die gelöschte Datenbanksicherung während der Aufbewahrungsdauer der Datenbank.

Die Aufbewahrungsdauer einer gelöschten Datenbank wird anhand der Dienstebene der Datenbank bestimmt, während sie vorhanden war, oder der Anzahl der Tage, während denen die Datenbank vorhanden ist, je nachdem, welcher Wert kleiner ist. Weitere Informationen zur Datenbankaufbewahrungsdauer finden Sie unter [Geschäftskontinuität – Überblick](sql-database-business-continuity.md).

So stellen Sie eine gelöschte Datenbank wieder her

- [Wiederherstellen einer gelöschten Datenbank im Azure-Portal](sql-database-restore-deleted-database-portal.md)
- [Wiederherstellen einer gelöschten Datenbank mit PowerShell](sql-database-restore-deleted-database-powershell.md)
- [Wiederherstellen einer gelöschten Datenbank mit der REST-API (createmode=Restore)](https://msdn.microsoft.com/library/azure/mt163685.aspx)


## Zusätzliche Ressourcen

- [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
- [SQL-Datenbankdokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!---HONumber=AcomDC_0511_2016-->