<properties
   pageTitle="Umbenennen in SQL Data Warehouse | Microsoft Azure"
   description="Tipps zum Umbenennen von Tabellen in Azure SQL Data Warehouse für die Entwicklung von Lösungen"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="jrowlandjones"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/28/2016"
   ms.author="mausher;jrj;barbkess;sonyama"/>

# Umbenennen in SQL Data Warehouse
SQL Data Warehouse verwendet die [RENAME][]-Anweisung zum Umbenennen von Tabellen. SQL Server dagegen verwendet `sp_rename`. Derzeit können nur Benutzertabellen umbenannt werden. Datenbanken und externe Tabellen können nicht umbenannt werden.

## Umbenennen von Tabellen

Beim Umbenennen einer Tabelle werden alle einer Tabelle zugeordneten Objekte und Eigenschaften aktualisiert, um auf den neuen Tabellennamen zu verweisen. Beispielsweise werden die Tabellendefinitionen, Indizes, Einschränkungen und Berechtigungen aktualisiert. Ansichten werden nicht aktualisiert.

Zum Umbenennen einer Tabelle wird die folgende Syntax verwendet:

```sql
RENAME OBJECT dbo.Customer TO NewCustomer;
```

## Ändern eines Tabellenschemas

Verwenden Sie ALTER SCHEMA zum Ändern des Schemas, zu dem ein Objekt gehört:

```sql
ALTER SCHEMA dbo TRANSFER OBJECT::product.item;
```

## Für das Umbenennen von Tabellen ist eine exklusive Tabellensperre erforderlich.

Eine Tabelle kann nicht umbenannt werden, während sie verwendet wird. Für das Umbenennen einer Tabelle muss die Tabelle exklusiv gesperrt werden. Wird die Tabelle verwendet, müssen Sie unter Umständen die Sitzung beenden, die die Tabelle verwendet. Verwenden Sie den [KILL][]-Befehl zum Beenden einer Sitzung. Beispiel: `KILL 'SID1234'`. Verwenden Sie den Befehl KILL mit Bedacht, da für jegliche Transaktionsaufgaben, für die noch kein Commit ausgeführt wurde, bei Beendigung der Sitzung ein Rollback ausgeführt wird. Weitere Informationen zu [Sitzungen und Anforderungen][] finden Sie im Artikel zu Verbindungen. Weitere Informationen dazu, wie sich die Beendigung einer Transaktionsabfrage per KILL und ein Rollback auswirken, finden Sie unter [Optimieren von Transaktionen für SQL Data Warehouse][].


## Nächste Schritte
Weitere Hinweise zu T-SQL finden Sie unter [Transact-SQL-Referenzen][].

<!--Image references-->

<!--Article references-->
[development overview]: ./sql-data-warehouse-overview-develop.md
[Sitzungen und Anforderungen]: ./sql-data-warehouse-develop-connections.md#sessions-and-requests
[Transact-SQL-Referenzen]: ./sql-data-warehouse-reference-tsql-statements.md
[Optimieren von Transaktionen für SQL Data Warehouse]: ./sql-data-warehouse-develop-best-practices-transactions.md

<!--MSDN references-->
[KILL]: https://msdn.microsoft.com/library/ms173730.aspx
[RENAME]: https://msdn.microsoft.com/library/mt631611.aspx

<!---HONumber=AcomDC_0601_2016-->