<properties
	pageTitle="Auswählen von Zeilen für die Migration mit einem Filterprädikat (Stretch-Datenbank) | Microsoft Azure"
	description="Informationen zum Auswählen von Zeilen für die Migration mit einem Filterprädikat."
	services="sql-server-stretch-database"
	documentationCenter=""
	authors="douglaslMS"
	manager=""
	editor=""/>

<tags
	ms.service="sql-server-stretch-database"
	ms.workload="data-management"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/14/2016"
	ms.author="douglasl"/>

# Auswählen von Zeilen für die Migration mit einem Filterprädikat (Stretch-Datenbank)

Wenn Sie Verlaufsdaten in einer separaten Tabelle speichern, können Sie Stretch-Datenbank so konfigurieren, dass die gesamte Tabelle migriert wird. Wenn die Tabelle aktuelle und Verlaufsdaten enthält, können Sie anderenfalls ein Filterprädikat zum Auswählen der zu migrierenden Zeilen angeben. Das Filterprädikat ist eine Inline-Tabellenwertfunktion. In diesem Thema wird beschrieben, wie Sie eine Inline-Tabellenwertfunktion zum Auswählen von zu migrierenden Zeilen schreiben.

>   [AZURE.NOTE] Falls Sie ein Filterprädikat mit schlechter Leistung angeben, wird die Datenmigration ebenfalls mit schlechter Leistung durchgeführt. Stretch-Datenbank wendet das Filterprädikat mithilfe des CROSS APPLY-Operators auf die Tabelle an.

Wenn Sie kein Filterprädikat angeben, wird die gesamte Tabelle migriert.

Wenn Sie den Assistenten zum Aktivieren einer Datenbank für Stretch ausführen, können Sie eine ganze Tabelle migrieren oder ein einfaches Filterprädikat im Assistenten festlegen. Führen Sie einen der folgenden Schritte aus, wenn Sie eine andere Art von Filterprädikat für die Auswahl der zu migrierenden Zeilen verwenden möchten.

-   Beenden Sie den Assistenten, und führen Sie die ALTER TABLE-Anweisung aus, um Stretch für die Tabelle zu aktivieren und ein Prädikat anzugeben.

-   Führen Sie die ALTER TABLE-Anweisung aus, um ein Prädikat anzugeben, nachdem Sie den Assistenten beendet haben.

Die ALTER TABLE-Syntax für das Hinzufügen eines Prädikats wird weiter unten in diesem Thema beschrieben.

## Grundlegende Anforderungen für das Filterprädikat
Die für ein Stretch-Datenbank-Filterprädikat erforderliche Inline-Tabellenwertfunktion sieht wie das folgende Beispiel aus.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datatype1, @column2 datatype2 [, ...n])
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE <predicate>
```
Die Parameter für die Funktion müssen Bezeichner für Spalten der Tabelle sein.

Die Bindung des Schemas ist erforderlich, um zu verhindern, dass Spalten, die vom Filterprädikat verwendet werden, verworfen oder geändert werden.

### Rückgabewert
Wenn die Funktion ein nicht leeres Ergebnis zurückgibt, ist die Zeile zur Migration berechtigt. Andernfalls – wenn die Funktion kein Ergebnis zurückgibt – ist die Zeile nicht zur Migration berechtigt.

### Bedingungen
Das &lt;*Prädikat*&gt; kann aus einer Bedingung oder mehreren Bedingungen bestehen, die mit dem logischen AND-Operator verknüpft sind.

```
<predicate> ::= <condition> [ AND <condition> ] [ ...n ]
```
Jede Bedingung kann wiederum aus einer primitiven Bedingung oder mehreren primitiven Bedingungen bestehen, die mit dem logischen OR-Operator verknüpft sind.

```
<condition> ::= <primitive_condition> [ OR <primitive_condition> ] [ ...n ]
```

### Primitive Bedingungen
Eine primitive Bedingung kann einen der folgenden Vergleiche ausführen.

```
<primitive_condition> ::=
{
<function_parameter> <comparison_operator> constant
| <function_parameter> { IS NULL | IS NOT NULL }
| <function_parameter> IN ( constant [ ,...n ] )
}
```

-   Vergleichen Sie einen Funktionsparameter mit einem konstanten Ausdruck. Beispiel: `@column1 < 1000`.

    Nachstehend sehen Sie ein Beispiel, in dem geprüft wird, ob der Wert einer *date*-Spalte &lt; „1/1/2016“ ist.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
    GO

    ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
    	FILTER_PREDICATE = dbo.fn_stretchpredicate(date),
    	MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Wenden Sie den Operator IS NULL oder IS NOT NULL auf einen Funktionsparameter an.

-   Verwenden Sie den IN-Operator, um einen Funktionsparameter mit einer Liste von konstanten Werten zu vergleichen.

    Nachstehend sehen Sie ein Beispiel, in dem geprüft wird, ob der Wert einer *shipment\_status*-Spalte `IN (N'Completed', N'Returned', N'Cancelled')` ist.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate(@column1 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
    	FILTER_PREDICATE = dbo.fn_stretchpredicate(shipment_status),
    	MIGRATION_STATE = OUTBOUND
    ) )
    ```

### Vergleichsoperatoren
Die folgenden Vergleichsoperatoren werden unterstützt.

`<, <=, >, >=, =, <>, !=, !<, !>`

```
<comparison_operator> ::= { < | <= | > | >= | = | <> | != | !< | !> }
```

### Konstante Ausdrücke
Bei den Konstanten, die Sie in einem Filterprädikat verwenden, kann es sich um einen beliebigen deterministischen Ausdruck handeln, der beim Definieren der Funktion ausgewertet werden kann. Konstante Ausdrücke können Folgendes enthalten.

-   Literale. Beispiel: `N’abc’, 123`.

-   Algebraische Ausdrücke. Beispiel: `123 + 456`.

-   Deterministische Funktionen. Beispiel: `SQRT(900)`.

-   Deterministische Konvertierungen, die CAST oder CONVERT verwenden. Beispiel: `CONVERT(datetime, '1/1/2016', 101)`.

### Andere Ausdrücke.
Sie können die BETWEEN- und NOT BETWEEN-Operatoren verwenden, wenn das resultierende Prädikat den Regeln entspricht, die hier beschrieben werden, nachdem Sie die BETWEEN- und NOT BETWEEN-Operatoren durch die entsprechenden AND- und OR-Ausdrücke ersetzen.

Sie können keine Unterabfragen oder nicht deterministische Funktionen wie RAND() oder GETDATE() verwenden.

## Hinzufügen eines Filterprädikats zu einer Tabelle
Fügen Sie einer Tabelle ein Filterprädikat hinzu, indem Sie die ALTER TABLE-Anweisung ausführen und eine vorhandene Inline-Tabellenwertfunktion als Wert des FILTER\_PREDICATE-Parameters angeben. Zum Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
	FILTER_PREDICATE = dbo.fn_stretchpredicate(column1, column2),
	MIGRATION_STATE = <desired_migration_state>
) )
```
Nachdem Sie die Funktion als Prädikat an die Tabelle gebunden haben, gelten die folgenden Punkte als erfüllt.

-   Bei der nächsten Datenmigration werden nur die Zeilen migriert, für die die Funktion einen nicht leeren Wert zurückgibt.

-   Die von der Funktion verwendeten Spalten sind schemagebunden. Sie können diese Spalten nicht ändern, solange eine Tabelle die Funktion als Filterprädikat verwendet.

Sie können die Inline-Tabellenwertfunktionen nicht verwerfen, solange eine Tabelle die Funktion als Filterprädikat verwendet.

>   [AZURE.NOTE] Um die Leistung der Filterfunktion zu verbessern, erstellen Sie einen Index für die Spalten, die von der Funktion verwendet werden.

### Übergeben von Spaltennamen an das Filterprädikat
Wenn Sie einer Tabelle eine Filterfunktion zuweisen, geben Sie einteilige Namen für die Spalten an, die an die Filterfunktion übermittelt werden. Wenn Sie bei der Übergabe der Spaltennamen einen dreiteiligen Namen angeben, wird durch anschließende Abfragen der Stretch-fähigen Tabelle ein Fehler verursacht.

Wenn Sie zum Beispiel einen dreiteiligen Spaltennamen angeben, wie im folgenden Beispiel dargestellt, wird die Anweisung erfolgreich ausgeführt, aber nachfolgende Abfragen der Tabelle verursachen einen Fehler.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(dbo.SensorTelemetry.ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

Geben Sie stattdessen, wie im folgenden Beispiel gezeigt, die Filterfunktion mit einem einteiligen Spaltennamen an.

```tsql
ALTER TABLE SensorTelemetry
  SET ( REMOTE_DATA_ARCHIVE = ON  (
    FILTER_PREDICATE=dbo.fn_stretchpredicate(ScanDate),
    MIGRATION_STATE = OUTBOUND )
  )
```

## <a name="addafterwiz"></a>Hinzufügen eines Filterprädikats nach dem Ausführen des Assistenten  

Wenn ein Prädikat verwendet werden soll, das nicht im Assistenten zum **Aktivieren einer Datenbank für Stretch** erstellt werden kann, können Sie durch Ausführen der Anweisung ALTER TABLE ein Prädikat angeben, nachdem Sie den Assistenten beendet haben. Bevor Sie ein Prädikat anwenden können, müssen Sie jedoch die bereits gestartete Datenmigration beenden und migrierte Daten zurückholen. (Weitere Informationen über die Notwendigkeit dieses Verfahrens finden Sie unter [Ersetzen eines vorhandenen Filterprädikats](#replacePredicate).

1. Kehren Sie die Migrationsrichtung um, und holen Sie die bereits migrierten Daten zurück. Dieser Vorgang kann nach dem Start nicht mehr abgebrochen werden. Es entstehen auch Kosten in Azure für ausgehende Datenübertragungen. Weitere Informationen finden Sie unter [Azure-Preisgestaltung](https://azure.microsoft.com/pricing/details/data-transfers/).  

    ```tsql  
    ALTER TABLE <table name>  
         SET ( REMOTE_DATA_ARCHIVE ( MIGRATION_STATE = INBOUND ) ) ;   
    ```  

2. Warten Sie, bis die Migration abgeschlossen ist. Sie können den Status in der **Stretch-Datenbanküberwachung** in SQL Server Management Studio überprüfen oder die Anzeige **sys.dm\_db\_rda\_migration\_status** abfragen. Weitere Informationen finden Sie unter [Überwachen und Behandeln von Problemen der Datenmigration](sql-server-stretch-database-monitor.md) oder [sys.dm\_db\_rda\_migration\_status](https://msdn.microsoft.com/library/dn935017.aspx).

3. Erstellen Sie das Filterprädikat, das auf die Tabelle angewendet werden soll.

4. Fügen Sie das Prädikat der Tabelle hinzu, und starten Sie erneut die Datenmigration zu Azure.

    ```tsql  
    ALTER TABLE <table name>  
        SET ( REMOTE_DATA_ARCHIVE  
            (           
                FILTER_PREDICATE = <predicate>,  
                MIGRATION_STATE = OUTBOUND  
            )  
        );   
    ```  

## Filtern von Zeilen nach Datum
Im folgenden Beispiel werden Zeilen migriert, in denen die Spalte **date** einen Wert enthält, der vor dem 1. Januar 2016 liegt.

```tsql
-- Filter by date
--
CREATE FUNCTION dbo.fn_stretch_by_date(@date datetime2)
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @date < CONVERT(datetime2, '1/1/2016', 101)
GO
```

## Filtern von Zeilen nach dem Wert in einer Statusspalte
Im folgenden Beispiel werden Zeilen migriert, in denen die Spalte **status** einen der angegebenen Werte enthält.

```tsql
-- Filter by status column
--
CREATE FUNCTION dbo.fn_stretch_by_status(@status nvarchar(128))
RETURNS TABLE
WITH SCHEMABINDING
AS
       RETURN SELECT 1 AS is_eligible WHERE @status IN (N'Completed', N'Returned', N'Cancelled')
GO
```

## Filtern von Zeilen mithilfe eines gleitenden Fensters
Um Zeilen mithilfe eines gleitenden Fensters zu filtern, beachten Sie die folgenden Anforderungen für die Filterfunktion.

-   Die Funktion muss deterministisch sein. Sie können daher keine Funktion erstellen, die das gleitende Fenster im Laufe der Zeit automatisch neu berechnet.

-   Die Funktion verwendet die Schemabindung. Darum können Sie die Funktion nicht einfach jeden Tag „direkt“ durch Aufrufen von ALTER FUNCTION zum Verschieben des gleitenden Fensters aktualisieren.

Beginnen Sie wie im folgenden Beispiel mit einem Filterprädikat, durch das Zeilen migriert werden, in denen die Spalte **systemEndTime** einen Wert vor dem 1. Januar 2016 enthält.

```tsql
CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160101(@systemEndTime datetime2)
RETURNS TABLE
WITH SCHEMABINDING  
AS  
RETURN SELECT 1 AS is_eligible
  WHERE @systemEndTime < CONVERT(datetime2, '2016-01-01T00:00:00', 101) ;
```

Wenden Sie das Filterprädikat auf die Tabelle an.

```tsql
ALTER TABLE <table name>
SET (
        REMOTE_DATA_ARCHIVE = ON
                (
                        FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160101 (SysEndTime)
                                , MIGRATION_STATE = OUTBOUND
                )
        )
;
```

Wenn Sie das gleitende Fenster aktualisieren möchten, gehen Sie folgendermaßen vor.

1.  Erstellen Sie eine neue Funktion, die das neue gleitende Fenster angibt. Das folgende Beispiel wählt die Datumsangaben, die vor dem 2. Januar 2016 statt dem 1. Januar 2016 liegen.

2.  Ersetzen Sie das vorherige Filterprädikat durch Aufrufen von ALTER TABLE durch das neue, wie im folgenden Beispiel gezeigt.

3. Löschen Sie optional die vorherige Filterfunktion, die Sie nicht mehr verwenden, durch Aufrufen von DROP FUNCTION. (Dieser Schritt ist nicht im Beispiel dargestellt.)

```tsql
BEGIN TRAN
GO
        /*(1) Create new predicate function definition */
        CREATE FUNCTION dbo.fn_StretchBySystemEndTime20160102(@systemEndTime datetime2)
        RETURNS TABLE
        WITH SCHEMABINDING
        AS
        RETURN SELECT 1 AS is_eligible
               WHERE @systemEndTime < CONVERT(datetime2,'2016-01-02T00:00:00', 101)
        GO

        /*(2) Set the new function as filter predicate */
        ALTER TABLE <table name>
        SET
        (
               REMOTE_DATA_ARCHIVE = ON
               (
                       FILTER_PREDICATE = dbo.fn_StretchBySystemEndTime20160102(SysEndTime),
                       MIGRATION_STATE = OUTBOUND
               )
        )
COMMIT ;
```

## Weitere Beispiele für gültige Filterprädikate

-   Das folgende Beispiel kombiniert die beiden primitiven Bedingungen mithilfe des logischen AND-Operators.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate((@column1 datetime, @column2 nvarchar(15))
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
      WHERE @column1 < N'20150101' AND @column2 IN (N'Completed', N'Returned', N'Cancelled')
    GO

    ALTER TABLE table1 SET ( REMOTE_DATA_ARCHIVE = ON (
    	FILTER_PREDICATE = dbo.fn_stretchpredicate(date, shipment_status),
    	MIGRATION_STATE = OUTBOUND
    ) )
    ```

-   Im folgenden Beispiel werden verschiedene Bedingungen und eine deterministische Konvertierung mit CONVERT verwendet.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example1(@column1 datetime, @column2 int, @column3 nvarchar)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
        WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)AND (@column2 < -100 OR @column2 > 100 OR @column2 IS NULL)AND @column3 IN (N'Completed', N'Returned', N'Cancelled')
    GO
    ```

-   Im folgenden Beispiel werden mathematische Operatoren und Funktionen verwendet.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example2(@column1 float)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 < SQRT(400) + 10
    GO
    ```

-   Im folgenden Beispiel werden die BETWEEN- und NOT BETWEEN-Operatoren verwendet. Diese Nutzung ist gültig, da das resultierende Prädikat den Regeln entspricht, die hier beschrieben werden, nachdem Sie die BETWEEN- und NOT BETWEEN-Operatoren durch die entsprechenden AND- und OR-Ausdrücke ersetzen.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example3(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 BETWEEN 0 AND 100
    			AND (@column2 NOT BETWEEN 200 AND 300 OR @column1 = 50)
    GO
    ```
    Die vorhergehende Funktion entspricht der folgenden Funktion, nachdem Sie die BETWEEN- und NOT BETWEEN-Operatoren durch die entsprechenden AND- und OR-Ausdrücke ersetzt haben.

    ```tsql
    CREATE FUNCTION dbo.fn_stretchpredicate_example4(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 >= 0 AND @column1 <= 100AND (@column2 < 200 OR @column2 > 300 OR @column1 = 50)
    GO
    ```

## Beispiele für Filterprädikate, die nicht gültig sind

-   Die folgende Funktion ist ungültig, da sie eine nicht deterministische Konvertierung enthält.

    ```tsql
    CREATE FUNCTION dbo.fn_example5(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 < CONVERT(datetime, '1/1/2016')
    GO
    ```

-   Die folgende Funktion ist ungültig, da sie einen nicht deterministischen Funktionsaufruf enthält.

    ```tsql
    CREATE FUNCTION dbo.fn_example6(@column1 datetime)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 < DATEADD(day, -60, GETDATE())
    GO
    ```

-   Die folgende Funktion ist ungültig, da sie eine Unterabfrage enthält.

    ```tsql
    CREATE FUNCTION dbo.fn_example7(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 IN (SELECT SupplierID FROM Supplier WHERE Status = 'Defunct'))
    GO
    ```

-   Die folgenden Funktionen sind ungültig, da Ausdrücke mit algebraischen Operatoren oder integrierten Funktionen beim Definieren der Funktion zu einer Konstanten ausgewertet werden müssen. Sie dürfen keine Spaltenverweise algebraischen Ausdrücken oder Funktionsaufrufen einschließen.

    ```tsql
    CREATE FUNCTION dbo.fn_example8(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE @column1 % 2 =  0
    GO

    CREATE FUNCTION dbo.fn_example9(@column1 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE SQRT(@column1) = 30
    GO
    ```

-   Die folgende Funktion ist ungültig, da sie die hier beschriebenen Regeln verletzt, nachdem Sie den BETWEEN-Operator mit dem entsprechenden AND-Ausdruck ersetzt haben.

    ```tsql
    CREATE FUNCTION dbo.fn_example10(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE (@column1 BETWEEN 1 AND 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```
    Die vorhergehende Funktion entspricht der folgenden Funktion, nachdem Sie den BETWEEN-Operatoren durch den entsprechenden AND-Ausdruck ersetzt haben. Diese Funktion ist ungültig, da primitive Bedingungen nur den logischen OR-Operator verwenden können.

    ```tsql
    CREATE FUNCTION dbo.fn_example11(@column1 int, @column2 int)
    RETURNS TABLE
    WITH SCHEMABINDING
    AS
    RETURN	SELECT 1 AS is_eligible
    		WHERE (@column1 >= 1 AND @column1 <= 200 OR @column1 = 300) AND @column2 > 1000
    GO
    ```

## Anwenden des Filterprädikats durch Stretch-Datenbank
Stretch-Datenbank wendet das Filterprädikat mithilfe des CROSS APPLY-Operators auf die Tabelle an und bestimmt berechtigte Zeilen. Beispiel:

```tsql
SELECT * FROM stretch_table_name CROSS APPLY fn_stretchpredicate(column1, column2)
```
Wenn die Funktion ein nicht leeres Ergebnis für die Zeile zurückgibt, ist die Zeile für eine Migration berechtigt.

## <a name="replacePredicate"></a>Ersetzen eines vorhandenen Filterprädikats
Sie können ein zuvor angegebenes Filterprädikat ersetzen, indem Sie die ALTER TABLE-Anweisung erneut ausführen und einen neuen Wert für den FILTER\_PREDICATE-Parameter angeben. Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
	FILTER_PREDICATE = dbo.fn_stretchpredicate2(column1, column2),
	MIGRATION_STATE = <desired_migration_state>
```
Für die neue Inline-Tabellenwertfunktion gelten die folgenden Voraussetzungen.

-   Die neue Funktion muss weniger restriktiv als die vorherige Funktion sein.

-   Alle Operatoren, die in der alten Funktion vorhanden waren, müssen auch in der neuen Funktion vorhanden sein.

-   Die neue Funktion darf keine Operatoren enthalten, die nicht in der alten Funktion vorhanden sind.

-   Der Reihenfolge der Operatorargumente darf nicht geändert werden.

-   Nur konstante Werte, die Teil eines `<, <=, >, >=`-Vergleichs sind, können so geändert werden, dass das Prädikat weniger restriktiv ist.

### Beispiel für einen gültigen Ersatz
Setzen Sie voraus, dass die folgende Funktion das aktuelle Filterprädikat ist.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_old (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
			AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist ein gültiger Ersatz, da die neuen Datumskonstante (die ein späteres Umstellungsdatum angibt) das Prädikat weniger restriktiv macht.

```tsql
CREATE FUNCTION dbo.fn_stretchpredicate_new (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE @column1 < CONVERT(datetime, '2/1/2016', 101)
			AND (@column2 < -50 OR @column2 > 50)
GO
```

### Beispiele für ungültige Ersatzmöglichkeiten
Die folgende Funktion ist kein gültiger Ersatz, da die neuen Datumskonstante (die ein späteres Umstellungsdatum angibt) das Prädikat nicht weniger restriktiv macht.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_1 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE @column1 < CONVERT(datetime, '1/1/2015', 101)
			AND (@column2 < -100 OR @column2 > 100)
GO
```
Die folgende Funktion ist kein gültiger Ersatz, da einer der Vergleichsoperatoren entfernt wurde.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_2 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
			AND (@column2 < -50)
GO
```
Die folgende Funktion ist kein gültiger Ersatz, da eine neue Bedingung mit dem logischen AND-Operator hinzugefügt wurde.

```tsql
CREATE FUNCTION dbo.fn_notvalidreplacement_3 (@column1 datetime, @column2 int)
RETURNS TABLE
WITH SCHEMABINDING
AS
RETURN	SELECT 1 AS is_eligible
		WHERE @column1 < CONVERT(datetime, '1/1/2016', 101)
			AND (@column2 < -100 OR @column2 > 100)
			AND (@column2 <> 0)
GO
```

## Entfernen eines Filterprädikats aus einer Tabelle
Um die gesamte Tabelle anstelle der ausgewählten Zeilen zu migrieren, entfernen Sie das vorhandene FILTER\_PREDICATE, indem Sie es auf null festlegen. Beispiel:

```tsql
ALTER TABLE stretch_table_name SET ( REMOTE_DATA_ARCHIVE = ON (
	FILTER_PREDICATE = NULL,
	MIGRATION_STATE = <desired_migration_state>
) )
```
Nachdem Sie das Filterprädikat entfernt haben, sind alle Zeilen in der Tabelle für die Migration berechtigt. Sie können daher nicht später ein Filterprädikat für die gleiche Tabelle angeben, es sei denn, Sie holen zunächst alle Remotedaten für die Tabelle aus Azure zurück. Diese Einschränkung soll die Situation vermeiden, dass Zeilen, die nicht zur Migration berechtigt sind, wenn Sie ein neues Filterprädikat bereitstellen, bereits in Azure migriert wurden.

## Überprüfen des auf eine Tabelle angewendeten Filterprädikats
Öffnen Sie die Katalogsicht **sys.remote\_data\_archive\_tables**, und überprüfen Sie den Wert der Spalte **filter\_predicate**, um das auf eine Tabelle angewendete Filterprädikat zu prüfen. Wenn der Wert null ist, ist die gesamte Tabelle für die Archivierung berechtigt. Weitere Informationen finden Sie unter [sys.remote\_data\_archive\_tables (Transact-SQL)](https://msdn.microsoft.com/library/dn935003.aspx).

## Sicherheitshinweise für Filterprädikate  
Ein kompromittiertes Konto mit db\_owner-Berechtigungen kann folgende Aktionen ausführen:

-   Erstellen und Übernehmen einer Tabellenwertfunktion, die große Mengen an Serverressourcen verbraucht oder für einen längeren Zeitraum wartet, was zu einem Denial of Service führt.  

-   Erstellen und Übernehmen einer Tabellenwertfunktion, die es ermöglicht, den Inhalt einer Tabelle abzuleiten, für die dem Benutzer explizit der Lesezugriff verweigert wurde.

## Weitere Informationen

[ALTER TABLE (Transact-SQL)](https://msdn.microsoft.com/library/ms190273.aspx)

<!---HONumber=AcomDC_0615_2016-->