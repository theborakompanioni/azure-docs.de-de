<properties
   pageTitle="Verbinden mit Azure SQL Data Warehouse | Microsoft Azure"
   description="Enthält eine Übersicht über das Herstellen einer Verbindung mit Azure SQL Data Warehouse."
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
   ms.date="06/20/2016"
   ms.author="sonyama;barbkess"/>

# Verbinden mit Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-connect-overview.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Treiber](sql-data-warehouse-connection-strings.md)

Enthält eine Übersicht über das Verbinden mit Azure SQL Data Warehouse.

## Ermitteln der Verbindungszeichenfolge über das Portal

Ihre SQL Data Warehouse-Instanz ist einem Azure SQL-Server zugeordnet. Um eine Verbindung herzustellen, benötigen Sie den vollqualifizierten Namen des Servers (**Servername**.database.windows.net*).

So ermitteln Sie den vollqualifizierten Servernamen

1. Öffnen Sie das [Azure-Portal][].
2. Klicken Sie auf **SQL-Datenbanken**, und klicken Sie auf die Datenbank, mit der Sie eine Verbindung herstellen möchten. In diesem Beispiel wird die AdventureWorksDW-Beispieldatenbank verwendet.
3. Suchen Sie den vollständigen Servernamen.

    ![Vollständiger Servername][1]

## Verbindungseinstellungen
In SQL Data Warehouse sind einige Einstellungen beim Herstellen der Verbindung und Erstellen von Objekten standardisiert. Diese können nicht außer Kraft gesetzt werden.

| Datenbankeinstellung | Wert |
| :----------------- | :--------------------------- |
| ANSI\_NULLS | EIN |
| QUOTED\_IDENTIFIERS | EIN |
| NO\_COUNT | AUS |
| DATEFORMAT | mdy |
| DATEFIRST | 7 |
| Datenbanksortierung | SQL\_Latin1\_General\_CP1\_CI\_AS |

## Sitzungen und Anforderungen
Nachdem eine Verbindung hergestellt und eine Sitzung eingerichtet wurde, können Sie Abfragen schreiben und an SQL Data Warehouse senden.

Jede Abfrage wird mit einem oder mehreren Anforderungsbezeichnern dargestellt. Alle Abfragen, die über diese Verbindung übermittelt werden, sind Teil einer einzelnen Sitzung und werden daher durch eine einzelne Sitzungs-ID dargestellt.

Da SQL Data Warehouse jedoch ein verteiltes MPP-System ist (Massively Parallel Processing), werden Sitzungs- und Anforderungsbezeichner im Vergleich zu SQL Server etwas anders verfügbar gemacht.

Sitzungen und Anforderungen werden logisch durch ihre jeweiligen Bezeichner dargestellt.

| Bezeichner | Beispielwert |
| :--------- | :------------ |
| Sitzungs-ID | SID123456 |
| Anforderungs-ID | QID123456 |

Beachten Sie, dass die Sitzungs-ID über das Präfix SID verfügt (Abkürzung von „Session ID“) und dass die Anforderungen über das Präfix QID verfügen (Abkürzung von „Query ID“).

Sie benötigen diese Informationen beim Identifizieren der Abfrage, wenn Sie Ihre Abfrageleistung überwachen. Sie können Ihre Abfrageleistung überwachen, indem Sie das [Azure-Portal] und die dynamischen Verwaltungssichten verwenden.

Mit dieser Abfrage wird die aktuelle Sitzung identifiziert.

```sql
SELECT SESSION_ID()
;
```

Zum Anzeigen aller Abfragen, die entweder derzeit ausgeführt werden oder vor Kurzem für Ihr Data Warehouse ausgeführt wurden, können Sie das folgende Beispiel verwenden. Es wird eine Sicht erstellt, und anschließend wird die Sicht ausgeführt.

```sql
CREATE VIEW dbo.vSessionRequests
AS
SELECT 	 s.[session_id]									AS Session_ID
		,s.[status]										AS Session_Status
		,s.[login_name]									AS Session_LoginName
		,s.[login_time]									AS Session_LoginTime
        ,r.[request_id]									AS Request_ID
		,r.[status]										AS Request_Status
		,r.[submit_time]								AS Request_SubmitTime
		,r.[start_time]									AS Request_StartTime
		,r.[end_compile_time]							AS Request_EndCompileTime
		,r.[end_time]									AS Request_EndTime
		,r.[total_elapsed_time]							AS Request_TotalElapsedDuration_ms
        ,DATEDIFF(ms,[submit_time],[start_time])		AS Request_InitiateDuration_ms
        ,DATEDIFF(ms,[start_time],[end_compile_time])	AS Request_CompileDuration_ms
        ,DATEDIFF(ms,[end_compile_time],[end_time])		AS Request_ExecDuration_ms
		,[label]										AS Request_QueryLabel
		,[command]										AS Request_Command
		,[database_id]									AS Request_Database_ID
FROM    sys.dm_pdw_exec_requests r
JOIN    sys.dm_pdw_exec_sessions s	ON	r.[session_id] = s.[session_id]
WHERE   s.[session_id] <> SESSION_ID()
;

SELECT * FROM dbo.vSessionRequests;
```

## Nächste Schritte

Informationen zum Abfragen Ihres Data Warehouse mit Visual Studio und anderen Anwendungen finden Sie unter [Abfragen mit Visual Studio][].


<!--Arcticles-->

[Abfragen mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Azure-Portal]: https://portal.azure.com

<!--Image references-->

[1]: media/sql-data-warehouse-connect-overview/get-server-name.png

<!---HONumber=AcomDC_0622_2016-->