---
title: 'Azure-Portal: Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen | Microsoft Docs'
description: "Kurzübersicht zum Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung von automatisierten Azure SQL-Datenbank-Sicherungen in einem Azure Recovery Services-Tresor über das Azure-Portal"
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
ms.date: 12/22/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: f234e50d194c1b8b8deed8282a6ab1250b53d075
ms.openlocfilehash: d055e40305b277338ae85e8d6c53d3d76a78d235
ms.lasthandoff: 02/16/2017


---
# <a name="configure-manage-and-restore-from-long-term-retention-of-database-backups-in-an-azure-recovery-services-vault-using-the-azure-portal"></a>Konfigurieren, Verwalten und Wiederherstellen der langfristigen Aufbewahrung von Datenbanksicherungen in einem Azure Recovery Services-Tresor über das Azure-Portal

In diesem Thema wird beschrieben, wie Sie die langfristige Aufbewahrung von automatisierten Sicherungen in einem Azure Recovery Services-Tresor über das Azure-Portal konfigurieren, verwalten und wiederherstellen. Diese Aufgabe können Sie auch mithilfe von [PowerShell](sql-database-manage-long-term-backup-retention-powershell.md) ausführen.

Weitere Informationen zur langfristigen Aufbewahrung von Sicherungen finden Sie unter [Langfristiges Aufbewahren von Sicherungen](sql-database-long-term-retention.md).

> [!TIP]
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung über das Azure-Portal](sql-database-get-started-backup-recovery-portal.md).
>

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
> Ein Tutorial finden Sie unter [Erste Schritte mit der Sicherung und Wiederherstellung für Datenschutz und Wiederherstellung](sql-database-get-started-backup-recovery-portal.md).

## <a name="next-steps"></a>Nächste Schritte

- Informationen zum Verwalten von Sicherungen in der langfristigen Sicherungsaufbewahrung mithilfe von PowerShell finden Sie unter [Verwalten der langfristigen Aufbewahrung von Datenbanksicherungen mithilfe von PowerShell](sql-database-manage-long-term-backup-retention-powershell.md).
- Weitere Informationen zu vom Dienst generierten automatischen Sicherungen finden Sie im Artikel zu [automatischen Sicherungen](sql-database-automated-backups.md).
- Weitere Informationen zur langfristigen Beibehaltung von Sicherungen finden Sie im Artikel zur [langfristigen Beibehaltung von Sicherungen](sql-database-long-term-retention.md).
- Weitere Informationen zum Wiederherstellen von Daten aus Sicherungen finden Sie im Artikel zur [Wiederherstellung aus einer Sicherung](sql-database-recovery-using-backups.md).
