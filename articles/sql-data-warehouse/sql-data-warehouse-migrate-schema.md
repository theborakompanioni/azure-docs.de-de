---
title: Migrieren Ihres Schemas nach SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Migration eines Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
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
ms.openlocfilehash: 0630f43642a0be98c470032d32b74ca14ee144e5
ms.lasthandoff: 04/03/2017


---
# <a name="migrate-your-schema-to-sql-data-warehouse"></a>Migrieren Ihres Schemas nach SQL Data Warehouse
In den folgenden Übersichten erhalten Sie Informationen zu den Unterschieden zwischen SQL Server und SQL Data Warehouse, sodass Sie Ihre Datenbank migrieren können.

## <a name="table-migration"></a>Tabellenmigration
Bei der Migration Ihrer Tabellen sollten Sie mit den Tabellenfeatures von SQL Data Warehouse-Tabellen vertraut.  Die [Tabellenübersicht][table overview] ist ein idealer Ausgangspunkt.  Dieser Artikel informiert Sie über die wichtigsten Aspekte beim Erstellen einer Tabelle, z.B. Tabellenstatistiken, Verteilung, Partitionierung und Indizierung.  Darüber hinaus werden einige [nicht unterstützte Tabellenfeatures][unsupported table features] und Problemumgehungen behandelt.

SQL Data Warehouse unterstützt die folgenden allgemeinen Geschäftsdatentypen.  Der Artikel über [Datentypen][data types] enthält eine Liste der unterstützten und der [nicht unterstützten Datentypen][unsupported data types].  Außerdem enthält der Artikel über [Datentypen][data types] eine Abfrage zum Ermitteln von [nicht unterstützten Datentypen][unsupported data types].  Beachten Sie bei der Konvertierung von Datentypen unbedingt die [bewährten Methoden für Datentypen][data type best practices].

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie Ihr Datenbankschema erfolgreich zu SQL Data Warehouse migriert haben, fahren Sie mit den folgenden Artikeln fort:

* [Migrieren von Daten][Migrate your data]
* [Migrieren Ihres Codes][Migrate your code]

Weitere Informationen zu bewährten Methoden für SQL Data Warehouse finden Sie unter [Bewährte Methoden für SQL Data Warehouse][best practices].

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[data type best practices]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->

