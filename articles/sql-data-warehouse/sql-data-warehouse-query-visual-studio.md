---
title: Abfragen von Azure SQL Data Warehouse (Visual Studio) | Microsoft Docs
description: Es wird beschrieben, wie Sie SQL Data Warehouse mit Visual Studio abfragen.
services: sql-data-warehouse
documentationcenter: NA
author: sonyam
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 06/16/2016
ms.author: sonyama;barbkess

---
# Abfragen von Azure SQL Data Warehouse (Visual Studio)
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> 
> 

Mit Visual Studio können Sie Azure SQL Data Warehouse innerhalb weniger Minuten abfragen. Bei dieser Methode wird die SSDT-Erweiterung (SQL Server Data Tools) in Visual Studio verwendet.

## Voraussetzungen
Für dieses Tutorial benötigen Sie Folgendes:

* Eine vorhandene SQL Data Warehouse-Instanz. Informationen zur Erstellung finden Sie unter [Erstellen eines SQL Data Warehouse][Erstellen eines SQL Data Warehouse].
* SSDT für Visual Studio. Wenn Sie über Visual Studio verfügen, ist diese Komponente darin wahrscheinlich bereits enthalten. Installationshinweise und -optionen finden Sie unter [Installieren von Visual Studio und SSDT][Installieren von Visual Studio und SSDT].
* Den vollqualifizierten SQL-Servernamen. Informationen zur Ermittlung finden Sie unter [Herstellen einer Verbindung mit SQL Data Warehouse][Herstellen einer Verbindung mit SQL Data Warehouse].

## 1\. Herstellen einer Verbindung mit SQL Data Warehouse
1. Öffnen Sie Visual Studio 2013 oder 2015.
2. Öffnen Sie den SQL Server-Objekt-Explorer. Wählen Sie zu diesem Zweck **Ansicht** > **SQL Server-Objekt-Explorer** aus.
   
    ![SQL Server-Objekt-Explorer][1]
3. Klicken Sie auf das Symbol **SQL Server hinzufügen**.
   
    ![SQL Server-Instanz hinzufügen][2]
4. Füllen Sie die Felder im Fenster zum Herstellen einer Verbindung mit dem Server aus.
   
    ![Verbindung mit dem Server herstellen][3]
   
   * **Servername**. Geben Sie den zuvor ermittelten **Servernamen** ein.
   * **Authentifizierung**. Wählen Sie **SQL Server-Authentifizierung** oder **Integrierte Active Directory-Authentifizierung**.
   * **Benutzername** und **Kennwort**. Geben Sie Benutzername und Kennwort ein, wenn Sie oben „SQL Server-Authentifizierung“ ausgewählt haben.
   * Klicken Sie auf **Verbinden**.
5. Erweitern Sie den Azure SQL-Server. Sie können die dem Server zugeordneten Datenbanken anzeigen. Erweitern Sie „AdventureWorksDW“, um die Tabellen in Ihrer Beispieldatenbank anzuzeigen.
   
    ![AdventureWorksDW erkunden][4]

## 2\. Ausführen einer Beispielabfrage
Nachdem jetzt eine Verbindung mit Ihrer Datenbank hergestellt wurde, schreiben wir eine Abfrage.

1. Klicken Sie mit der rechten Maustaste im SQL Server-Objekt-Explorer auf Ihre Datenbank.
2. Wählen Sie **Neue Abfrage** aus. Ein neues Abfragefenster wird geöffnet.
   
    ![Neue Abfrage][5]
3. Kopieren Sie die folgende TSQL-Abfrage in das Abfragefenster:
   
    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```
4. Führen Sie die Abfrage aus. Zu diesem Zweck klicken Sie auf den grünen Pfeil oder verwenden die folgende Tastenkombination: `CTRL`+`SHIFT`+`E`.
   
    ![Abfrage ausführen][6]
5. Sehen Sie sich die Abfrageergebnisse an. In diesem Beispiel weist die Tabelle „FactInternetSales“ 60398 Zeilen auf.
   
    ![Abfrageergebnisse][7]

## Nächste Schritte
Nachdem Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [die Daten mit Power BI zu visualisieren][die Daten mit Power BI zu visualisieren].

Informationen zum Konfigurieren der Umgebung für die Azure Active Directory-Authentifizierung finden Sie unter [Authentifizieren für SQL Data Warehouse][Authentifizieren für SQL Data Warehouse].

<!--Arcticles-->
[Herstellen einer Verbindung mit SQL Data Warehouse]: sql-data-warehouse-connect-overview.md
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installieren von Visual Studio und SSDT]: sql-data-warehouse-install-visual-studio.md
[Authentifizieren für SQL Data Warehouse]: sql-data-warehouse-authentication.md
[die Daten mit Power BI zu visualisieren]: sql-data-warehouse-get-started-visualize-with-power-bi.md

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-query-visual-studio/open-ssdt.png
[2]: media/sql-data-warehouse-query-visual-studio/add-server.png
[3]: media/sql-data-warehouse-query-visual-studio/connection-dialog.png
[4]: media/sql-data-warehouse-query-visual-studio/explore-sample.png
[5]: media/sql-data-warehouse-query-visual-studio/new-query2.png
[6]: media/sql-data-warehouse-query-visual-studio/run-query.png
[7]: media/sql-data-warehouse-query-visual-studio/query-results.png

<!---HONumber=AcomDC_0622_2016-->