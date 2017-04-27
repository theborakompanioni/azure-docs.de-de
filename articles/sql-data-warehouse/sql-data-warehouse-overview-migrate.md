---
title: "Migrieren Ihrer Lösung nach SQL Data Warehouse | Microsoft Docs"
description: "Migrationshinweise zum Übertragen Ihrer Lösung auf die Plattform Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: e5937f8472492cd1dd77c82ed518a665718623a1
ms.lasthandoff: 04/03/2017


---
# <a name="migrate-your-solution-to-sql-data-warehouse"></a>Migrieren Ihrer Lösung nach SQL Data Warehouse
SQL Data Warehouse ist ein verteiltes Datenbanksystem, das elastisch gemäß Ihren Anforderungen skaliert wird. Damit sowohl die Leistung als auch die Skalierbarkeit gewährleistet werden können, wurden nicht alle Funktionen von SQL Server in SQL Data Warehouse implementiert. In den folgenden Themen zur Migration werden wichtige Faktoren für die Migration Ihrer Lösung zu SQL Data Warehouse angesprochen. Beim Auslegen von Data Warehouses auf Skalierung kommen andere Designmuster zum Tragen, sodass herkömmliche Ansätzen nicht immer optimal sind. Möglicherweise stellen Sie fest, dass Sie Ihre vorhandene Lösung anpassen müssen, um die durch SQL Data Warehouse bereitgestellte verteilte Plattform vollständig nutzen zu können.

Außerdem sollten Sie berücksichtigen, dass SQL Data Warehouse eine Plattform ist, die auf Microsoft Azure basiert. Daher kann es auch zu Ihrer Migration gehören, Ihre Daten in die Cloud zu übertragen. Datenübertragung ist ein eigenständiges Thema und sollte sorgfältig überdacht werden, vor allem angesichts wachsender Datenmengen. Datenübertragung und das Laden von Daten sind getrennte Themen.

## <a name="migration-guidance"></a>Hinweise zur Migration
Lesen Sie vor der Migration unbedingt diese Artikel, um sicherzustellen, dass Sie einige der Produktunterschiede und der grundlegenden Konzepte verstehen.

* [Migrieren Ihres Schemas][Migrate your schema]
* [Migrieren von Daten][Migrate your data]
* [Migrieren Ihres Codes][Migrate your code]

## <a name="next-steps"></a>Nächste Schritte
Das CAT (Customer Advisory Team) bietet auch einige hervorragende SQL Data Warehouse-Anleitungen, die es in Blogs veröffentlicht.  Der Artikel [Migrating data to Azure SQL Data Warehouse in practice][Migrating data to Azure SQL Data Warehouse in practice] (Migrieren von Daten zu Azure SQL Data Warehouse in der Praxis) bietet Ihnen eine zusätzliche Anleitung zur Migration.

<!--Image references-->

<!--Article references-->
[Migrate your schema]: sql-data-warehouse-migrate-schema.md
[Migrate your data]: sql-data-warehouse-migrate-data.md
[Migrate your code]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/

