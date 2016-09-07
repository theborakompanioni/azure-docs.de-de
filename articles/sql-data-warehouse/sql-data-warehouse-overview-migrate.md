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
   ms.date="08/17/2016"
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
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][].

Noch mehr Informationen finden Sie in der [Transact-SQL-Referenz][].

Sehen Sie sich außerdem die [Übersicht über das Laden][] an. Dieser Artikel beschreibt verschiedene Optionen zum Laden von Daten und enthält eine ausführliche Anleitung.

<!--Image references-->

<!--Article references-->
[Migrieren Ihres Schemas]: sql-data-warehouse-migrate-schema.md
[Migrieren Ihrer Daten]: sql-data-warehouse-migrate-data.md
[Migrieren Ihres Codes]: sql-data-warehouse-migrate-code.md

[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md
[Übersicht über das Laden]: sql-data-warehouse-overview-load.md
[Transact-SQL-Referenz]: sql-data-warehouse-overview-reference.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0824_2016-->