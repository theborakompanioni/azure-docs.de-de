---
title: Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor | Microsoft-Dokumentation
description: "Enthält eine Kurzübersicht über die Konfiguration der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor."
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
ms.openlocfilehash: b395fc5c1deaf04205dcfccacd13d30ec90dd1e7


---
# <a name="configure-long-term-retention-of-automated-backups-in-an-azure-recovery-services-vault"></a>Konfigurieren der langfristigen Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor 

In diesem Thema wird beschrieben, wie Sie die langfristige Beibehaltung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor konfigurieren.

## <a name="configure-long-term-retention-using-the-azure-portal"></a>Konfigurieren der langfristigen Beibehaltung mit dem Azure-Portal

1. Öffnen Sie das Blatt **SQL Server** für Ihren Server.

    ![Blatt „SQL Server“](./media/sql-database-get-started/sql-server-blade.png)

2. Klicken Sie auf **Long-term backup retention** (Langfristige Beibehaltung der Sicherung).

   ![Link „Long-term backup retention“ (Langfristige Beibehaltung der Sicherung)](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. Lesen und akzeptieren Sie auf dem Blatt **Long-term backup retention** (Langfristige Beibehaltung der Sicherung) die Preview-Bedingungen. Dies ist nicht erforderlich, wenn Sie diesen Schritt bereits ausgeführt haben oder sich das Feature nicht mehr in der Vorschauphase befindet.

   ![Preview-Bedingungen akzeptieren](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. Um die langfristige Sicherungsaufbewahrung für eine Datenbank zu konfigurieren, wählen Sie die Datenbank im Raster aus und klicken dann auf der Symbolleiste auf **Konfigurieren**.

   ![Datenbank für langfristige Beibehaltung der Sicherung auswählen](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. Klicken Sie auf dem Blatt **Konfigurieren** unter **Recovery service vault** (Recovery Services-Tresor) auf **Erforderliche Einstellungen konfigurieren**.

   ![Link zum Konfigurieren des Tresors](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. Wählen Sie auf dem Blatt **Recovery Services-Tresor** einen vorhandenen Tresor aus. Falls kein Recovery Services-Tresor für Ihr Abonnement vorhanden ist, klicken Sie auf den entsprechenden Link, um den Vorgang zu beenden, und erstellen Sie einen Recovery Services-Tresor.

   ![Link zum Erstellen eines neuen Tresors](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. Klicken Sie auf dem Blatt **Recovery Services-Tresore** auf **Hinzufügen**.

   ![Link zum Hinzufügen eines neuen Tresors](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. Geben Sie auf dem Blatt **Recovery Services-Tresor** einen gültigen Namen für den neuen Recovery Services-Tresor ein.

   ![Neuer Tresorname](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Wählen Sie Ihr Abonnement und die Ressourcengruppe und dann den Standort für den Tresor aus. Klicken Sie anschließend auf **Erstellen**.

   ![Neuen Tresor erstellen](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > Der Tresor muss sich in der gleichen Region wie der logische SQL Azure-Server befinden und die gleiche Ressourcengruppe wie der logische Server verwenden.
   >

10. Führen Sie nach der Erstellung des neuen Tresors die notwendigen Schritte durch, um zum Blatt **Recovery Services-Tresor** zurückzukehren.

11. Klicken Sie auf dem Blatt **Recovery Services-Tresor** auf den Tresor und dann auf **Auswählen**.

   ![Vorhandenen Tresor auswählen](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. Geben Sie auf dem Blatt **Konfigurieren** einen gültigen Namen für die neue Aufbewahrungsrichtlinie ein, ändern Sie die Standardaufbewahrungsrichtlinie wie erforderlich, und klicken Sie dann auf **OK**.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. Klicken Sie auf dem Blatt **Long-term backup retention** (Langfristige Beibehaltung der Sicherung) auf **Speichern** und dann auf **OK**, um die Richtlinie für langfristige Beibehaltung der Sicherung auf alle ausgewählten Datenbanken anzuwenden.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klicken Sie auf **Speichern**, um die langfristige Sicherungsaufbewahrung anhand der neuen Richtlinie für den von Ihnen konfigurierten Azure Recovery Services-Tresor zu aktivieren.

   ![Aufbewahrungsrichtlinie definieren](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

14. Öffnen Sie nach dem Aktivieren der langfristigen Sicherungsaufbewahrung das Blatt **Recovery Services-Tresor** (wechseln Sie zu **Alle Ressourcen**, und wählen Sie den Tresor in der Liste der Ressourcen für Ihr Abonnement aus).

   ![Recovery Services-Tresors anzeigen](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault.png)


> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery.md).
>

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Wiederherstellen einer Datenbank aus einer Sicherung mit langfristiger Beibehaltung finden Sie im Artikel zum [Wiederherstellen aus einer Sicherung mit langfristiger Beibehaltung](sql-database-restore-from-long-term-retention.md).
- Informationen zum Anzeigen von Sicherungen im Azure Recovery Services-Tresor finden Sie unter [Anzeigen von Sicherungen mit langfristiger Beibehaltung](sql-database-view-backups-in-vault.md).
- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](: https://azure.microsoft.com/en-us/documentation/articles/)sql-database-automated-backups.MD.
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).


<!--HONumber=Dec16_HO2-->


