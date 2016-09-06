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
   ms.date="08/27/2016"
   ms.author="sonyama;barbkess"/>

# Verbinden mit Azure SQL Data Warehouse

> [AZURE.SELECTOR]
- [Übersicht](sql-data-warehouse-connect-overview.md)
- [Authentifizierung](sql-data-warehouse-authentication.md)
- [Treiber](sql-data-warehouse-connection-strings.md)

Enthält eine Übersicht über das Verbinden mit Azure SQL Data Warehouse.

## Ermitteln der Verbindungszeichenfolge über das Portal

Ihre SQL Data Warehouse-Instanz ist einem Azure SQL-Server zugeordnet. Um eine Verbindung herzustellen, benötigen Sie den vollqualifizierten Namen des Servers. Z.B. **meinserver**.database.windows.net.

So ermitteln Sie den vollqualifizierten Servernamen

1. Öffnen Sie das [Azure-Portal][].
2. Klicken Sie auf **SQL-Datenbanken**, und klicken Sie auf die Datenbank, mit der Sie eine Verbindung herstellen möchten. In diesem Beispiel wird die AdventureWorksDW-Beispieldatenbank verwendet.
3. Suchen Sie den vollständigen Servernamen.

    ![Vollständiger Servername][1]

## Verbindungseinstellungen

In SQL Data Warehouse sind einige Einstellungen beim Herstellen der Verbindung und Erstellen von Objekten standardisiert. Diese können nicht außer Kraft gesetzt werden.

| Datenbankeinstellung | Wert |
| :--------------------- | :--------------------------- |
| [ANSI\_NULLS][] | EIN |
| [QUOTED\_IDENTIFIERS][] | EIN |
| [DATEFORMAT][] | mdy |
| [DATEFIRST][] | 7 |

## Überwachen von Verbindungen und Abfragen

Nachdem eine Verbindung hergestellt und eine Sitzung eingerichtet wurde, können Sie Abfragen schreiben und an SQL Data Warehouse senden. Informationen zum Überwachen von Sitzungen und Abfragen finden Sie unter [Überwachen Ihres Workloads mit dynamischen Verwaltungssichten][].

## Nächste Schritte

Informationen zum Abfragen Ihres Data Warehouse mit Visual Studio und anderen Anwendungen finden Sie unter [Abfragen mit Visual Studio][].

<!--Articles-->
[Abfragen mit Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Überwachen Ihres Workloads mit dynamischen Verwaltungssichten]: ./sql-data-warehouse-manage-monitor.md

<!--MSDN references-->
[ANSI\_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED\_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure-Portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png

<!---HONumber=AcomDC_0831_2016-->