---
title: 'Azure-Portal: Wiederherstellen einer Azure SQL-Datenbank | Microsoft-Dokumentation'
description: Wiederherstellen einer Azure SQL-Datenbank (Azure-Portal).
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 33b0c9e6-1cd2-4fd9-9b0d-70ecf6e54821
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.date: 10/12/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 49800be1a61e45f64a77f8d2662ca9edeae29de9
ms.lasthandoff: 03/10/2017


---
# <a name="restore-an-azure-sql-database-using-the-azure-portal"></a>Wiederherstellen einer Azure SQL-Datenbank mithilfe des Azure-Portals

Die folgenden Schritte zeigen die Point-in-Time-Wiederherstellung einer Azure SQL-Datenbank sowie deren Wiederherstellung auf Basis einer gelöschten Datenbank und einer georedundanten Sicherung.

## <a name="restore-an-azure-sql-database-to-a-previous-point-in-time"></a>Wiederherstellen des Zustands einer Azure SQL-Datenbank zu einem früheren Zeitpunkt 

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery-portal.md).
>

Auswählen einer Datenbank für die Wiederherstellung im Azure-Portal:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **Weitere Dienste** > **SQL-Datenbanken**.
3. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
4. Wählen Sie oben auf der Seite der Datenbank **Wiederherstellen**aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore.png)
5. Wählen Sie auf der Seite **Wiederherstellen** das Datum und die Uhrzeit (im UTC-Zeitformat) für die Wiederherstellung der Datenbank, und klicken Sie dann auf **OK**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/restore-details.png)

6. Nachdem Sie im vorherigen Schritt auf **OK** geklickt haben, klicken auf der Seite oben rechts auf das Benachrichtigungssymbol und dann auf die Benachrichtigung **SQL-Datenbank wird wiederhergestellt**, um Details anzuzeigen.
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/notification-icon.png)
7. Die Seite „SQL-Datenbank wird wiederhergestellt“ mit Informationen zum Status der Wiederherstellung wird geöffnet. Sie können auf die Position klicken, um weitere Details anzuzeigen:
   
    ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-point-in-time-restore-portal/inprogress.png)


## <a name="restore-a-deleted-azure-sql-database-from-backups"></a>Wiederherstellen einer gelöschten Azure SQL-Datenbank auf der Basis von Sicherungen
So stellen Sie eine gelöschte Datenbank im Azure-Portal wieder her:

1. Klicken Sie im [Azure-Portal](https://portal.azure.com)auf **Weitere Dienste** > **SQL-Server**.
2. Wählen Sie den Server aus, der die Datenbank enthält, die Sie wiederherstellen möchten.
3. Scrollen Sie auf dem Serverblatt nach unten zum Bereich **Vorgänge**, und wählen Sie **Gelöschte Datenbanken** aus: ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted-trashbin.png)
4. Wählen Sie die Datenbank aus, die Sie wiederherstellen möchten.
5. Geben Sie einen Datenbanknamen an, und klicken Sie auf **OK**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-restore-deleted-database-portal/restore-deleted.png)

## <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup"></a>Wiederherstellen einer Azure SQL-Datenbank auf der Basis einer georedundanten Sicherung

Führen Sie zur Geowiederherstellung einer Datenbank im Azure-Portal die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **+Neu** > **Datenbanken** > **SQL-Datenbank** aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/new-sql-database.png)
3. Wählen Sie als Quelle die Option **Sicherung**, und wählen Sie anschließend die Sicherung aus, die wiederhergestellt werden soll. Geben Sie einen Datenbanknamen sowie einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf **Erstellen**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

4. Überwachen Sie den Status des Wiederherstellungsvorgangs, indem Sie auf das Benachrichtigungssymbol oben rechts auf der Seite klicken.

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)


