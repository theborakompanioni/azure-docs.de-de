---
title: "Ressourcen für die Entwicklung eines Data Warehouse in Azure | Microsoft-Dokumentation"
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
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 2548f779767635865daf790d301d86feff573a29
ms.openlocfilehash: 37344b7916d8ceb2ad3b6a34df9fc8681af4dff7
ms.lasthandoff: 01/24/2017


---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Entwurfsentscheidungen und Programmiertechniken für SQL Data Warehouse
Lesen Sie diese Entwicklungsartikel, um ein besseres Verständnis der wesentlichen Entwurfsentscheidungen, der Empfehlungen und der Programmiertechniken für SQL Data Warehouse zu erlangen.

## <a name="key-design-decisions"></a>Wesentliche Entwurfsentscheidungen
In den folgenden Artikeln werden einige der zentralen Konzepte und Entwurfsentscheidungen hervorgehoben, die Sie für die Entwicklung Ihres verteilten Data Warehouses mit SQL Data Warehouse verstehen müssen:

* [Verbindungen][connections]
* [Parallelität][concurrency]
* [Transaktionen][transactions]
* [Benutzerdefinierte Schemas][user-defined schemas]
* [Tabellenverteilung][table distribution]
* [Tabellenindizes][table indexes]
* [Tabellenpartitionen][table partitions]
* [CTAS][CTAS]
* [Statistiken][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Entwicklungsempfehlungen und Programmiertechniken
In diesen Artikeln werden bestimmte Programmiertechniken, Tipps und Empfehlungen für die Entwicklung eines SQL Data Warehouse behandelt:

* [Gespeicherten Prozeduren][stored procedures]
* [Bezeichnungen][labels]
* [Ansichten][views]
* [Temporäre Tabellen][temporary tables]
* [Dynamischer SQL-Code][dynamic SQL]
* [Schleifen][looping]
* [Gruppierungsoptionen][group by options]
* [Variablenzuweisung][variable assignment]

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie die Entwicklungsartikel durchgearbeitet haben, finden Sie auf der Seite [Transact-SQL-Referenz][Transact-SQL reference] weitere Informationen zur unterstützten Syntax für SQL Data Warehouse.

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->

