<properties
   pageTitle="Migrieren Ihres Schemas nach SQL Data Warehouse | Microsoft Azure"
   description="Tipps für die Migration eines Schemas in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
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
   ms.date="08/25/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Migrieren Ihres Schemas nach SQL Data Warehouse#

In den folgenden Übersichten erhalten Sie Informationen zu den Unterschieden zwischen SQL Server und SQL Data Warehouse, sodass Sie Ihre Datenbank migrieren können.

## Tabellenmigration

Bei der Migration Ihrer Tabellen sollten Sie mit den Tabellenfeatures von SQL Data Warehouse-Tabellen vertraut. Die [Tabellenübersicht][] ist ein idealer Ausgangspunkt. Dieser Artikel informiert Sie über die wichtigsten Aspekte beim Erstellen einer Tabelle, z.B. Tabellenstatistiken, Verteilung, Partitionierung und Indizierung. Darüber hinaus werden einige [nicht unterstützte Tabellenfeatures][] und Problemumgehungen behandelt.

SQL Data Warehouse unterstützt die folgenden allgemeinen Geschäftsdatentypen. Der Artikel über [Datentypen][] enthält eine Liste der unterstützten und [nicht unterstützten Datentypen][]. Außerdem enthält der Artikel über [Datentypen][] auch eine Abfrage zur Identifizierung [nicht unterstützter Datentypen][]. Beachten Sie bei der Konvertierung von Datentypen unbedingt die [bewährten Vorgehensweisen im Umgang mit Datentypen][].

## Nächste Schritte
Nachdem Sie Ihr Datenbankschema erfolgreich zu SQL Data Warehouse migriert haben, fahren Sie mit den folgenden Artikeln fort:

- [Migrieren Ihrer Daten][]
- [Migrieren Ihres Codes][]

Weitere Informationen zu SQL Data Warehouse finden Sie unter [Bewährte Methoden für SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Migrieren Ihres Codes]: ./sql-data-warehouse-migrate-code.md
[Migrieren Ihrer Daten]: ./sql-data-warehouse-migrate-data.md
[Bewährte Methoden für SQL Data Warehouse]: ./sql-data-warehouse-best-practices.md
[Tabellenübersicht]: ./sql-data-warehouse-tables-overview.md
[nicht unterstützte Tabellenfeatures]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Datentypen]: ./sql-data-warehouse-tables-data-types.md
[nicht unterstützten Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[nicht unterstützter Datentypen]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types
[bewährten Vorgehensweisen im Umgang mit Datentypen]: ./sql-data-warehouse-tables-data-types.md#data-type-best-practices

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0831_2016-->