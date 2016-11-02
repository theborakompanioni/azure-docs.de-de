<properties
   pageTitle="Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal) | Microsoft Azure"
   description="Azure-Portal-Aufgaben zum Wiederherstellen einer Azure SQL Data Warehouse-Instanz."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="Lakshmi1812"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/21/2016"
   ms.author="lakshmir;barbkess;sonyama"/>


# <a name="restore-an-azure-sql-data-warehouse-(portal)"></a>Wiederherstellen einer Azure SQL Data Warehouse-Instanz (Portal)

> [AZURE.SELECTOR]
- [Übersicht][]
- [Portal][]
- [PowerShell][]
- [REST][]

In diesem Artikel erfahren Sie, wie Sie eine Azure SQL Data Warehouse-Instanz über das Azure-Portal wiederherstellen.

## <a name="before-you-begin"></a>Voraussetzungen

**Überprüfen Sie Ihre DTU-Kapazität.** Jedes SQL Data Warehouse wird von einer SQL Server-Instanz gehostet (z.B. myserver.database.windows.net), die über ein Standard-DTU-Kontingent verfügt.  Bevor Sie ein SQL Data Warehouse wiederherstellen können, überprüfen Sie, ob Ihre SQL Server-Instanz über genügend verbleibendes DTU-Kontingent für die Datenbank-Wiederherstellung verfügt. Informationen zum Berechnen des DTU-Bedarfs bzw. zur Anforderung weiterer DTUs finden Sie unter [Anfordern einer DTU-Kontingentänderung][].


## <a name="restore-an-active-or-paused-database"></a>Wiederherstellen einer aktiven oder angehaltenen Datenbank

So stellen Sie eine Datenbank wieder her:

1. Melden Sie sich beim [Azure-Portal][]
2. Wählen Sie auf der linken Bildschirmseite **Durchsuchen** und dann **SQL Server** aus.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)
    
3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-server.png)

4. Suchen Sie die SQL Data Warehouse-Instanz, mit der Sie die Wiederherstellung durchführen möchten, und wählen Sie sie aus.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-active-dw.png)
5. Klicken Sie oben auf dem Data Warehouse-Blatt auf **Wiederherstellen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-select-restore-from-active.png)

6. Geben Sie einen neuen **Datenbanknamen**
7. Wählen Sie den letzten **Wiederherstellungspunkt**
    1. Stellen Sie sicher, dass Sie den letzten Wiederherstellungspunkt auswählen.  Da Wiederherstellungspunkte in UTC angezeigt werden, ist manchmal die gezeigte Standardoption nicht der letzte Wiederherstellungspunkt.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-restore-blade-from-active.png)

8. Klicken Sie auf **OK**
9. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN**

>[AZURE.NOTE] Nach Abschluss der Wiederherstellung können Sie Ihre wiederhergestellte Datenbank konfigurieren. Befolgen Sie hierzu die Anleitung [Konfigurieren der Datenbank nach der Wiederherstellung][].


## <a name="restore-a-deleted-database"></a>Wiederherstellen einer gelöschten Datenbank

So stellen Sie eine gelöschte Datenbank wieder her

1. Melden Sie sich beim [Azure-Portal][]
2. Wählen Sie auf der linken Bildschirmseite **Durchsuchen** und dann **SQL Server** aus.
    
    ![](./media/sql-data-warehouse-restore-database-portal/01-browse-for-sql-server.png)

3. Navigieren Sie zu Ihrem Server, und wählen Sie ihn aus.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-server.png)

4. Führen Sie auf dem Serverblatt einen Bildlauf nach unten zum Abschnitt mit den Vorgängen aus.
5. Klicken Sie auf die Kachel **Gelöschte Datenbanken** .
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dws.png)

6. Klicken Sie auf die Datenbank, die Sie wiederherstellen möchten.
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-select-deleted-dw.png)

7. Geben Sie einen neuen **Datenbanknamen**
    
    ![](./media/sql-data-warehouse-restore-database-portal/02-restore-blade-from-deleted.png)
    
8. Klicken Sie auf **OK**
9. Der Datenbank-Wiederherstellungsvorgang wird gestartet und kann mithilfe von **BENACHRICHTIGUNGEN**

>[AZURE.NOTE] Informationen zum Konfigurieren der Datenbank nach Abschluss der Wiederherstellung finden Sie unter [Konfigurieren der Datenbank nach der Wiederherstellung][]. 

## <a name="next-steps"></a>Nächste Schritte
Informationen zu den Geschäftskontinuitätsfunktionen von Azure SQL-Datenbank-Editionen finden Sie in der [Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität][].

<!--Image references-->

<!--Article references-->
[Azure SQL-Datenbank-Übersicht zur Geschäftskontinuität]: ./sql-database-business-continuity.md
[Übersicht]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md
[Konfigurieren der Datenbank nach der Wiederherstellung]: ./sql-database-disaster-recovery.md#configure-your-database-after-recovery
[Anfordern einer DTU-Kontingentänderung]: ./sql-data-warehouse-get-started-create-support-ticket.md#request-quota-change

<!--MSDN references-->

<!--Blog references-->

<!--Other Web references-->
[Azure-Portal]: https://portal.azure.com/



<!--HONumber=Oct16_HO2-->


