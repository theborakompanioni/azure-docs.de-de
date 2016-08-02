<properties
   pageTitle="Abfragen von Azure SQL Data Warehouse (sqlcmd) | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Azure SQL Data Warehouse mit dem Befehlszeilenprogramm sqlcmd abfragen."
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
   ms.date="07/22/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Abfragen von Azure SQL Data Warehouse (sqlcmd)

> [AZURE.SELECTOR]
- [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
- [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
- [Visual Studio](sql-data-warehouse-query-visual-studio.md)
- [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)

In dieser exemplarischen Vorgehensweise wird das Befehlszeilenprogramm sqlcmd verwendet, um ein Azure SQL Data Warehouse abzufragen.

## Voraussetzungen

Für dieses Lernprogramm ist Folgendes erforderlich:

-  [sqlcmd.exe][]. Informationen zum Herunterladen finden Sie unter [Microsoft Befehlszeilenprogramme 11 für SQL Server][]. Dafür ist unter Umständen auch [Microsoft ODBC-Treiber 11 für SQL Server Windows][] erforderlich.

## 1\. Verbinden

Öffnen Sie zur Nutzung von sqlcmd zuerst die Befehlseingabeaufforderung, und geben Sie **sqlcmd** gefolgt von der Verbindungszeichenfolge für Ihre SQL Data Warehouse-Datenbank ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

+ **Server (-S)**: Server in Form von `<`Servername`>`.database.windows.net
+ **Datenbank (-D)**: Datenbankname.
+ **Benutzer (-U):** Serverbenutzer im Format `<`Benutzer`>`
+ **Kennwort (-P)**: Das Kennwort des Benutzers.
+ **Bezeichner in Anführungszeichen aktivieren (-I)**: Bezeichner müssen in Anführungszeichen eingeschlossen sein, um die Verbindung mit einer SQL Data Warehouse-Instanz herzustellen.

Die Verbindungszeichenfolge kann beispielsweise wie folgt aussehen:

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
```

> [AZURE.NOTE] Die Option „-I“, mit der Bezeichner in Anführungszeichen aktiviert werden, ist derzeit erforderlich, um eine Verbindung mit SQL Data Warehouse herzustellen.

## 2\. Abfrage

Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben. In diesem Beispiel werden Abfragen im interaktiven Modus gesendet.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Die nächsten Beispiele zeigen, wie Sie Ihre Abfragen im Batchmodus ausführen können, indem Sie die Option „-Q“ oder für den SQL-Code ein Pipe-Zeichen zur Verknüpfung mit „sqlcmd“ verwenden.

```sql
sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Nächste Schritte

Ausführliche Informationen zu den in sqlcmd verfügbaren Optionen finden Sie in der [Dokumentation zu sqlcmd][sqlcmd.exe].

<!--Image references-->

<!--Article references-->

<!--MSDN references--> 
[sqlcmd.exe]: https://msdn.microsoft.com/library/ms162773.aspx
[Microsoft ODBC-Treiber 11 für SQL Server Windows]: https://www.microsoft.com/download/details.aspx?id=36434
[Microsoft Befehlszeilenprogramme 11 für SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure portal]: https://portal.azure.com

<!--Other Web references-->

<!---HONumber=AcomDC_0727_2016-->