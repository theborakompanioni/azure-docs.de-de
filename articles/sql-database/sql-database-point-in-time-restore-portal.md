---
title: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt (Azure-Portal) | Microsoft Docs"
description: "Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: d1822905-a11f-4c42-8940-98c6b81aed20
ms.service: sql-database
ms.custom: business continuity; how to
ms.devlang: NA
ms.date: 10/18/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 6feaea525ae1fceff5acdc95fefa115939d5b1da


---
# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt über das Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md)
> * [Point-in-Time-Wiederherstellung: mit PowerShell](sql-database-point-in-time-restore-powershell.md)
> 
> 

In diesem Artikel erfahren Sie, wie Sie auf der [Grundlage automatisierter SQL-Datenbanksicherungen](sql-database-automated-backups.md) über das Azure-Portal einen früheren Zustand Ihrer Datenbank wiederherstellen.

## <a name="restore-a-sql-database-to-a-previous-point-in-time"></a>Restore a SQL database to a previous point in time (Wiederherstellen eines früheren Zustands einer SQL-Datenbank)
Auswählen einer Datenbank für die Wiederherstellung im Azure-Portal:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **Weitere Dienste** > **SQL-Datenbanken**.
3. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
4. Wählen Sie oben auf der Seite der Datenbank **Wiederherstellen**aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)
5. Wählen Sie auf der Seite **Wiederherstellen** das Datum und die Uhrzeit (im UTC-Zeitformat) für die Wiederherstellung der Datenbank, und klicken Sie dann auf **OK**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)

## <a name="monitor-the-restore-operation"></a>Überwachen des Wiederherstellungsvorgangs
1. Nachdem Sie im vorherigen Schritt auf **OK** geklickt haben, klicken auf der Seite oben rechts auf das Benachrichtigungssymbol und dann auf die Benachrichtigung **SQL-Datenbank wird wiederhergestellt**, um Details anzuzeigen.
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/notification-icon.png)
2. Die Seite „SQL-Datenbank wird wiederhergestellt“ mit Informationen zum Status der Wiederherstellung wird geöffnet. Sie können auf die Position klicken, um weitere Details anzuzeigen:
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/inprogress.png)

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)




<!--HONumber=Nov16_HO3-->


