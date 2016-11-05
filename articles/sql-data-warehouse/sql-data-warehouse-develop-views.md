---
title: Sichten in SQL Data Warehouse | Microsoft Docs
description: Tipps für die Verwendung von Transact-SQL-Sichten in Azure SQL Data Warehouse zum Entwickeln von Lösungen.
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 07/01/2016
ms.author: jrj;barbkess;sonyama

---
# Sichten in SQL Data Warehouse
Sichten sind in SQL Data Warehouse besonders nützlich. Sie können auf verschiedene Weisen zur Verbesserung der Qualität der Lösung verwendet werden. Dieser Artikel enthält einige Beispiele dafür, wie Sie Ihre Lösung mit Sichten bereichern können, sowie Informationen zu den Einschränkungen, die zu berücksichtigen sind.

> [!NOTE]
> Die Syntax für `CREATE VIEW` wird in diesem Artikel nicht erörtert. Sie finden diese Informationen unter [CREATE VIEW][CREATE VIEW] auf der MSDN-Website.
> 
> 

## Architekturabstraktion
Ein häufig verwendetes Anwendungsmuster ist das erneute Erstellen von Tabellen mit CREATE TABLE AS SELECT (CTAS) gefolgt von einem Muster zur Objektumbenennung beim Laden von Daten.

Im folgenden Beispiel werden einer Datumsdimension neue Datumsdatensätze hinzugefügt. Beachten Sie, wie eine neue Tabelle (DimDate\_New) zuerst erstellt und dann umbenannt wird, um die ursprüngliche Version der Tabelle zu ersetzen.

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

## Leistungsoptimierung
Sichten können auch genutzt werden, um leistungsoptimierte Verknüpfungen zwischen Tabellen durchzusetzen. Beispielsweise kann eine Sicht einen redundanten Verteilungsschlüssel als Teil des Verknüpfungskriteriums enthalten, um die Datenverschiebung zu minimieren. Ein weiterer Vorteil einer Sicht kann beispielsweise sein, eine bestimmte Abfrage oder einen Verknüpfungshinweis zu erzwingen. Durch den Einsatz von Sichten auf diese Weise wird sichergestellt, dass Verknüpfungen immer optimal durchgeführt werden, und Benutzer müssen sich nicht mehr das richtige Konstrukt für ihre Verknüpfungen merken.

## Einschränkungen
Sichten in SQL Data Warehouse bestehen nur aus Metadaten Daher sind die folgenden Optionen nicht verfügbar:

* Es gibt keine Schemabindungsoption.
* Basistabellen können nicht über die Ansicht aktualisiert werden.
* Sichten können nicht für temporäre Tabellen erstellt werden.
* EXPAND/NOEXPAND-Hinweise werden nicht unterstützt.
* Es sind keine indizierten Sichten in SQL Data Warehouse verfügbar.

## Nächste Schritte
Weitere Hinweise zur Entwicklung finden Sie in der [SQL Data Warehouse-Entwicklungsübersicht][SQL Data Warehouse-Entwicklungsübersicht]. Die Syntax für `CREATE VIEW` finden Sie unter [CREATE VIEW][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse-Entwicklungsübersicht]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/de-DE/library/ms187956.aspx

<!--Other Web references-->

<!---HONumber=AcomDC_0706_2016-->