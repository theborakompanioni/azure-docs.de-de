---
title: 'Azure-Portal: Wiederherstellen einer SQL-Datenbank-Instanz aus einer georedundanten Sicherung | Microsoft Docs'
description: "Wiederherstellen einer Azure SQL-Datenbank-Instanz aus einer georedundanten Sicherung auf einem neuen Server über das Azure-Portal"
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: NA
ms.date: 12/19/2016
ms.author: sstein; carlrab
translationtype: Human Translation
ms.sourcegitcommit: 120075710d466a10233aadfc1f7e29da871f8f83
ms.openlocfilehash: 7d9314444c39cda3f9d893e4f97b4afce4c75777
ms.lasthandoff: 02/16/2017


---
# <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-with-the-azure-portal"></a>Wiederherstellen einer Azure SQL-Datenbank-Instanz aus einer georedundanten Sicherung über das Azure-Portal

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank mit der Geowiederherstellung über das Azure-Portal auf einem neuen Server wiederherstellen können. Diese Aufgabe kann auch [mithilfe von PowerShell](sql-database-geo-restore-powershell.md) ausgeführt werden.

## <a name="restore-an-azure-sql-database-from-a-geo-redundant-backup-by-using-the-azure-portal"></a>Wiederherstellen einer Azure SQL-Datenbank aus einer georedundanten Sicherung über das Azure-Portal

Führen Sie zur Geowiederherstellung einer Datenbank im Azure-Portal die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **+Neu** > **Datenbanken** > **SQL-Datenbank** aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/new-sql-database.png)
3. Wählen Sie als Quelle die Option **Sicherung**, und wählen Sie anschließend die Sicherung aus, die wiederhergestellt werden soll. Geben Sie einen Datenbanknamen sowie einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf **Erstellen**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

4. Überwachen Sie den Status des Wiederherstellungsvorgangs, indem Sie auf das Benachrichtigungssymbol oben rechts auf der Seite klicken.


## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md).
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md).
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md).
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md).  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md).


