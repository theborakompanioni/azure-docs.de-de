<properties
   pageTitle="Herstellen einer Verbindung mit SQL Data Warehouse über Visual Studio | Microsoft Azure"
   description="Erste Schritte zum Herstellen von Verbindungen mit SQL Data Warehouse für das Ausführen von Abfragen."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/13/2016"
   ms.author="sonyama;barbkess"/>

# Herstellen einer Verbindung mit SQL Data Warehouse über Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)

In dieser exemplarischen Vorgehensweise wird das Verbinden mit einer Azure SQL Data Warehouse-Instanz veranschaulicht, das mit der SQL Server Data Tools-Erweiterung (SSDT) in Visual Studio nur wenige Minuten dauert. Nachdem die Verbindung hergestellt wurde, führen Sie eine einfache Abfrage aus.

## Voraussetzungen

+ AdventureWorksDW-Beispieldaten in SQL Data Warehouse. Informationen zum Erstellen dieser Daten finden Sie unter [Erstellen eines SQL Data Warehouse][].
+ SQL Server Data Tools für Visual Studio. Installationshinweise und -optionen finden Sie unter [Installieren von Visual Studio und SSDT][].

## Schritt 1: Ermitteln des vollqualifizierten Namens des SQL Azure-Servers

Ihre SQL Data Warehouse-Datenbank ist einem Azure SQL Server zugeordnet. Um eine Verbindung mit Ihrer Datenbank herzustellen, benötigen Sie den vollqualifizierten Namen des Servers (**Servername**.database.windows.net*).

So ermitteln Sie den vollqualifizierten Servernamen:

1. Öffnen Sie das [Azure-Portal][].
2. Klicken Sie auf **SQL-Datenbanken**, und klicken Sie auf die Datenbank, mit der Sie eine Verbindung herstellen möchten. In diesem Beispiel wird die AdventureWorksDW-Beispieldatenbank verwendet.
3. Suchen Sie den vollständigen Servernamen.

    ![Vollständiger Servername][1]

## Schritt 2: Herstellen einer Verbindung mit SQL Data Warehouse

1. Öffnen Sie Visual Studio 2013 oder 2015.
2. Öffnen Sie den SQL Server-Objekt-Explorer. Wählen Sie zu diesem Zweck **Ansicht** > **SQL Server-Objekt-Explorer** aus.

    ![SQL Server-Objekt-Explorer][2]

3. Klicken Sie auf das Symbol **SQL Server hinzufügen**.

    ![SQL Server-Instanz hinzufügen][3]

4. Füllen Sie die Felder im Fenster zum Herstellen einer Verbindung mit dem Server aus.

    ![Verbindung mit dem Server herstellen][4]

    - **Servername**. Geben Sie den zuvor ermittelten **Servernamen** ein.
    - **Authentifizierung**. Wählen Sie **SQL Server-Authentifizierung** oder **Integrierte Active Directory-Authentifizierung**.
    - **Benutzername** und **Kennwort**. Geben Sie Benutzername und Kennwort ein, wenn Sie oben „SQL Server-Authentifizierung“ ausgewählt haben.
    - Klicken Sie auf **Verbinden**.

5. Erweitern Sie den Azure SQL-Server. Sie können die dem Server zugeordneten Datenbanken anzeigen. Erweitern Sie „AdventureWorksDW“, um die Tabellen in Ihrer Beispieldatenbank anzuzeigen.

    ![AdventureWorksDW erkunden][5]

## Schritt 3: Ausführen einer Beispielabfrage

Nachdem jetzt eine Verbindung mit Ihrer Datenbank hergestellt wurde, schreiben wir eine Abfrage.

1. Klicken Sie mit der rechten Maustaste im SQL Server-Objekt-Explorer auf Ihre Datenbank.

2. Wählen Sie **Neue Abfrage** aus. Ein neues Abfragefenster wird geöffnet.

    ![Neue Abfrage][6]

3. Kopieren Sie die folgende TSQL-Abfrage in das Abfragefenster:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Führen Sie die Abfrage aus. Zu diesem Zweck klicken Sie auf den grünen Pfeil oder verwenden die folgende Tastenkombination: `CTRL`+`SHIFT`+`E`.

    ![Abfrage ausführen][7]

5. Sehen Sie sich die Abfrageergebnisse an. In diesem Beispiel weist die Tabelle „FactInternetSales“ 60398 Zeilen auf.

    ![Abfrageergebnisse][8]

## Nächste Schritte

Nachdem Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [die Daten mit Power BI zu visualisieren][].

Informationen zum Konfigurieren Ihrer Umgebung für die Windows-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung][].

<!--Arcticles-->
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Installieren von Visual Studio und SSDT]: sql-data-warehouse-install-visual-studio.md
[Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[die Daten mit Power BI zu visualisieren]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md

<!--Other-->
[Azure-Portal]: https://portal.azure.com

<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/add-server.png
[4]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[5]: ./media/sql-data-warehouse-get-started-connect/explore-sample.png
[6]: ./media/sql-data-warehouse-get-started-connect/new-query2.png
[7]: ./media/sql-data-warehouse-get-started-connect/run-query.png
[8]: ./media/sql-data-warehouse-get-started-connect/query-results.png

<!---HONumber=AcomDC_0601_2016-->