---
title: Wiederherstellen einer Azure SQL-Datenbank aus einer automatischen Sicherung (Azure-Portal) | Microsoft Docs
description: Wiederherstellen einer Azure SQL-Datenbank aus einer automatischen Sicherung (Azure-Portal).
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 62d94085-d7e7-4f08-bb83-404cf866a6c1
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.date: 10/18/2016
ms.author: sstein
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 85f8f2473e372a72c66153cd4131ba46e1c8c741


---
# <a name="restore-an-azure-sql-database-from-an-automatic-backup-using-the-azure-portal"></a>Wiederherstellen einer Azure SQL-Datenbank aus einer automatischen Sicherung über das Azure-Portal
> [!div class="op_single_selector"]
> * [Übersicht](sql-database-recovery-using-backups.md#geo-restore)
> * [Geowiederherstellung: mit PowerShell](sql-database-geo-restore-powershell.md)
> 
> 

In diesem Artikel wird erläutert, wie Sie Ihre Datenbank über das Azure-Portal mithilfe der [Geowiederherstellung](sql-database-recovery-using-backups.md#geo-restore) aus einer [automatischen Sicherung](sql-database-automated-backups.md) auf einem neuen Server wiederherstellen können.

## <a name="select-a-database-to-restore"></a>Auswählen der wiederherzustellenden Datenbank
Führen Sie zum Wiederherstellen einer Datenbank im Azure-Portal die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Wählen Sie auf der linken Bildschirmseite **+Neu** > **Datenbanken** > **SQL-Datenbank** aus:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/new-sql-database.png)
3. Wählen Sie als Quelle die Option **Sicherung**, und wählen Sie anschließend die Sicherung aus, die wiederhergestellt werden soll. Geben Sie einen Datenbanknamen sowie einen Server ein, auf dem die Datenbank wiederhergestellt werden soll, und klicken Sie dann auf **Erstellen**:
   
   ![Wiederherstellen einer Azure SQL-Datenbank](./media/sql-database-geo-restore-portal/geo-restore.png)

Überwachen Sie den Status des Wiederherstellungsvorgangs, indem Sie auf das Benachrichtigungssymbol oben rechts auf der Seite klicken.

## <a name="next-steps"></a>Nächste Schritte
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Informationen über schnellere Wiederherstellungsoptionen finden Sie unter [Aktive Georeplikation](sql-database-geo-replication-overview.md)  
* Informationen zum Verwenden automatisierter Sicherungen für die Archivierung finden Sie unter [Datenbankkopie](sql-database-copy.md)




<!--HONumber=Nov16_HO3-->


