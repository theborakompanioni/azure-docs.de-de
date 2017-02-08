---
title: Wiederherstellen einer Datenbank aus einer Sicherung im Azure Recovery Services-Tresor | Microsoft Docs
description: "Kurzübersicht über das Wiederherstellen einer Datenbank für eine Sicherung im Azure Recovery Services-Tresor und den von diesen Sicherungen belegten Speicherplatz"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
ms.date: 12/07/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 6d729f89322b67fe75d2c0ad001e70b42ccedb7e
ms.openlocfilehash: cdb898ef86517b6eb6eed4b2ae38c4e327e78181


---
# <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung

In diesem Thema erfahren Sie, wie Sie eine Datenbank aus einer Sicherung mit langfristiger Beibehaltung wiederherstellen.

## <a name="restore-from-long-term-backup-retention-using-the-azure-portal"></a>Wiederherstellen langfristig aufbewahrter Sicherungen über das Azure-Portal

1. Klicken Sie auf dem Blatt **Azure vault backups** (Sicherungen im Azure-Tresor) auf die Sicherung, die Sie wiederherstellen möchten, und dann auf **Auswählen**.

    ![Sicherung im Tresor auswählen](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. Geben Sie im Textfeld **Datenbankname** den Namen für die wiederhergestellte Datenbank an.

    ![Neuer Datenbankname](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klicken Sie auf **OK**, um die Datenbank aus der Sicherung im Tresor in der neuen Datenbank wiederherzustellen.

4. Klicken Sie auf der Symbolleiste auf das Benachrichtigungssymbol, um den Status des Wiederherstellungsauftrags anzuzeigen.

    ![Status des Wiederherstellungsauftrags (aus dem Tresor)](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. Öffnen Sie nach Abschluss des Wiederherstellungsauftrags das Blatt **SQL-Datenbanken**, um die neu wiederhergestellte Datenbank anzuzeigen.

    ![Aus dem Tresor wiederhergestellte Datenbank](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor finden Sie unter [Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor](sql-database-configure-long-term-retention.md).
- Informationen zum Anzeigen von Sicherungen im Azure Recovery Services-Tresor finden Sie unter [Anzeigen von Sicherungen mit langfristiger Beibehaltung](sql-database-view-backups-in-vault.md).
- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](: https://azure.microsoft.com/en-us/documentation/articles/)sql-database-automated-backups.MD.
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).


<!--HONumber=Dec16_HO2-->


