---
title: Anzeigen der Sicherungen im Azure Recovery Services-Tresor | Microsoft-Dokumentation
description: "Kurzübersicht über das Anzeigen der Sicherungen im Azure Recovery Services-Tresor und den von diesen Sicherungen belegten Speicherplatz"
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
ms.openlocfilehash: 723be587128c131670c9905572d808c176362fb8


---
# <a name="view-information-about-your-database-backups-in-long-term-backup-retention"></a>Zeigen Sie Informationen zu Ihren Datenbanksicherungen mit langfristiger Beibehaltung der Sicherung an.

In diesem Thema erfahren Sie, wie Sie Informationen zu Ihren Datenbanksicherungen mit langfristiger Beibehaltung der Sicherung anzeigen.

## <a name="view-long-term-backup-retention-information-using-the-azure-portal"></a>Anzeigen von Informationen zur langfristigen Beibehaltung der Sicherung über das Azure-Portal 

1. Öffnen Sie das Blatt für den Azure Recovery Services-Tresor (wechseln Sie zu **Alle Ressourcen**, und wählen Sie den Tresor in der Liste der Ressourcen für Ihr Abonnement aus), um den Speicherplatz anzuzeigen, den Ihre Datenbanksicherungen im Tresor belegen.

   ![Recovery Services-Tresor mit Sicherungen anzeigen](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Öffnen Sie das Blatt **SQL-Datenbank** für Ihre Datenbank.

    ![Blatt für neue Beispieldatenbank](./media/sql-database-get-started/new-sample-db-blade.png)

3. Klicken Sie auf der Symbolleiste auf **Wiederherstellen**.

    ![„Wiederherstellen“ auf der Symbolleiste](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. Klicken Sie auf dem Blatt „Wiederherstellen“ auf **Langfristig**.

5. Klicken Sie unter „Azure vault backups“ (Sicherungen im Azure-Tresor) auf **Sicherung auswählen**, um die verfügbaren Datenbanksicherungen mit langfristiger Beibehaltung anzuzeigen.

    ![Sicherungen im Tresor](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor finden Sie unter [](sql-database-configure-long-term-retention.md).
- Informationen zum Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung finden Sie unter [Restore a database from a backup in long-term backup retention](sql-database-restore-from-long-term-retention.md) (Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung der Sicherung).
- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).


<!--HONumber=Dec16_HO2-->


