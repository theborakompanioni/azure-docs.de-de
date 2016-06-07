<properties
   pageTitle="Erste Schritte: Verbinden mit Azure SQL Data Warehouse | Microsoft Azure"
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
   ms.date="05/16/2016"
   ms.author="mausher;barbkess;sonyama"/>

# Verbinden und Abfragen mit SQLCMD

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)


In dieser exemplarischen Vorgehensweise wird das Verbinden mit und Abfragen einer Azure SQL Data Warehouse-Datenbank veranschaulicht, was mit dem Hilfsprogramm SQLCMD.EXE nur wenige Minuten dauert. In dieser exemplarischen Vorgehensweise führen Sie folgende Aktionen aus:

+ Installieren der erforderlichen Software
+ Verbinden mit einer Datenbank, die die Beispieldatenbank "AdventureWorksDW" enthält
+ Anwenden einer Abfrage auf die Beispieldatenbank  

## Voraussetzungen

+ Informationen zum Herunterladen von [sqlcmd.exe][] finden Sie unter [Microsoft-Befehlszeilenprogramme 11 für SQL Server][].

## Abrufen des vollqualifizierten Namens des SQL Azure-Servers

Um eine Verbindung mit Ihrer Datenbank herzustellen, benötigen Sie den vollständigen Namen des Servers (****Servername**.database.windows.net*), auf dem sich die Datenbank befindet.

1. Öffnen Sie das [Azure-Portal][].
2. Navigieren Sie zu der Datenbank, mit der Sie eine Verbindung herstellen möchten.
3. Suchen Sie den vollständigen Servernamen (der in den folgenden Schritten verwendet wird):

![][1]


## Verbinden mit SQL Data Warehouse per sqlcmd

Für die Verbindung mit einer bestimmten Instanz von SQL Data Warehouse müssen Sie bei Verwendung von SQLCMD die Eingabeaufforderung öffnen und **sqlcmd** gefolgt von der Verbindungszeichenfolge für die SQL Data Warehouse-Datenbank eingeben. Die Verbindungszeichenfolge muss die folgenden Parameter enthalten:

+ **Server (-S)**: Server in Form von `<`Servername`>`.database.windows.net
+ **Datenbank (-D)**: Datenbankname.
+ **Benutzer (-U):** Serverbenutzer im Format `<`Benutzer`>`
+ **Kennwort (-P)**: Das Kennwort des Benutzers.
+ **Bezeichner in Anführungszeichen aktivieren (-I)**: Bezeichner müssen in Anführungszeichen eingeschlossen sein, um die Verbindung mit einer SQL Data Warehouse-Instanz herzustellen.

Zur Verbindung mit einer SQL Data Warehouse-Instanz geben Sie daher Folgendes ein:

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
```

## Durchführen von Beispielabfragen

Nach dem Herstellen der Verbindung können Sie alle unterstützten Transact-SQL-Anweisungen für die Instanz ausgeben.

```sql
C:\>sqlcmd -S <Server Name>.database.windows.net -d <Database> -U <User> -P <Password> -I
1> SELECT name FROM sys.tables;
2> GO
3> QUIT
```

Weitere Informationen zu SQLCMD finden Sie in der [SQLCMD-Dokumentation][sqlcmd.exe].


## Nächste Schritte

Nachdem Sie eine Verbindung hergestellt haben und Abfragen senden können, versuchen Sie, [eine Verbindung mit Power BI herzustellen][].

Informationen zum Konfigurieren Ihrer Umgebung für die Windows-Authentifizierung finden Sie unter [Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung][].

<!--Articles-->
[Herstellen einer Verbindung mit SQL-Datenbank oder SQL Data Warehouse unter Verwendung der Azure Active Directory-Authentifizierung]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[eine Verbindung mit Power BI herzustellen]: ./sql-data-warehouse-integrate-power-bi.md
[Visual Studio]: ./sql-data-warehouse-get-started-connect.md
[SQLCMD]: ./sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other-->
[sqlcmd.exe]: https://msdn.microsoft.com/de-DE/library/ms162773.aspx
[Microsoft-Befehlszeilenprogramme 11 für SQL Server]: http://go.microsoft.com/fwlink/?LinkId=321501
[Azure-Portal]: https://portal.azure.com

<!--Image references-->
[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png

<!---HONumber=AcomDC_0601_2016-->