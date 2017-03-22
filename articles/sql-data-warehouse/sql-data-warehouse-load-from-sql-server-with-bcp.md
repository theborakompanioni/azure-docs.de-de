---
title: Laden von Daten aus SQL Server in Azure SQL Data Warehouse (bcp) | Microsoft Docs
description: "Kleinere Datenmengen können mithilfe von bcp aus SQL Server in Flatfiles exportiert und direkt in Azure SQL Data Warehouse importiert werden."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: f87d8d7c-8276-43c5-90e7-d4ccc0e3a224
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: barbkess
ms.custom: loading
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 8dc7c2fb833c1c51ecef772ba1cbe5f0405fe494
ms.lasthandoff: 01/30/2017


---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-flat-files"></a>Laden von Daten aus SQL Server in Azure SQL Data Warehouse (Flatfiles)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Bei kleinen Datasets können Sie Daten mithilfe des bcp-Befehlszeilenprogramms aus SQL Server exportieren und sie anschließend direkt in Azure SQL Data Warehouse laden.

In diesem Tutorial verwenden Sie bcp für folgende Aktionen:

* Exportieren einer Tabelle aus SQL Server mithilfe des bcp-Befehls „out“ (oder Erstellen einer einfachen Beispieldatei)
* Importieren der Tabelle aus einer Flatfile in SQL Data Warehouse
* Erstellen von Statistiken für die geladenen Daten

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-data-into-Azure-SQL-Data-Warehouse-with-BCP/player]
> 
> 

## <a name="before-you-begin"></a>Voraussetzungen
### <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Eine SQL Data Warehouse-Datenbank
* Eine Installation des Befehlszeilenprogramms bcp
* Eine Installation des Befehlszeilenprogramms sqlcmd

Sie können die Dienstprogramme bcp und sqlcmd aus dem [Microsoft Download Center][Microsoft Download Center] herunterladen.

### <a name="data-in-ascii-or-utf-16-format"></a>Daten im ASCII- oder UTF-16-Format
Wenn Sie in diesem Tutorial Ihre eigenen Daten verwenden möchten, müssen die Daten in ASCII- oder UTF-16-Codierung vorlegen, da UTF-8 von bcp nicht unterstützt wird. 

PolyBase unterstützt zwar UTF-8, aber noch kein UTF-16. Wenn Sie bcp mit PolyBase kombinieren möchten, müssen die aus SQL Server exportierten Daten in das UTF-8-Format umgewandelt werden. 

## <a name="1-create-a-destination-table"></a>1. Erstellen einer Zieltabelle
Definieren Sie in SQL Data Warehouse eine Zieltabelle für die Daten. Die Spalten der Tabelle müssen auf die Daten in den Zeilen der Datendatei abgestimmt sein.

Führen Sie zum Erstellen einer Tabelle an einer Eingabeaufforderung den Befehl „sqlcmd.exe“ aus:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    WITH
    (
        CLUSTERED COLUMNSTORE INDEX,
        DISTRIBUTION = ROUND_ROBIN
    );
"
```


## <a name="2-create-a-source-data-file"></a>2. Erstellen einer Quelldatendatei
Öffnen Sie Editor, kopieren Sie die folgenden Datenzeilen in eine neue Textdatei, und speichern Sie diese Datei im lokalen temporären Verzeichnis C:\Temp\DimDate2.txt. Diese Daten liegen im ASCII-Format vor.

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

(Optional) Wenn Sie Ihre eigenen Daten aus einer SQL Server-Datenbank exportieren möchten, führen Sie an einer Eingabeaufforderung den folgenden Befehl aus. Ersetzen Sie „TableName“, „ServerName“, „DatabaseName“, „Username“ und „Password“ durch Ihre eigenen Informationen.

```sql
bcp <TableName> out C:\Temp\DimDate2_export.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <Password> -q -c -t ','
```



## <a name="3-load-the-data"></a>3. Laden der Daten
Führen Sie zum Laden der Daten an einer Eingabeaufforderung den folgenden Befehl aus, und ersetzen Sie dabei die Werte für Servername, Datenbankname, Benutzername und Kennwort durch Ihre eigenen Informationen:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Vergewissern Sie sich mithilfe des folgenden Befehls, dass die Daten ordnungsgemäß geladen wurden:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Das Ergebnis sollte in etwa wie folgt aussehen:

| DateId | CalendarQuarter | FiscalQuarter |
| --- | --- | --- |
| 20150101 |1 |3 |
| 20150201 |1 |3 |
| 20150301 |1 |3 |
| 20150401 |2 |4 |
| 20150501 |2 |4 |
| 20150601 |2 |4 |
| 20150701 |3 |1 |
| 20150801 |3 |1 |
| 20150801 |3 |1 |
| 20151001 |4 |2 |
| 20151101 |4 |2 |
| 20151201 |4 |2 |

## <a name="4-create-statistics"></a>4. Erstellen von Statistiken
SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder Aktualisierung von Statistiken. Für eine bestmögliche Abfrageleistung ist es wichtig, nach dem ersten Laden oder nach allen wesentlichen Datenänderungen Statistiken für alle Spalten sämtlicher Tabellen zu erstellen. Eine ausführliche Erläuterung von Statistiken finden Sie unter [Statistiken][Statistics]. 

Führen Sie den folgenden Befehl aus, um Statistiken für Ihre neu geladene Tabelle zu erstellen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="5-export-data-from-sql-data-warehouse"></a>5. Exportieren von Daten aus SQL Data Warehouse
Zum Spaß können Sie die soeben geladenen Daten wieder aus SQL Data Warehouse exportieren.  Der Exportbefehl ist der gleiche wie beim Exportieren aus SQL Server.

Die Ergebnisse unterscheiden sich jedoch. Da die Daten an verteilten Standorten in SQL Data Warehouse gespeichert sind, schreibt beim Exportieren jeder Compute-Knoten seine Daten in die Ausgabedatei. In der Ausgabedatei haben die Daten daher wahrscheinlich eine andere Reihenfolge als in der Eingabedatei.

### <a name="export-a-table-and-compare-exported-results"></a>Exportieren einer Tabelle und vergleichen der exportierten Ergebnisse
Führen Sie zum Anzeigen der exportierten Daten an einer Eingabeaufforderung den folgenden Befehl mit Ihren eigenen Parametern aus. „ServerName“ ist der Name Ihrer logischen Azure SQL Server-Instanz.

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, ob die Daten ordnungsgemäß exportiert wurden, indem Sie die neue Datei öffnen. Die Daten in der Datei sollten dem folgenden Text entsprechen, liegen aber wahrscheinlich in einer anderen Reihenfolge vor:

```
20150301,1,3
20150501,2,4
20151001,4,2
20150201,1,3
20151201,4,2
20150801,3,1
20150601,2,4
20151101,4,2
20150401,2,4
20150701,3,1
20150901,3,1
20150101,1,3
```

### <a name="export-the-results-of-a-query"></a>Exportieren der Ergebnisse einer Abfrage
Mithilfe der Funktion **queryout** von bcp können Sie anstelle der gesamten Tabelle die Ergebnisse einer Abfrage exportieren. 

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über das Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][Load data into SQL Data Warehouse].
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][SQL Data Warehouse development overview].
In der [Übersicht über Tabellen][Table Overview] oder unter [CREATE TABLE][CREATE TABLE syntax] finden Sie weitere Informationen zum Erstellen von Tabellen in SQL Data Warehouse.

<!--Image references-->

<!--Article references-->

[Load data into SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
[Table Overview]: ./sql-data-warehouse-tables-overview.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433

