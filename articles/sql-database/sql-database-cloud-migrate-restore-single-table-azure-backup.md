---
title: Wiederherstellen einer einzelnen Tabelle aus einer Azure SQL-Datenbanksicherung | Microsoft Docs
description: Erfahren Sie, wie Sie eine einzelne Tabelle aus einer Azure SQL-Datenbanksicherung wiederherstellen.
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: 340b41bd-9df8-47fb-adfc-03216de38a5e
ms.service: sql-database
ms.custom: migrate and move
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2017
ms.author: daleche
translationtype: Human Translation
ms.sourcegitcommit: 9992b6a2bf73fd84c7c47783d1f4f13e10889a81
ms.openlocfilehash: 8c074243db2ae729c4fd1a483e5ac40fbbebd750


---
# <a name="how-to-restore-a-single-table-from-an-azure-sql-database-backup"></a>Wiederherstellen einer einzelnen Tabelle aus einer Azure SQL-Datenbanksicherung
Es kann Situationen geben, in denen Sie versehentlich einige Daten in einer SQL-Datenbank geändert haben und die einzelne betroffene Tabelle wiederherstellen möchten. In diesem Artikel wird die Wiederherstellung einer einzelnen Tabelle in einer Datenbank aus einer der [automatischen Sicherungen](sql-database-automated-backups.md)der SQL-Datenbank beschrieben.

## <a name="preparation-steps-rename-the-table-and-restore-a-copy-of-the-database"></a>Vorbereitungsschritte: Benennen Sie die Tabelle um, und stellen Sie eine Kopie der Datenbank wieder her.
1. Identifizieren Sie die Tabelle in Ihrer Azure SQL-Datenbank, die Sie durch die wiederhergestellte Kopie ersetzen möchten. Verwenden Sie Microsoft SQL Management Studio zum Umbenennen der Tabelle. Benennen Sie die Tabelle beispielsweise in „&lt;Tabellenname&gt;_alt“ um.
   
   > [!NOTE]
   > Stellen Sie sicher, dass für die Tabelle, die umbenannt werden soll, keine Aktivitäten ausgeführt werden, um eine Blockierung zu vermeiden. Wenn Probleme auftreten, stellen Sie sicher, dass dieser Schritt während eines Wartungsfensters ausgeführt wird.
   >

2. Stellen Sie mithilfe der Schritte zur [Point-in-Time-Wiederherstellung](sql-database-recovery-using-backups.md#point-in-time-restore) eine Sicherung Ihrer Datenbank auf den Zustand zu einem von Ihnen gewünschten Zeitpunkt wieder her.
   
   > [!NOTE]
   > Der Name der wiederhergestellten Datenbank liegt im Format „Datenbankname+Zeitstempel“ vor. Beispiel: **Adventureworks2012_2016-01-01T22-12Z**. Durch diesen Schritt wird der vorhandene Datenbankname auf dem Server nicht überschrieben. Dies ist eine Sicherheitsmaßnahme, die Ihnen die Überprüfung der wiederhergestellten Datenbank ermöglicht, bevor die aktuelle Datenbank gelöscht und die wiederhergestellte Datenbank zum Einsatz in der Produktion umbenannt wird.
   
## <a name="copying-the-table-from-the-restored-database-by-using-the-sql-database-migration-tool"></a>Kopieren der Tabelle aus der wiederhergestellten Datenbank mithilfe des Tools für die SQL-Datenbankmigration 

1. Laden Sie den [SQL-Datenbankmigrations-Assistenten](https://sqlazuremw.codeplex.com)herunter, und installieren Sie ihn.
2. Öffnen Sie den SQL-Datenbankmigrations-Assistenten, wählen Sie auf der Seite **Prozess auswählen** die Einstellung **Datenbank in Analyse/Migration** aus, und klicken Sie dann auf **Weiter**.

   ![SQL-Datenbankmigrations-Assistent: Prozess auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/1.png)

3. Geben Sie im Dialogfeld **Verbindung mit dem Server herstellen** die folgenden Einstellungen an:

   * Servername: **Ihr SQL-Server**
   * Authentifizierung: **SQL Server-Authentifizierung**
   * Anmeldung: **Ihre Anmeldung**
   * Kennwort: **Ihr Kennwort**
   * Datenbank: **Masterdatenbank (alle Datenbanken auflisten)**
   
   > [!NOTE]
   > Der Assistent speichert standardmäßig Ihre Anmeldeinformationen. Wenn Sie dies nicht möchten, aktivieren Sie **Anmeldeinformationen vergessen**.
   >
   
     ![SQL-Datenbankmigrations-Assistent: Quelle auswählen, Schritt 1](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/2.png)
4. Wählen Sie im Dialogfeld **Quelle auswählen** aus dem Abschnitt **Vorbereitungsschritte** den Namen der wiederhergestellten Datenbank als Quelle aus, und klicken Sie auf **Weiter**.
   
    ![SQL-Datenbankmigrations-Assistent: Quelle auswählen, Schritt 2](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/3.png)
5. Wählen Sie im Dialogfeld **Objekte auswählen** die Option **Bestimmte Datenbankobjekte auswählen** und anschließend die Tabelle (oder Tabellen) aus, die Sie zum Zielserver migrieren möchten.
   ![SQL-Datenbankmigrations-Assistent: Objekte auswählen](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/4.png)
6. Klicken Sie auf der Seite **Skript-Assistent – Zusammenfassung** auf **Ja**, wenn Sie gefragt werden, ob Sie zum Erstellen eines SQL-Skripts bereit sind. Sie können das TSQL-Skript auch zur späteren Verwendung speichern.
   ![SQL-Datenbankmigrations-Assistent: Skript-Assistent – Zusammenfassung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/5.png)
7. Klicken Sie auf der Seite **Zusammenfassung der Ergebnisse** auf **Weiter**.
   ![SQL-Datenbankmigrations-Assistent: Ergebniszusammenfassung](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/6.png)
8. Klicken Sie auf der Seite **Zielserververbindung einrichten** auf **Verbindung mit Server herstellen**, und geben Sie dann folgende Informationen ein:
   
   * **Servername**: Zielserverinstanz.
   * **Authentifizierung**: **SQL Server-Authentifizierung**. Geben Sie Ihre Anmeldeinformationen ein.
   * **Datenbank**: **Masterdatenbank (alle Datenbanken auflisten)**. Mit dieser Option werden alle Datenbanken auf dem Zielserver aufgelistet.
     
     ![SQL-Datenbankmigrations-Assistent: Zielserververbindung einrichten](./media/sql-database-cloud-migrate-restore-single-table-azure-backup/7.png)
9. Klicken Sie auf **Verbinden**, wählen Sie die Zieldatenbank aus, in die Sie die Tabelle verschieben möchten, und klicken Sie dann auf **Weiter**. Dadurch wird die Ausführung des zuvor generierten Skripts abgeschlossen, und es sollte die neu verschobene Tabelle angezeigt werden, die Sie in die Zieldatenbank kopiert haben.

## <a name="verification-step"></a>Überprüfungsschritt

- Testen Sie die neu kopierte Tabelle, und führen Sie Abfragen aus, um sicherzustellen, dass die Daten intakt sind. Wenn Sie sich von der Richtigkeit der Daten überzeugt haben, können Sie die umbenannte Tabelle aus dem Abschnitt **Vorbereitungsschritte** löschen. (z.B. „&lt;Tabellenname&gt;_alt“).

## <a name="next-steps"></a>Nächste Schritte
[Übersicht: Automatisierte SQL-Datenbanksicherungen](sql-database-automated-backups.md)




<!--HONumber=Jan17_HO3-->


