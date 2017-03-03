---
title: Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals | Microsoft Docs
description: Erstellen der Kopie einer Azure SQL-Datenbank
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: daa6f079-13ed-462f-b346-e201aa61681b
ms.service: sql-database
ms.custom: migrate and move
ms.devlang: NA
ms.date: 02/07/2017
ms.author: carlrab
ms.workload: data-management
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 60bcd02d24e2084b9020ce56ef6a9f8268c6b1b5
ms.openlocfilehash: 5da07683a4ca9fe1751debc163a0a5a595b6f82b
ms.lasthandoff: 02/11/2017


---
# <a name="copy-an-azure-sql-database-using-the-azure-portal"></a>Kopieren einer Azure SQL-Datenbank mithilfe des Azure-Portals

Die folgenden Schritte veranschaulichen, wie Sie eine SQL-Datenbank über das [Azure-Portal](https://portal.azure.com) auf den gleichen oder einen anderen Server kopieren. 

> [!NOTE]
> Sie können eine SQL-Datenbank auch über [PowerShell](sql-database-copy-powershell.md) oder [Transact-SQL](sql-database-copy-transact-sql.md) kopieren.
>

Zum Kopieren einer SQL-Datenbank benötigen Sie Folgendes:

* Ein Azure-Abonnement. Wenn Sie ein Azure-Abonnement benötigen, klicken Sie einfach oben auf dieser Seite auf den Link **Kostenloses Konto** klicken. Lesen Sie anschließend den Artikel weiter.
* Eine zu kopierende SQL-Datenbank. Wenn Sie nicht über eine SQL-Datenbank verfügen, können Sie die Erstellung anhand der Schritte im folgenden Artikel durchführen: [Erstellen der ersten Azure SQL-Datenbank](sql-database-get-started.md).

## <a name="copy-your-sql-database"></a>Kopieren der SQL-Datenbank
Öffnen Sie das Blatt „SQL-Datenbank“ für die Datenbank, die Sie kopieren möchten:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**, und klicken Sie dann auf die gewünschte Datenbank.
3. Klicken Sie auf der Seite der SQL-Datenbank auf **Kopieren**:
   
   ![SQL-Datenbank](./media/sql-database-copy-portal/sql-database-copy.png)
4. Auf der Seite **Kopieren** wird ein Standarddatenbankname bereitgestellt. Geben Sie bei Bedarf einen anderen Namen ein (alle Datenbanken auf einem Server müssen eindeutige Namen aufweisen).
5. Wählen Sie einen **Zielserver**aus. Auf dem Zielserver wird die Kopie der Datenbank erstellt. Sie können die Datenbank auf den gleichen oder einen anderen Server kopieren. Sie können einen Server erstellen oder einen vorhandenen Server aus der Liste auswählen. 
6. Nach Auswahl des **Zielservers** werden die Optionen **Pool für elastische Datenbanken** und **Tarif** aktiviert. Wenn der Server über einen Pool verfügt, können Sie die Datenbank in diesen Pool kopieren.
7. Klicken Sie auf **OK** , um den Kopiervorgang zu starten.
   
   ![SQL-Datenbank](./media/sql-database-copy-portal/copy-page.png)

## <a name="monitor-the-progress-of-the-copy-operation"></a>Überwachen des Fortschritts des Kopiervorgangs
* Klicken Sie nach dem Starten des Kopiervorgangs auf die Portalbenachrichtigung, um Details anzuzeigen.
  
    ![Benachrichtigung][3]
  
    ![Überwachen][4]

## <a name="verify-the-database-is-live-on-the-server"></a>Sicherstellen, dass die Datenbank auf dem Server aktiv ist
* Klicken Sie auf **Weitere Dienste** > **SQL-Datenbanken**, und überprüfen Sie, ob die neue Datenbank **online** ist.

## <a name="resolve-logins"></a>Auflösen von Anmeldungen
Informationen zum Auflösen von Anmeldungen, nachdem der Kopiervorgang abgeschlossen wurde, finden Sie unter [Auflösen von Anmeldungen](sql-database-copy-transact-sql.md#resolve-logins-after-the-copy-operation-completes)

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Verwalten von Benutzern und Anmeldungen beim Kopieren einer Datenbank auf einen anderen logischen Server finden Sie unter [Verwalten der Sicherheit der Azure SQL-Datenbank nach der Notfallwiederherstellung](sql-database-geo-replication-security-config.md).
* Informationen zum Exportieren einer Datenbank in eine BACPAC-Datei im Azure-Portal finden Sie unter [Exportieren der Datenbank in eine BACPAC-Datei im Azure-Portal](sql-database-export-portal.md).
* [Übersicht über die Geschäftskontinuität](sql-database-business-continuity.md)
* [SQL-Datenbank-Dokumentation](https://azure.microsoft.com/documentation/services/sql-database/)

<!--Image references-->
[1]: ./media/sql-database-copy-portal/copy.png
[2]: ./media/sql-database-copy-portal/copy-ok.png
[3]: ./media/sql-database-copy-portal/copy-notification.png
[4]: ./media/sql-database-copy-portal/monitor-copy.png


