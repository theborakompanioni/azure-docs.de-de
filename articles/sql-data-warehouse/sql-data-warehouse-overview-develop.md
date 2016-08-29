<properties
   pageTitle="Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse-Entwicklung | Microsoft Azure"
   description="Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für SQL Data Warehouse."
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
   ms.date="08/16/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse

Lesen Sie diese Entwicklungsartikel, um ein besseres Verständnis der wesentlichen Entwurfsentscheidungen, der Empfehlungen und der Programmiertechniken für SQL Data Warehouse zu erlangen.

## Wesentliche Entwurfsentscheidungen
In den folgenden Artikeln werden einige der zentralen Konzepte und Entwurfsentscheidungen hervorgehoben, die Sie für die Entwicklung Ihres verteilten Data Warehouses mit SQL Data Warehouse verstehen müssen:

- [Verbindungen][]
- [Parallelität][]
- [Transaktionen][]
- [Benutzerdefinierte Schemas][]
- [Tabellenverteilung][]
- [Tabellenindizes][]
- [Tabellenpartitionen][]
- [CTAS][]
- [Statistiken][]

## Entwicklungsempfehlungen und Programmiertechniken
In diesen Artikeln werden bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung eines SQL Data Warehouse behandelt:

- [Gespeicherte Prozeduren][]
- [Bezeichnungen][]
- [Ansichten][]
- [Temporäre Tabellen][]
- [Dynamisches SQL][]
- [Schleifen][]
- [Gruppierungsoptionen][]
- [Variablenzuweisung][]

## Nächste Schritte
Wenn Sie die Entwicklungsartikel durchgearbeitet haben, finden Sie auf der Seite [Transact-SQL-Referenz][] weitere Informationen zur unterstützten Syntax für SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[Parallelität]: ./sql-data-warehouse-develop-concurrency.md
[Verbindungen]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[Dynamisches SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[Gruppierungsoptionen]: ./sql-data-warehouse-develop-group-by-options.md
[Bezeichnungen]: ./sql-data-warehouse-develop-label.md
[Schleifen]: ./sql-data-warehouse-develop-loops.md
[Statistiken]: ./sql-data-warehouse-tables-statistics.md
[Gespeicherte Prozeduren]: ./sql-data-warehouse-develop-stored-procedures.md
[Tabellenverteilung]: ./sql-data-warehouse-tables-distribute.md
[Tabellenindizes]: ./sql-data-warehouse-tables-index.md
[Tabellenpartitionen]: ./sql-data-warehouse-tables-partition.md
[Temporäre Tabellen]: ./sql-data-warehouse-tables-temporary.md
[Transaktionen]: ./sql-data-warehouse-develop-transactions.md
[Benutzerdefinierte Schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[Variablenzuweisung]: ./sql-data-warehouse-develop-variable-assignment.md
[Ansichten]: ./sql-data-warehouse-develop-views.md
[Transact-SQL-Referenz]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0817_2016-->