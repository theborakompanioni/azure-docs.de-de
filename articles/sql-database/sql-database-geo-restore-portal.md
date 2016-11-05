---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal) | Microsoft Docs
description: Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung (Azure-Portal)
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
# Geowiederherstellung einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md)
> * [Geowiederherstellung: mit PowerShell](sql-database-geo-restore-powershell.md)
> 
> 

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung über das Azure-Portal auf einem neuen Server wiederherstellen können.

## Auswählen der wiederherzustellenden Datenbank
Gehen Sie zum Wiederherstellen einer Datenbank im Azure-Portal wie folgt vor:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **Neu** > **Daten und Speicher** > **SQL-Datenbank** aus.
3. Wählen Sie als Quelle die Option **Sicherung** und anschließend die georedundante Sicherung aus, die zur Wiederherstellung verwendet werden soll.
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)
4. Geben Sie einen Datenbanknamen und einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf „Erstellen“.

## Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).

<!---HONumber=AcomDC_0727_2016-->