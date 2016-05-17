<properties
   pageTitle="Laden von Daten aus SQL Server in Azure SQL Data Warehouse (PolyBase) | Microsoft Azure"
   description="Verwendet bcp zum Exportieren von Daten aus SQL Server in Flatfiles, AZCopy zum Importieren von Daten in Azure-Blobspeicher und PolyBase zum Erfassen der Daten in Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/08/2016"
   ms.author="lodipalm;barbkess;sonyama"/>


# Laden von Daten aus SQL Server in Azure SQL Data Warehouse (AZCopy)

Verwenden Sie bcp und AZCopy-Befehlszeilenprogramme zum Laden von Daten aus SQL Server in den Azure-Blobspeicher. Verwenden Sie anschließend PolyBase oder Azure Data Factory, um die Daten in Azure SQL Data Warehouse zu laden.


## Voraussetzungen

Für dieses Lernprogramm ist Folgendes erforderlich:

- Eine SQL Data Warehouse-Datenbank
- Eine Installation des bcp-Befehlszeilenprogramms
- Eine Installation des SQLCMD-Befehlszeilenprogramms

>[AZURE.NOTE] Sie können die Dienstprogramme bcp und sqlcmd im [Microsoft Download Center][] herunterladen.

## Importieren von Daten in SQL Data Warehouse

In diesem Lernprogramm erstellen Sie eine Tabelle in Azure SQL Data Warehouse und importieren Daten in die Tabelle.

### Schritt 1: Erstellen einer Tabelle in Azure SQL Data Warehouse

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

>[AZURE.NOTE] Unter [Tabellenentwurf in SQL Data Warehouse][] und [CREATE TABLE][] finden Sie weitere Informationen zum Erstellen von Tabellen in SQL Data Warehouse und den in der WITH-Klausel verfügbaren Optionen.

### Schritt 2: Erstellen einer Quelldatendatei

Öffnen Sie Editor, kopieren Sie die folgenden Datenzeilen in eine neue Textdatei, und speichern Sie diese Datei im lokalen temporären Verzeichnis C:\\Temp\\DimDate2.txt.

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

> [AZURE.NOTE] Es ist wichtig, daran zu denken, dass bcp.exe die UTF-8-Codierung nicht unterstützt. Verwenden Sie ASCII-Dateien oder UTF-16-codierte Dateien, wenn Sie bcp.exe verwenden.

### Schritt 3: Herstellen einer Verbindung und Importieren von Daten
Sie können mit bcp eine Verbindung herstellen und die Daten mit dem folgenden Befehl importieren (ersetzen Sie die Werte nach Bedarf):

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <Server Name> -d <Database Name> -U <Username> -P <password> -q -c -t  ','
```

Sie können prüfen, ob die Daten geladen wurden, indem Sie mithilfe von sqlcmd die folgende Abfrage ausführen:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Daraufhin sollten Sie folgende Ergebnisse erhalten:

DateId |CalendarQuarter |FiscalQuarter
----------- |--------------- |-------------
20150101 |1 |3
20150201 |1 |3
20150301 |1 |3
20150401 |2 |4
20150501 |2 |4
20150601 |2 |4
20150701 |3 |1
20150801 |3 |1
20150801 |3 |1
20151001 |4 |2
20151101 |4 |2
20151201 |4 |2

### Schritt 4: Erstellen von Statistiken für die neu geladenen Daten

Azure SQL Data Warehouse bietet noch keine Unterstützung für die automatische Erstellung oder die automatische Aktualisierung von Statistiken. Um die beste Leistung bei Abfragen zu erhalten, ist es wichtig, dass die Statistiken für alle Spalten aller Tabellen nach dem ersten Laden oder nach allen wesentlichen Datenänderungen erstellt werden. Eine ausführliche Erläuterung der Statistik finden Sie im Thema [Statistiken][] in der Entwicklungsgruppe der Themen. Es folgt ein kurzes Beispiel, wie Sie Statistiken für die in diesem Beispiel geladene Tabelle erstellen können.

Führen Sie die folgenden CREATE STATISTICS-Anweisungen über eine sqlcmd-Eingabeaufforderung aus:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    create statistics [DateId] on [DimDate2] ([DateId]);
    create statistics [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
    create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
"
```

## Exportieren von Daten aus SQL Data Warehouse
In diesem Lernprogramm erstellen Sie eine Datendatei aus einer Tabelle in SQL Data Warehouse. Wir werden die oben erstellten Daten in eine neue Datendatei namens "DimDate2\_export.txt" exportieren.

### Schritt 1: Exportieren der Daten

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

>[AZURE.NOTE] Aufgrund der Struktur verteilter Systeme unterscheidet sich die Reihenfolge der Daten in den SQL Data Warehouse-Datenbanken möglicherweise. Eine andere Möglichkeit ist die Verwendung der Funktion **queryout** von bcp, um einen Auszug der Abfrage zu schreiben, und nicht die gesamte Tabelle zu exportieren.

## Nächste Schritte
Eine Übersicht über das Laden finden Sie unter [Laden von Daten in SQL Data Warehouse][]. Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->

[Laden von Daten in SQL Data Warehouse]: sql-data-warehouse-overview-load.md
[Entwicklungsübersicht für SQL Data Warehouse]: sql-data-warehouse-overview-develop.md
[Tabellenentwurf in SQL Data Warehouse]: sql-data-warehouse-develop-table-design.md
[Statistiken]: sql-data-warehouse-develop-statistics.md

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433

<!---HONumber=AcomDC_0511_2016-->