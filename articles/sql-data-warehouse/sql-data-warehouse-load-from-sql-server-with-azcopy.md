---
title: Laden von Daten aus SQL Server in Azure SQL Data Warehouse (PolyBase) | Microsoft Docs
description: Verwendet BCP zum Exportieren von Daten aus SQL Server in Flatfiles, AZCopy zum Importieren von Daten in Azure-Blobspeicher und PolyBase zum Erfassen der Daten in Azure SQL Data Warehouse.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: barbkess
editor: 
ms.assetid: 4d42786a-fb28-43c9-9c3b-72d19c0ecc11
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 21b4cc704e271ac220fd606305f8f97c9b2593bb
ms.lasthandoff: 03/22/2017



---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-azcopy"></a>Laden von Daten aus SQL Server in Azure SQL Data Warehouse (AZCopy)
Verwenden Sie bcp und AZCopy-Befehlszeilenprogramme zum Laden von Daten aus SQL Server in den Azure-Blobspeicher. Verwenden Sie anschließend PolyBase oder Azure Data Factory, um die Daten in Azure SQL Data Warehouse zu laden. 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Eine SQL Data Warehouse-Datenbank
* Eine Installation des bcp-Befehlszeilenprogramms
* Eine Installation des SQLCMD-Befehlszeilenprogramms

> [!NOTE]
> Sie können die Dienstprogramme bcp und sqlcmd aus dem [Microsoft Download Center][Microsoft Download Center] herunterladen.
> 
> 

## <a name="import-data-into-sql-data-warehouse"></a>Importieren von Daten in SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Tabelle in Azure SQL Data Warehouse und importieren Daten in die Tabelle.

### <a name="step-1-create-a-table-in-azure-sql-data-warehouse"></a>Schritt 1: Erstellen einer Tabelle in Azure SQL Data Warehouse
Verwenden Sie sqlcmd von einer Befehlszeile aus zum Ausführen der folgenden Abfrage, um eine Tabelle für Ihre Instanz zu erstellen:

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

> [!NOTE]
> In der [Übersicht über Tabellen][Table Overview] und unter [CREATE TABLE][CREATE TABLE syntax] finden Sie weitere Informationen zum Erstellen von Tabellen in SQL Data Warehouse sowie zu den verfügbaren Optionen der WITH-Klausel.
> 
> 

### <a name="step-2-create-a-source-data-file"></a>Schritt 2: Erstellen einer Quelldatendatei
Öffnen Sie Editor, kopieren Sie die folgenden Datenzeilen in eine neue Textdatei, und speichern Sie diese Datei im lokalen temporären Verzeichnis C:\Temp\DimDate2.txt.

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

> [!NOTE]
> Es ist wichtig, daran zu denken, dass bcp.exe die UTF-8-Codierung nicht unterstützt. Verwenden Sie ASCII-Dateien oder UTF-16-codierte Dateien, wenn Sie bcp.exe verwenden.
> 
> 

### <a name="step-3-connect-and-import-the-data"></a>Schritt 3: Herstellen einer Verbindung und Importieren von Daten
Sie können mit bcp eine Verbindung herstellen und die Daten mit dem folgenden Befehl importieren (ersetzen Sie die Werte nach Bedarf):

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Sie können prüfen, ob die Daten geladen wurden, indem Sie mithilfe von sqlcmd die folgende Abfrage ausführen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Daraufhin sollten Sie folgende Ergebnisse erhalten:

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

### <a name="step-4-create-statistics-on-your-newly-loaded-data"></a>Schritt 4: Erstellen von Statistiken für die neu geladenen Daten
Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken. Um die beste Leistung bei Abfragen zu erhalten, ist es wichtig, dass die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellt werden. Eine ausführliche Erläuterung der Statistik finden Sie im Thema [Statistiken][Statistics] in der Entwicklungsgruppe der Themen. Es folgt ein kurzes Beispiel, wie Sie Statistiken für die in diesem Beispiel geladene Tabelle erstellen können.

Führen Sie die folgenden CREATE STATISTICS-Anweisungen über eine sqlcmd-Eingabeaufforderung aus:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## <a name="export-data-from-sql-data-warehouse"></a>Exportieren von Daten aus SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Datendatei aus einer Tabelle in SQL Data Warehouse. Wir werden die oben erstellten Daten in eine neue Datendatei namens "DimDate2_export.txt" exportieren.

### <a name="step-1-export-the-data"></a>Schritt 1: Exportieren der Daten
Sie können mit dem Dienstprogramm bcp eine Verbindung herstellen und die Daten mit dem folgenden Befehl exportieren (ersetzen Sie die Werte nach Bedarf):

```sql
bcp DimDate2 out C:\Temp\DimDate2_export.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t ','
```
Sie können überprüfen, ob die Daten ordnungsgemäß exportiert wurden, indem Sie die neue Datei öffnen. Die Daten in der Datei sollten mit folgendem Text übereinstimmen:

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

> [!NOTE]
> Aufgrund der Struktur verteilter Systeme unterscheidet sich die Reihenfolge der Daten in den SQL Data Warehouse-Datenbanken möglicherweise. Eine andere Möglichkeit ist die Verwendung der Funktion **queryout** von bcp, um einen Auszug der Abfrage zu schreiben, und nicht die gesamte Tabelle zu exportieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Eine Übersicht über das Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][Load data into SQL Data Warehouse].
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][SQL Data Warehouse development overview].

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

