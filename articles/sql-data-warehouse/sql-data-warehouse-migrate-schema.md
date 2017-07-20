---
title: Migrieren Ihres Schemas nach SQL Data Warehouse | Microsoft Docs
description: "Tipps für die Migration eines Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
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
ms.author: joeyong;barbkess
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017


---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a>Migrieren Ihrer Schemas nach SQL Data Warehouse
Anleitung zum Migrieren Ihrer SQL-Schemas in SQL Data Warehouse 

## <a name="plan-your-schema-migration"></a>Planen Ihrer Schemamigration

Wenn Sie eine Migration planen, sehen Sie sich die [Tabellenübersicht][table overview] an, um mit den Tabellenentwurfsüberlegungen wie Statistiken, Verteilung, Partitionierung und Indizierung vertraut zu werden.  Darüber hinaus werden einige [nicht unterstützte Tabellenfeatures][unsupported table features] und deren Problemumgehungen behandelt.

## <a name="use-user-defined-schemas-to-consolidate-databases"></a>Verwenden benutzerdefinierter Schemas zum Konsolidieren von Datenbanken

Ihre vorhandene Workload verfügt sicherlich über mehr als eine Datenbank. Ein Data Warehouse in SQL Server kann z.B. eine Stagingdatenbank, eine Data Warehouse-Datenbank und einige Data Mart-Datenbanken enthalten. In dieser Topologie wird jede Datenbank als eine separate Workload mit separaten Sicherheitsrichtlinien ausgeführt.

Im Gegensatz dazu werden mit SQL Data Warehouse die gesamten Data Warehouse-Workloads innerhalb einer Datenbank ausgeführt. Datenbankübergreifende Verknüpfungen sind nicht zulässig. SQL Data Warehouse erwartet daher, dass alle vom Data Warehouse verwendeten Tabellen innerhalb der einen Datenbank gespeichert werden.

Es wird empfohlen, mithilfe des benutzerdefinierten Schemas Ihre vorhandene Workload in einer Datenbank zu konsolidieren. Beispiele finden Sie unter [Benutzerdefinierte Schemas in SQL Data Warehouse](sql-data-warehouse-develop-user-defined-schemas.md).

## <a name="use-compatible-data-types"></a>Verwenden kompatibler Datentypen
Ändern Sie Ihre Datentypen, sodass sie mit SQL Data Warehouse kompatibel sind. Eine Liste der unterstützten und nicht unterstützten Datentypen finden Sie unter [Datentypen][data types]. Dieses Thema bietet Problemumgehungen für die nicht unterstützten Typen. Darüber hinaus bietet es eine Abfrage, um vorhandene Typen zu identifizieren, die nicht in SQL Data Warehouse unterstützt werden.

## <a name="minimize-row-size"></a>Minimieren der Zeilengröße
Minimieren Sie die Zeilenlänge Ihrer Tabellen für optimale Leistung. Da kürzere Zeilenlängen zu verbesserter Leistung führen, verwenden Sie die kleinsten Datentypen, die für Ihre Daten funktionieren. 

PolyBase hat für die Zeilenbreite der Tabelle ein Limit von 1 MB.  Wenn Sie beabsichtigen, Daten in SQL Data Warehouse mit PolyBase zu laden, aktualisieren Sie die Tabelle, damit sie die maximale Zeilenbreite von weniger als 1 MB besitzt. 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a>Angeben der Verteilungsoption
SQL Data Warehouse ist ein verteiltes Datenbanksystem. Jede Tabelle ist in den Computeknoten verteilt oder repliziert. Es gibt eine Tabellenoption, mit der Sie angeben können, wie die Daten verteilt werden sollen. Die Auswahlmöglichkeiten sind Roundrobin, repliziert oder mit Hashverteilung. Jede Möglichkeit hat ihre Vor- und Nachteile. Wenn Sie die Verteilungsoption nicht angeben, wird SQL Data Warehouse Roundrobin als Standard verwenden.

- Roundrobin ist die Standardeinstellung. Diese Einstellung ist am einfachsten zu verwenden und lädt die Daten so schnell wie möglich. Joins benötigen jedoch eine Datenverschiebung, die die Abfrageleistung verlangsamt.
- Durch die Möglichkeit „repliziert“ wird eine Kopie der Tabelle auf jedem Computeknoten gespeichert. Replizierte Tabellen sind leistungsstärker, da sie keine Datenverschiebung für Joins und Aggregationen erfordern. Sie erfordern zusätzlichen Speicher und funktionieren deshalb am besten für kleinere Tabellen.
- Mit Hashverteilung werden die Zeilen über alle Knoten mithilfe der Hashfunktion verteilt. Verteilte Hashtabellen stellen das Kernstück von SQL Data Warehouse dar, da sie für eine hohe Abfrageleistung bei umfangreichen Tabellen ausgelegt sind. Diese Option erfordert einiges an Planung, um die beste Spalte auszuwählen, auf der die Daten verteilt werden sollen. Wenn Sie zunächst jedoch nicht die beste Spalte auswählen, können Sie die Daten ganz einfach auf eine andere Spalte verteilen. 

Um die optimale Verteilungsoption für jede Tabelle auswählen, wechseln Sie zu [Verteilen von Tabellen in SQL Data Warehouse](sql-data-warehouse-tables-distribute.md).


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

<!--MSDN references-->


<!--Other Web references-->

