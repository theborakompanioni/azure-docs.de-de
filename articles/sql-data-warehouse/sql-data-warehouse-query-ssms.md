---
title: "Verbinden mit Azure SQL Data Warehouse – SSMS | Microsoft Docs"
description: Verwenden Sie SQL Server Management Studio (SSMS), um eine Verbindung mit Azure SQL Data Warehouse herzustellen und das Data Warehouse abzufragen.
services: sql-data-warehouse
documentationcenter: 
author: hirokib
manager: jhubbard
editor: 
ms.assetid: 299e50b3-e68a-471c-8aee-b0b9874781bd
ms.service: sql-data-warehouse
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 10/31/2016
ms.author: elbutter;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 77474214c6fafe7f591030d30f6a46c66fbc5c09
ms.openlocfilehash: 30ac3558534e96b63d78f9c66d42d11b7c1a3c75


---
# <a name="connect-to-sql-data-warehouse-with-sql-server-management-studio-ssms"></a>Herstellen einer Verbindung mit der SQL Data Warehouse-Instanz mit SQL Server Management Studio (SSMS)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

Verwenden Sie SQL Server Management Studio (SSMS), um eine Verbindung mit Azure SQL Data Warehouse herzustellen und das Data Warehouse abzufragen. 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Eine vorhandene SQL Data Warehouse-Instanz. Informationen zur Erstellung finden Sie unter [Erstellen eines SQL Data Warehouse][Erstellen eines SQL Data Warehouse].
* SQL Server Management Studio (SSMS) muss installiert sein. [Installieren Sie SSMS][Installieren von SSMS] kostenlos, falls Sie noch nicht über die Anwendung verfügen.
* Den vollqualifizierten SQL-Servernamen. Informationen zur Ermittlung finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse][Herstellen einer Verbindung mit SQL Data Warehouse].

## <a name="1-connect-to-your-sql-data-warehouse"></a>1. Herstellen einer Verbindung mit SQL Data Warehouse
1. Öffnen Sie SSMS.
2. Öffnen Sie den Objekt-Explorer. Wählen Sie zu diesem Zweck **Datei** > **Objekt-Explorer verbinden** aus.
   
    ![SQL Server-Objekt-Explorer][1]
3. Füllen Sie die Felder im Fenster zum Herstellen einer Verbindung mit dem Server aus.
   
    ![Verbindung mit dem Server herstellen][2]
   
   * **Servername**. Geben Sie den zuvor ermittelten **Servernamen** ein.
   * **Authentifizierung**. Wählen Sie SQL **Server-Authentifizierung** oder **Integrierte Active Directory-Authentifizierung**.
   * **Benutzername** und **Kennwort**. Geben Sie Benutzername und Kennwort ein, wenn Sie oben „SQL Server-Authentifizierung“ ausgewählt haben.
   * Klicken Sie auf **Verbinden**.
4. Erweitern Sie den Azure SQL-Server. Sie können die dem Server zugeordneten Datenbanken anzeigen. Erweitern Sie „AdventureWorksDW“, um die Tabellen in Ihrer Beispieldatenbank anzuzeigen.
   
    ![AdventureWorksDW erkunden][3]

## <a name="2-run-a-sample-query"></a>2. Ausführen einer Beispielabfrage
Nachdem jetzt eine Verbindung mit Ihrer Datenbank hergestellt wurde, schreiben wir eine Abfrage.

1. Klicken Sie mit der rechten Maustaste im SQL Server-Objekt-Explorer auf Ihre Datenbank.
2. Wählen Sie **Neue Abfrage**aus. Ein neues Abfragefenster wird geöffnet.
   
    ![Neue Abfrage][4]
3. Kopieren Sie die folgende TSQL-Abfrage in das Abfragefenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Führen Sie die Abfrage aus. Zu diesem Zweck klicken Sie auf `Execute` oder drücken auf `F5`.
   
    ![Abfrage ausführen][5]
5. Sehen Sie sich die Abfrageergebnisse an. In diesem Beispiel weist die Tabelle „FactInternetSales“ 60398 Zeilen auf.
   
    ![Abfrageergebnisse][6]

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [die Daten mit Power BI zu visualisieren][die Daten mit Power BI zu visualisieren].

Informationen zum Konfigurieren der Umgebung für die Azure Active Directory-Authentifizierung finden Sie unter [Authentifizieren für SQL Data Warehouse][Authentifizieren für SQL Data Warehouse].

<!--Arcticles-->
[Herstellen einer Verbindung mit SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Authentifizieren für SQL Data Warehouse]: sql-data-warehouse-authentication.md
[die Daten mit Power BI zu visualisieren]: sql-data-warehouse-get-started-visualize-with-power-bi.md 

<!--Other-->
[Azure-Portal]: https://portal.azure.com
[Installieren von SSMS]: https://msdn.microsoft.com/en-US/library/hh213248.aspx


<!--Image references-->

[1]: media/sql-data-warehouse-query-ssms/connect-object-explorer.png
[2]: media/sql-data-warehouse-query-ssms/connect-object-explorer1.png
[3]: media/sql-data-warehouse-query-ssms/explore-tables.png
[4]: media/sql-data-warehouse-query-ssms/new-query.png
[5]: media/sql-data-warehouse-query-ssms/execute-query.png
[6]: media/sql-data-warehouse-query-ssms/results.png



<!--HONumber=Nov16_HO3-->


