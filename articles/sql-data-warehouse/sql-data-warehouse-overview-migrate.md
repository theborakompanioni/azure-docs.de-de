<properties
   pageTitle="Migrieren Ihrer Lösung nach SQL Data Warehouse | Microsoft Azure"
   description="Migrationshinweise zum Übertragen Ihrer Lösung auf die Plattform Azure SQL Data Warehouse."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/30/2016"
   ms.author="barbkess;jrj;sonyama"/>

# Migrieren Ihrer Lösung nach SQL Data Warehouse

SQL Data Warehouse ist ein verteiltes Datenbanksystem, das elastisch gemäß Ihren Anforderungen skaliert wird. Damit sowohl die Leistung als auch die Skalierbarkeit gewährleistet werden können, wurden nicht alle Funktionen von SQL Server in SQL Data Warehouse implementiert. In den folgenden Themen zur Migration werden wichtige Faktoren für die Migration Ihrer Lösung zu SQL Data Warehouse angesprochen. Beim Auslegen von Data Warehouses auf Skalierung kommen andere Designmuster zum Tragen, sodass herkömmliche Ansätzen nicht immer optimal sind. Möglicherweise stellen Sie fest, dass Sie Ihre vorhandene Lösung anpassen müssen, um die durch SQL Data Warehouse bereitgestellte verteilte Plattform vollständig nutzen zu können.

Außerdem sollten Sie berücksichtigen, dass SQL Data Warehouse eine Plattform ist, die auf Microsoft Azure basiert. Daher kann es auch zu Ihrer Migration gehören, Ihre Daten in die Cloud zu übertragen. Datenübertragung ist ein eigenständiges Thema und sollte sorgfältig überdacht werden, vor allem angesichts wachsender Datenmengen. Datenübertragung und das Laden von Daten sind getrennte Themen.

## Hinweise zur Migration

Lesen Sie vor der Migration unbedingt diese Artikel, um sicherzustellen, dass Sie einige der Produktunterschiede und der grundlegenden Konzepte verstehen.

- [Migrieren Ihres Schemas][]
- [Migrieren Ihrer Daten][]
- [Migrieren Ihres Codes][]

## Nächste Schritte

Das CAT (Customer Advisory Team) bietet auch einige hervorragende SQL Data Warehouse-Anleitungen, die es in Blogs veröffentlicht. Der Artikel [Migrating data to Azure SQL Data Warehouse in practice][] (Migrieren von Daten zu Azure SQL Data Warehouse in der Praxis) bietet Ihnen eine zusätzliche Anleitung zur Migration.

<!--Image references-->

<!--Article references-->
[Migrieren Ihres Schemas]: sql-data-warehouse-migrate-schema.md
[Migrieren Ihrer Daten]: sql-data-warehouse-migrate-data.md
[Migrieren Ihres Codes]: sql-data-warehouse-migrate-code.md


<!--MSDN references-->


<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/

<!---HONumber=AcomDC_0831_2016-->