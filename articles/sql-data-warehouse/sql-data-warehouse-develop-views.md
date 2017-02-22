---
title: Verwenden von T-SQL-Ansichten in Azure SQL Data Warehouse | Microsoft-Dokumentation
description: "Tipps für die Verwendung von Transact-SQL-Sichten in Azure SQL Data Warehouse zum Entwickeln von Lösungen."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: jrj;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 43ab6a2f71ab51c50847b1ba5249f51c48e03fea
ms.openlocfilehash: 902929fba9e2486d97323ab52505ba6db3fe0aab


---
# <a name="views-in-sql-data-warehouse"></a>Sichten in SQL Data Warehouse
Sichten sind in SQL Data Warehouse besonders nützlich. Sie können auf verschiedene Weisen zur Verbesserung der Qualität der Lösung verwendet werden.  Dieser Artikel enthält einige Beispiele dafür, wie Sie Ihre Lösung mit Sichten bereichern können, sowie Informationen zu den Einschränkungen, die zu berücksichtigen sind.

> [!NOTE]
> Die Syntax für `CREATE VIEW` wird in diesem Artikel nicht erörtert. Sie finden diese Informationen unter [CREATE VIEW][CREATE VIEW] auf der MSDN-Website.
> 
> 

## <a name="architectural-abstraction"></a>Architekturabstraktion
Ein häufig verwendetes Anwendungsmuster ist das erneute Erstellen von Tabellen mit CREATE TABLE AS SELECT (CTAS) gefolgt von einem Muster zur Objektumbenennung beim Laden von Daten.

Im folgenden Beispiel werden einer Datumsdimension neue Datumsdatensätze hinzugefügt. Beachten Sie, wie eine neue Tabelle (DimDate_New) zuerst erstellt und dann umbenannt wird, um die ursprüngliche Version der Tabelle zu ersetzen.

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

Dieser Ansatz kann aber auch dazu führen, dass Tabellen in der Sicht eines Benutzers ein- und ausgeblendet und Fehlermeldungen der Art „Tabelle nicht vorhanden“ angezeigt werden. Sichten können verwendet werden, um eine konsistente Darstellungsschicht für Benutzer bereitzustellen, während die zugrunde liegenden Objekte umbenannt werden. Indem Benutzern der Zugriff auf Daten über Sichten gewährt wird, benötigen sie keine Einblicke in die zugrunde liegenden Tabellen. Dadurch wird eine einheitliche Benutzererfahrung gewährleistet, während die Data Warehouse-Designer das Datenmodell weiterentwickeln und die Leistung maximieren können, indem sie beim Datenladevorgang CTAS verwenden.    

## <a name="performance-optimization"></a>Leistungsoptimierung
Sichten können auch genutzt werden, um leistungsoptimierte Verknüpfungen zwischen Tabellen durchzusetzen. Beispielsweise kann eine Sicht einen redundanten Verteilungsschlüssel als Teil des Verknüpfungskriteriums enthalten, um die Datenverschiebung zu minimieren.  Ein weiterer Vorteil einer Sicht kann beispielsweise sein, eine bestimmte Abfrage oder einen Verknüpfungshinweis zu erzwingen. Durch den Einsatz von Sichten auf diese Weise wird sichergestellt, dass Verknüpfungen immer optimal durchgeführt werden, und Benutzer müssen sich nicht mehr das richtige Konstrukt für ihre Verknüpfungen merken.

## <a name="limitations"></a>Einschränkungen
Sichten in SQL Data Warehouse bestehen nur aus Metadaten  Daher sind die folgenden Optionen nicht verfügbar:

* Es gibt keine Schemabindungsoption.
* Basistabellen können nicht über die Ansicht aktualisiert werden.
* Sichten können nicht für temporäre Tabellen erstellt werden.
* EXPAND/NOEXPAND-Hinweise werden nicht unterstützt.
* Es sind keine indizierten Sichten in SQL Data Warehouse verfügbar.

## <a name="next-steps"></a>Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht für SQL Data Warehouse][SQL Data Warehouse development overview].
Die Syntax für `CREATE VIEW` finden Sie unter [CREATE VIEW][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->



<!--HONumber=Jan17_HO4-->


