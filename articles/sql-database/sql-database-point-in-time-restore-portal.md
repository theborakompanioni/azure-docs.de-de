---
title: "Azure-Portal: Wiederherstellen des Zustands einer Azure SQL-Datenbank-Instanz zu einem früheren Zeitpunkt | Microsoft Docs"
description: "Wiederherstellen des Zustands einer Azure SQL-Datenbank-Instanz zu einem früheren Zeitpunkt über das Azure-Portal"
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
ms.date: 12/08/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 120075710d466a10233aadfc1f7e29da871f8f83
ms.openlocfilehash: 342df453a2e4cc573117fd59fd5c3aa899295439
ms.lasthandoff: 02/16/2017


---
# <a name="restore-an-azure-sql-database-to-a-previous-point-in-time-with-the-azure-portal"></a>Wiederherstellen des Zustands einer Azure SQL-Datenbank-Instanz zu einem früheren Zeitpunkt über das Azure-Portal

In diesem Artikel erfahren Sie, wie Sie aus [automatisierten SQL-Datenbanksicherungen](sql-database-automated-backups.md) einen früheren Zustand Ihrer Datenbank wiederherstellen. Verwenden von PowerShell Diese Aufgabe kann auch [mithilfe von PowerShell](sql-database-point-in-time-restore-powershell.md) ausgeführt werden.

## <a name="restore-to-a-previous-point-in-time"></a>Wiederherstellung des Zustands zu einem früheren Zeitpunkt 

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

## <a name="next-steps"></a>Nächste Schritte
* Informationen über automatisierte Sicherungen von Azure SQL-Datenbanken finden Sie unter [Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)
* Informationen zum Verwenden automatisierter Sicherungen für die Wiederherstellung finden Sie unter [Wiederherstellen einer Datenbank aus vom Dienst initiierten Sicherungen](sql-database-recovery-using-backups.md)
* Eine Übersicht und verschiedene Szenarien zum Thema Geschäftskontinuität finden Sie unter [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)


