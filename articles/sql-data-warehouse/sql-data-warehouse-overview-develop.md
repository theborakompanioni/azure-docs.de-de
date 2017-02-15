---
title: "Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse-Entwicklung | Microsoft Docs"
description: "Entwicklungskonzepte, Entwurfsentscheidungen, Empfehlungen und Programmiertechniken für SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: d1c2255264a4240eecc51199050f8406d141ace2


---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse
Lesen Sie diese Entwicklungsartikel, um ein besseres Verständnis der wesentlichen Entwurfsentscheidungen, der Empfehlungen und der Programmiertechniken für SQL Data Warehouse zu erlangen.

## <a name="key-design-decisions"></a>Wesentliche Entwurfsentscheidungen
In den folgenden Artikeln werden einige der zentralen Konzepte und Entwurfsentscheidungen hervorgehoben, die Sie für die Entwicklung Ihres verteilten Data Warehouses mit SQL Data Warehouse verstehen müssen:

* [Verbindungen][Verbindungen]
* [Parallelität][Parallelität]
* [Transaktionen][Transaktionen]
* [Benutzerdefinierte Schemas][Benutzerdefinierte Schemas]
* [Tabellenverteilung][Tabellenverteilung]
* [Tabellenindizes][Tabellenindizes]
* [Tabellenpartitionen][Tabellenpartitionen]
* [CTAS][CTAS]
* [Statistiken][Statistiken]

## <a name="development-recommendations-and-coding-techniques"></a>Entwicklungsempfehlungen und Programmiertechniken
In diesen Artikeln werden bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung eines SQL Data Warehouse behandelt:

* [Gespeicherte Prozeduren][Gespeicherte Prozeduren]
* [Bezeichnungen][Bezeichnungen]
* [Ansichten][Ansichten]
* [Temporäre Tabellen][Temporäre Tabellen]
* [Dynamisches SQL][Dynamisches SQL]
* [Schleifen][Schleifen]
* [Gruppierungsoptionen][Gruppierungsoptionen]
* [Variablenzuweisung][Variablenzuweisung]

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Entwicklungsartikel durchgearbeitet haben, finden Sie auf der Seite [Transact-SQL-Referenz][Transact-SQL-Referenz] weitere Informationen zur unterstützten Syntax für SQL Data Warehouse.

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
[Umbenennen von Objekten]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->



<!--HONumber=Nov16_HO3-->


