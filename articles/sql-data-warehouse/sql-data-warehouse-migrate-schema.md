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
   ms.date="08/17/2016"
   ms.author="jrj;barbkess;sonyama"/>

# Migrieren Ihres Schemas nach SQL Data Warehouse#

In den folgenden Übersichten erhalten Sie Informationen zu den Unterschieden zwischen SQL Server und SQL Data Warehouse, sodass Sie Ihre Datenbank migrieren können.

### Funktionen in Tabellen
In SQL Data Warehouse werden die folgenden Funktionen nicht verwendet oder nicht unterstützt:

- Primärschlüssel
- Fremdschlüssel
- CHECK-Einschränkungen
- UNIQUE-Einschränkungen
- Eindeutige Indizes
- Berechnete Spalten
- Spalten mit geringer Dichte
- Benutzerdefinierte Typen
- Indizierte Sichten
- Identitäten
- Sequenzen
- Trigger
- Synonyme


### Datentypunterschiede
In SQL Data Warehouse werden die folgenden allgemeinen Geschäftsdatentypen unterstützt:

- bigint
- binary
- Bit
- char
- date
- datetime
- datetime2
- datetimeoffset
- decimal
- float
- int
- money
- nchar
- nvarchar
- numeric
- real
- smalldatetime
- smallint
- smallmoney
- sysname
- time
- tinyint
- varbinary
- varchar
- uniqueidentifier

Mit der folgenden Abfrage können Sie Spalten in Ihrem Data Warehouse ermitteln, die inkompatible Datentypen enthalten:

```sql
SELECT  t.[name]
,       c.[name]
,       c.[system_type_id]
,       c.[user_type_id]
,       y.[is_user_defined]
,       y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN
                (   'geography'
                ,   'geometry'
                ,   'hierarchyid'
                ,   'image'
                ,   'ntext'
                ,   'sql_variant'
                ,   'text'
                ,   'timestamp'
                ,   'xml'
                )
AND  y.[is_user_defined] = 1
;

```

Die Abfrage enthält alle benutzerdefinierten Datentypen, die auch nicht unterstützt werden.

Wenn Ihre Datenbank nicht unterstützte Datentypen enthält, ist das kein Grund zur Sorge. Unten sind einige Alternativen aufgeführt, die Sie stattdessen wählen können.

Alternativen:

- **geometry**: ein varbinary-Typ
- **geography**: ein varbinary-Typ
- **hierarchyid**: Dieser CLR-Typ wird nicht unterstützt.
- **image**, **text**, **ntext**: varchar/nvarchar (je kleiner, desto besser)
- **sql\_variant**: Spalte in mehrere Spalten mit starker Typisierung unterteilen
- **table**: in temporäre Tabellen konvertieren
- **timestamp**: Code anpassen, sodass „datetime2“ und die `CURRENT_TIMESTAMP`-Funktion verwendet werden. Beachten Sie, dass Sie „current\_timestamp“ nicht als Standardeinschränkung verwenden können. Wenn Sie rowversion-Werte aus einer Spalte mit timestamp-Typ migrieren müssen, sollten Sie „binary(8)“ oder „varbinary(8)“ für NOT NULL- oder NULL-Zeilenversionswerte verwenden.
- **Benutzerdefinierte Datentypen**: zurück in native Typen konvertieren, falls möglich
- **xml**: varchar(max) oder kleiner zur Verbesserung der Leistung. Gegebenenfalls in Spalten unterteilen.

Für bessere Leistung anstelle von:

- nvarchar(max): nvarchar(4000) oder kleiner zur Verbesserung der Leistung
- varchar(max): varchar(8000) oder kleiner zur Verbesserung der Leistung

Teilweise unterstützt:

- Standardeinschränkungen unterstützen nur Literale und Konstanten. Nicht deterministische Ausdrücke oder Funktionen, z. B. `GETDATE()` oder `CURRENT_TIMESTAMP`, werden nicht unterstützt.

> [AZURE.NOTE] Definieren Sie die Tabellen, sodass die maximale Zeilengröße 32.767 Byte nicht überschreitet, wenn PolyBase zum Laden verwendet wird. Sie müssen daran denken, dass die maximale Zeilengröße die vollständige Länge aller Spalten mit variabler Länge umfasst. Zwar können Sie eine Zeile mit variabler Länge definieren, Daten, die diese Angabe überschreiten, können Sie jedoch derzeit nicht mit PolyBase laden. Verwenden Sie als Zwischenlösung BCP, um große Zeilen zu laden. Versuchen Sie schließlich, die Größe Ihrer Spalten mit variabler Länge zu beschränken, um beim Ausführen von Abfragen einen noch besseren Durchsatz zu erzielen.

## Nächste Schritte
Nachdem Sie Ihr Datenbankschema erfolgreich zu SQL Data Warehouse migriert haben, fahren Sie mit den folgenden Artikeln fort:

- [Migrieren Ihrer Daten][]
- [Migrieren Ihres Codes][]

Weitere Hinweise zur Entwicklung finden Sie in der [Entwicklungsübersicht][].

<!--Image references-->

<!--Article references-->
[Migrieren Ihres Codes]: sql-data-warehouse-migrate-code.md
[Migrieren Ihrer Daten]: sql-data-warehouse-migrate-data.md
[Entwicklungsübersicht]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->


<!--Other Web references-->

<!---HONumber=AcomDC_0824_2016-->