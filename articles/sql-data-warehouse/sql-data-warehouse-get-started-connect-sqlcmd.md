<properties
   pageTitle="Abfragen mit SQLCMD | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Data Warehouse mit SQLCMD abfragen."
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
   ms.date="06/09/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Abfragen mit SQLCMD

> [AZURE.SELECTOR]
- [Power BI][]
- [Azure Machine Learning][]
- [SQLCMD][]

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie Azure SQL Data Warehouse mit dem Hilfsprogramm „sqlcmd.exe“ abfragen.

## Voraussetzungen

+ Informationen zum Herunterladen von [sqlcmd.exe][] finden Sie unter [Microsoft-Befehlszeilenprogramme 11 für SQL Server][].

## Verbinden

Öffnen Sie zur Nutzung von „sqlcmd“ zuerst die Befehlseingabeaufforderung, und geben Sie **sqlcmd** gefolgt von der Verbindungszeichenfolge für Ihre SQL Data Warehouse-Datenbank ein. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

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

## Abfrage

Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben. In diesem Beispiel werden Abfragen im interaktiven Modus gesendet.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Die nächsten Beispiele zeigen, wie Sie Ihre Abfragen im Batchmodus ausführen können, indem Sie die Option „-Q“ oder für den SQL-Code ein Pipe-Zeichen zur Verknüpfung mit „sqlcmd“ verwenden.

```sql
C:\>sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I -Q "SELECT name FROM sys.tables;"
```

```sql
C:\>"SELECT name FROM sys.tables;" | sqlcmd -S MySqlDw.database.windows.net -d Adventure_Works -U myuser -P myP@ssword -I > .\tables.out
```

## Nächste Schritte

Weitere Informationen zu allen sqlcmd-Optionen finden Sie in der [sqlcmd-Dokumentation][sqlcmd.exe].

<!--Articles-->
[connecting with PowerBI]: ./sql-data-warehouse-integrate-power-bi.md
[Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Azure Machine Learning]: ./sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md
[SQLCMD]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/de-DE/library/ms162773.aspx
[Microsoft-Befehlszeilenprogramme 11 für SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure portal]: https://portal.azure.com

<!--Image references-->

<!---HONumber=AcomDC_0615_2016-->