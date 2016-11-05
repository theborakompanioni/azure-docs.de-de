---
title: Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt (Azure-Portal) | Microsoft Docs
description: Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt
services: sql-database
documentationcenter: ''
author: stevestein
manager: jhubbard
editor: ''

ms.service: sql-database
ms.devlang: NA
ms.date: 07/17/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA

---
# Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md)
> * [Point-in-Time-Wiederherstellung: mit PowerShell](sql-database-point-in-time-restore-powershell.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie auf der [Grundlage automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) über das Azure-Portal einen früheren Zustand Ihrer Datenbank wiederherstellen.

## Auswählen einer Datenbank für die Wiederherstellung des Zustands zu einem früheren Zeitpunkt
Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **DURCHSUCHEN** > **SQL-Datenbanken** aus.
3. Navigieren Sie zu der wiederherzustellenden Datenbank, und wählen Sie sie aus.
4. Wählen Sie oben auf dem Blatt der Datenbank **Wiederherstellen** aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)
5. Geben Sie einen Datenbanknamen und einen Zeitpunkt an, und klicken Sie dann auf „OK“:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0727_2016-->