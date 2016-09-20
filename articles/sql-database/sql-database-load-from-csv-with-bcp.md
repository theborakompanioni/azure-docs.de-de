<properties
   pageTitle="Laden von Daten aus einer CSV-Datei in Azure SQL-Datenbank (BCP) | Microsoft Azure"
   description="Für kleinere Datenmengen wird BCP zum Importieren von Daten in Azure SQL-Datenbank verwendet."
   services="sql-database"
   documentationCenter="NA"
   authors="CarlRabeler"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/13/2016"
   ms.author="carlrab"/>


# Laden von Daten aus einer CSV-Datei in Azure SQL Data Warehouse (Flatfiles)

Sie können das Befehlszeilenprogramm BCP verwenden, um Daten aus einer CSV-Datei in Azure SQL-Datenbank zu importieren.

## Voraussetzungen

### Voraussetzungen

Für dieses Lernprogramm ist Folgendes erforderlich:

- Ein logischer Azure SQL-Datenbankserver und eine Datenbank
- Eine Installation des Befehlszeilenprogramms bcp
- Eine Installation des Befehlszeilenprogramms sqlcmd

Sie können die Dienstprogramme bcp und sqlcmd im [Microsoft Download Center][] herunterladen.

### Daten im ASCII- oder UTF-16-Format

Wenn Sie in diesem Tutorial Ihre eigenen Daten verwenden möchten, müssen die Daten in ASCII- oder UTF-16-Codierung vorlegen, da UTF-8 von bcp nicht unterstützt wird.

## 1\. Erstellen einer Zieltabelle

Definieren Sie eine Tabelle in SQL-Datenbank als Zieltabelle. Die Spalten der Tabelle müssen auf die Daten in den Zeilen der Datendatei abgestimmt sein.

Führen Sie zum Erstellen einer Tabelle an einer Eingabeaufforderung den Befehl „sqlcmd.exe“ aus:


```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "
    CREATE TABLE DimDate2
    (
        DateId INT NOT NULL,
        CalendarQuarter TINYINT NOT NULL,
        FiscalQuarter TINYINT NOT NULL
    )
    ;
"
```


## 2\. Erstellen einer Quelldatendatei

Öffnen Sie Editor, kopieren Sie die folgenden Datenzeilen in eine neue Textdatei, und speichern Sie diese Datei im lokalen temporären Verzeichnis C:\\Temp\\DimDate2.txt. Diese Daten liegen im ASCII-Format vor.

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

## 3\. Laden der Daten
Führen Sie zum Laden der Daten an einer Eingabeaufforderung den folgenden Befehl aus, und ersetzen Sie dabei die Werte für Servername, Datenbankname, Benutzername und Kennwort durch Ihre eigenen Informationen:

```sql
bcp DimDate2 in C:\Temp\DimDate2.txt -S <ServerName> -d <DatabaseName> -U <Username> -P <password> -q -c -t  ','
```

Vergewissern Sie sich mithilfe des folgenden Befehls, dass die Daten ordnungsgemäß geladen wurden:

```sql
sqlcmd.exe -S <server name> -d <database name> -U <username> -P <password> -I -Q "SELECT * FROM DimDate2 ORDER BY 1;"
```

Das Ergebnis sollte in etwa wie folgt aussehen:

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


## Nächste Schritte

Informationen zum Migrieren einer SQL Server-Datenbank finden Sie im Artikel zur [SQL Server-Datenbankmigration](sql-database-cloud-migrate.md).

<!--MSDN references-->
[bcp]: https://msdn.microsoft.com/library/ms162802.aspx
[CREATE TABLE syntax]: https://msdn.microsoft.com/library/mt203953.aspx

<!--Other Web references-->
[Microsoft Download Center]: https://www.microsoft.com/download/details.aspx?id=36433

<!---HONumber=AcomDC_0914_2016-->