---
title: Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Azure-Portal) | Microsoft-Dokumentation
description: "Enthält Azure-Portal-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: barbkess
editor: 
ms.assetid: b0aef539-7657-4b0e-9899-74098f5c21bc
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 09/21/2016
ms.author: lakshmir;barbkess;sonyama
translationtype: Human Translation
ms.sourcegitcommit: 763ae02d1196a924bbf89e423871af2ec22e9e6c
ms.openlocfilehash: 84f582e8d767466ce9dec83203ec78047648caea


---
# <a name="restore-azure-sql-data-warehouse-portal"></a>Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal)
> [!div class="op_single_selector"]
> * [Übersicht][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
>
>
In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz über das Azure-Portal wiederherstellen.

## <a name="before-you-begin"></a>Voraussetzungen
**Überprüfen Sie Ihre DTU-Kapazität.** Jede Instanz von SQL Data Warehouse wird von einer SQL Server-Instanz (z.B. „myserver.database.windows.net“) gehostet, die über ein DTU-Standardkontingent (Data Throughput Unit) verfügt. Überprüfen Sie vor dem Wiederherstellen einer SQL Data Warehouse-Instanz, ob das DTU-Kontingent Ihrer SQL Server-Instanz für die wiederherzustellende Datenbank ausreicht. Informationen zum Berechnen des DTU-Kontingents bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung][Request a DTU quota change].

## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer aktiven oder angehaltenen Datenbank
So stellen Sie eine Datenbank wieder her:

1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Wählen Sie im linken Bereich die Option **Durchsuchen** und die Option **Computer mit SQL Server**.

    ![Wählen Sie „Durchsuchen“ > „Computer mit SQL Server“.](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Suchen Sie nach Ihrem Server, und wählen Sie ihn aus.

    ![Auswählen des Servers](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)
4. Suchen Sie nach der Instanz von SQL Data Warehouse, mit der Sie die Wiederherstellung durchführen möchten, und wählen Sie sie aus.

    ![Auswählen der Instanz von SQL Data Warehouse für die Wiederherstellung](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Wählen Sie oben auf dem Data Warehouse-Blatt die Option **Wiederherstellen**.

    ![Auswählen von „Wiederherstellen“](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)
6. Geben Sie einen neuen **Datenbanknamen** ein.
7. Wählen Sie den letzten **Wiederherstellungspunkt** aus.

   Stellen Sie sicher, dass Sie den letzten Wiederherstellungspunkt auswählen. Da Wiederherstellungspunkte in Coordinated Universal Time (UTC) angezeigt werden, kann es sein, dass die Standardoption unter Umständen nicht der letzte Wiederherstellungspunkt ist.

      ![Auswählen eines Wiederherstellungspunkts](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)
8. Klicken Sie auf **OK**.
9. Der Prozess für die Datenbankwiederherstellung beginnt, und Sie können **BENACHRICHTIGUNGEN** verwenden, um den Prozess zu überwachen.

> [!NOTE]
> Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung unter [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].
>
>

## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank
So stellen Sie eine gelöschte Datenbank wieder her

1. Melden Sie sich beim [Azure-Portal][Azure portal] an.
2. Wählen Sie im linken Bereich die Option **Durchsuchen** und die Option **Computer mit SQL Server**.

    ![Wählen Sie „Durchsuchen“ > „Computer mit SQL Server“.](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
3. Suchen Sie nach Ihrem Server, und wählen Sie ihn aus.

    ![Auswählen des Servers](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)
4. Führen Sie auf dem Serverblatt einen Bildlauf nach unten zum Abschnitt mit den **Vorgängen** durch.
5. Wählen Sie die Kachel **Gelöschte Datenbanken**.

    ![Auswählen der Kachel „Gelöschte Datenbanken“](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)
6. Wählen Sie die gelöschte Datenbank aus, die Sie wiederherstellen möchten.

    ![Auswählen der wiederherzustellenden Datenbank](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)
7. Geben Sie einen neuen **Datenbanknamen** ein.

    ![Hinzufügen eines Namens für die Datenbank](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
8. Klicken Sie auf **OK**.
9. Der Prozess für die Datenbankwiederherstellung beginnt, und Sie können **BENACHRICHTIGUNGEN** verwenden, um den Prozess zu überwachen.

> [!NOTE]
> Informationen zum Konfigurieren der Datenbank nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung][Configure your database after recovery].
>
>

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][Azure SQL Database business continuity overview].

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Configure your database after recovery]: ../sql-database/sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Request a DTU quota change]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure portal]: https://portal.azure.com/



<!--HONumber=Dec16_HO3-->


