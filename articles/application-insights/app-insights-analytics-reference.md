<properties 
	pageTitle="Referenz in Analytics in Application Insights" 
	description="Referenz für Anweisungen in Analytics, dem leistungsfähigen Suchtool von Application Insights." 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="04/18/2016" 
	ms.author="awills"/>

# Referenz für Analytics

[Analytics](app-insights-analytics.md) ist die leistungsfähige Suchfunktion von [Application Insights](app-insights-overview.md). Auf diesen Seiten wird die Analytics-Abfragesprache beschrieben.


[AZURE.INCLUDE [app-insights-analytics-top-index](../../includes/app-insights-analytics-top-index.md)]


| | | | | 
|---|---|---|---|---
|[ago](#ago)|[dayofweek](#dayofweek)|[let-Klausel](#let-clause)|[rand](#rand)|[sum](#sum)
|[beliebig](#any)|[dcount](#dcount)|[limit-Operator](#limit-operator)|[range](#range)|[summarize-Operator](#summarize-operator)
|[argmax](#argmax)|[Dynamische Objekte in Let-Klauseln](#dynamic-objects-in-let-clauses)|[log](#log)|[range-Operator](#range-operator)|[take-Operator](#take-operator)
|[argmin](#argmin)|[exp](#exp)|[makelist](#makelist)|[reduce-Operator](#reduce-operator)|[todatetime](#todatetime)
|[Arithmetische Operatoren](#arithmetic-operators)|[extend-Operator](#extend-operator)|[makeset](#makeset)|[render-Anweisung](#render-directive)|[todouble](#todouble)
|[Array- und Objektliterale](#array-and-object-literals)|[extract](#extract)|[max](#max)|[replace](#replace)|[todynamic](#todynamic)
|[arraylength](#arraylength)|[extractjson](#extractjson)|[min](#min)|[restrict-Klausel](#restrict-clause)|[toint](#toint)
|[avg](#avg)|[floor](#floor)|[mvexpand-Operator](#mvexpand-operator)|[Skalare Vergleiche](#scalar-comparisons)|[tolong](#tolong)
|[bin](#bin)|[getmonth](#getmonth)|[notempty](#notempty)|[sort-Operator](#sort-operator)|[tolower](#tolower)
|[Boolesche Literale](#boolean-literals)|[gettype](#gettype)|[notnull](#notnull)|[split](#split)|[top-Operator](#top-operator)
|[Boolesche Operatoren](#boolean-operators)|[getyear](#getyear)|[now](#now)|[sqrt](#sqrt)|[totimespan](#totimespan)
|[buildschema](#buildschema)|[hash](#hash)|[Numerische Literale](#numeric-literals)|[startofmonth](#startofmonth)|[toupper](#toupper)
|[Typumwandlungen](#casts)|[iff](#iff)|[Verborgene Zeichenfolgenliterale](#obfuscated-string-literals)|[startofyear](#startofyear)|[treepath](#treepath)
|[count](#count)|[isempty](#isempty)|[parse-Operator](#parse-operator)|[stdev](#stdev)|[union-Operator](#union-operator)
|[count-Operator](#count-operator)|[isnotempty](#isnotempty)|[parsejson](#parsejson)|[strcat](#strcat)|[variance](#variance)
|[countif](#countif)|[isnotnull](#isnotnull)|[percentile](#percentile)|[Zeichenfolgenvergleiche](#string-comparisons)|[where-Operator](#where-operator)
|[countof](#countof)|[isnull](#isnull)|[percentiles](#percentiles)|[Zeichenfolgenliterale](#string-literals)
|[Datum und Uhrzeit – Ausdrücke](#date-and-time-expressions)|[join-Operator](#join-operator)|[project-Operator](#project-operator)|[strlen](#strlen)
|[Datum und Uhrzeit – Literale](#date-and-time-literals)|[JSON Path-Ausdrücke](#json-path-expressions)|[project-away-Operator](#project-away-operator)|[substring](#substring)


## Abfragen und Operatoren

Eine Abfrage in Ihrer Telemetrie besteht aus einen Verweis auf einen Quelldatenstrom, gefolgt von einer Pipeline von Filtern. Beispiel:


```AIQL
requests // The request table starts this pipeline.
| where client_City == "London" // filter the records
   and timestamp > ago(3d)
| count 
```
    
Jeder Filter mit einem senkrechten Strich `|` als Präfix ist eine Instanz eines *Operators* mit einigen Parametern. Die Eingabe des Operators ist die Tabelle, die das Ergebnis der vorhergehenden Pipeline ist. In den meisten Fällen sind alle Parameter [skalare Ausdrücke](##scalars) über die Spalten der Eingabe. In einigen Fällen sind die Parameter die Namen von Eingabespalten, und in einigen Fällen ist der Parameter eine zweite Tabelle. Das Ergebnis einer Abfrage ist immer eine Tabelle, auch wenn sie nur eine Spalte und eine Zeile enthält.

Abfragen können einzelne Zeilenumbrüche enthalten, aber sie werden mit einer Leerzeile beendet. Unter Umständen enthalten sie Kommentare zwischen `//` und dem Ende der Zeile.

Eine Abfrage kann eine oder mehrere [let-Klauseln](#let-clause) als Präfix aufweisen, die Skalare, Tabellen oder Funktionen definieren, die in der Abfrage verwendet werden können.

```AIQL

    let interval = 3d ;
    let city = "London" ;
    let req = (city:string) {
      requests
      | where client_City == city and timestamp > ago(interval) };
    req(city) | count
```

> `T` wird in den folgenden Abfragebeispielen verwendet, um die vorhergehende Pipeline oder Quelltabelle anzugeben.
> 

### count-Operator

Der `count`-Operator gibt die Anzahl von Datensätzen (Zeilen) in der Eingabe-Datensatzgruppe an.

**Syntax**

    T | count

**Argumente**

* *T*: Die tabellarischen Daten, deren Datensätze gezählt werden.

**Rückgabe**

Diese Funktion gibt eine Tabelle mit einem einzelnen Datensatz und einer Spalte vom Typ `long` zurück. Der Wert der einzigen Zelle ist die Anzahl der Datensätze in *T*.

**Beispiel**

```AIQL
requests | count
```



### extend-Operator

     T | extend duration = stopTime - startTime

Fügen Sie eine oder mehrere berechnete Spalten an eine Tabelle an.


**Syntax**

    T | extend ColumnName = Expression [, ...]

**Argumente**

* *T*: Die Eingabetabelle.
* *ColumnName*: Der Name einer hinzuzufügenden Spalte. Bei [Namen](#names) muss Groß-/Kleinschreibung beachtet werden. Sie können alphabetische oder numerische Zeichen oder das Zeichen „\_“ enthalten. Verwenden Sie `['...']` oder `["..."]` zum Angeben von Schlüsselwörtern oder Namen mit anderen Zeichen.
* *Expression*: Eine Berechnung über die vorhandenen Spalten.

**Rückgabe**

Eine Kopie der Eingabetabelle mit den angegebenen zusätzlichen Spalten.

**Tipps**

* Verwenden Sie stattdessen [`project`](#project-operator), wenn Sie auch einige Spalten löschen oder umbenennen möchten.
* Verwenden Sie nicht einfach `extend`, um einen kürzeren Namen zur Verwendung in einem langen Ausdruck zu erhalten. `...| extend x = anonymous_user_id_from_client | ... func(x) ...` 

    Die systemeigenen Spalten der Tabelle wurden indiziert; der neue Name definiert eine zusätzliche, nicht indizierte Spalte, d. h. die Abfrage wird wahrscheinlich langsamer ausgeführt.

**Beispiel**

```AIQL
traces
| extend
    Age = now() - timestamp
```


### join-Operator

    Table1 | join (Table2) on CommonColumn

Führt die Zeilen zweier Tabellen anhand von übereinstimmenden Werten der angegebenen Spalte zusammen.


**Syntax**

    Table1 | join [kind=Kind] \(Table2) on CommonColumn [, ...]

**Argumente**

* *Table1*: Die „linke Seite“ der Verknüpfung.
* *Table2*: Die „rechte Seite“ der Verknüpfung. Kann ein geschachtelter Abfrageausdruck sein, der eine Tabelle ausgibt.
* *CommonColumn*: Eine Spalte mit dem gleichen Namen in beiden Tabellen.
* *Kind*: Gibt an, wie Zeilen aus den beiden Tabellen abgeglichen werden.

**Rückgabe**

Eine Tabelle mit:

* Einer Spalte für jede Spalte in jeder der beiden Tabellen, einschließlich der übereinstimmenden Schlüssel. Die Spalten der rechten Seite werden bei Namenskonflikten automatisch umbenannt.
* Einer Zeile für jede Übereinstimmung zwischen den Eingabetabellen. Eine Übereinstimmung ist eine ausgewählte Zeile in einer Tabelle, die in allen `on`-Feldern denselben Wert aufweist wie eine Zeile in der anderen Tabelle. 

* `Kind` – nicht angegeben

    Nur eine Zeile der linken Seite wird für jeden Wert des `on`-Schlüssels abgeglichen. Die Ausgabe enthält eine Zeile für jede Übereinstimmung dieser Zeile mit Zeilen der rechten Seite.

* `Kind=inner`
 
     Der Ausgabe enthält eine Zeile für jede Kombination von übereinstimmenden Zeilen der linken und rechten Seite.

* `kind=leftouter` (oder `kind=rightouter` oder `kind=fullouter`)

     Zusätzlich zu den internen Übereinstimmungen ist eine Zeile für jede Zeile auf der linken (und/oder rechten) Seite vorhanden, selbst wenn es keine Übereinstimmung damit gibt. In diesem Fall enthalten die Ausgabezellen ohne Übereinstimmung NULL-Werte.

* `kind=leftanti`

     Gibt alle Datensätze von der linken Seite zurück, für die keine Übereinstimmungen auf der rechten Seite vorhanden sind. Die Ergebnistabelle enthält nur die Spalten von der linken Seite.
 
Wenn mehrere Zeilen mit denselben Werten für diese Felder vorhanden sind, erhalten Sie Zeilen für alle Kombinationen.

**Tipps**

Für eine optimale Leistung:

* Verwenden Sie `where` und `project`, um die Anzahl von Zeilen und Spalten in den Eingabetabellen vor der Verknüpfung (`join`) zu reduzieren. 
* Wenn eine Tabelle immer kleiner als die andere ist, verwenden Sie diese als die linke (weitergeleitete) Seite der Verknüpfung.
* Die Spalten für die Verknüpfungsübereinstimmung müssen den gleichen Namen haben. Verwenden Sie ggf. den project-Operator, um eine Spalte in einer der Tabellen umzubenennen.

**Beispiel**

Abrufen erweiterter Aktivitäten aus einem Protokoll, in dem einige Einträge den Start und das Ende einer Aktivität markieren

```AIQL
    let Events = MyLogTable | where type=="Event" ;
    Events
    | where Name == "Start"
    | project Name, City, ActivityId, StartTime=timestamp
    | join (Events
           | where Name == "Stop"
           | project StopTime=timestamp, ActivityId)
        on ActivityId
    | project City, ActivityId, StartTime, StopTime, Duration, StopTime, StartTime

```

### let-Klausel

**Tabellarisches let – Benennen einer Tabelle**

    let recentReqs = requests | where timestamp > ago(3d); 
    recentReqs | count

**Skalares let – Benennen eines Werts**

    let interval = 3d; 
    requests | where timestamp > ago(interval)

**Lambda-let – Benennen einer Funktion**

    let Recent = 
       (interval:timespan) { requests | where timestamp > ago(interval) };
    Recent(3h) | count

Eine let-Klausel bindet einen [Namen](#names) an ein tabellarisches Ergebnis, einen Skalarwert oder eine Funktion. Die Klausel ist das Präfix einer Abfrage, und diese Abfrage ist der Bereich der Bindung. (Die let-Klausel bietet keine Möglichkeit, Dinge zu benennen, die Sie später in der Sitzung verwenden.)

**Syntax**

    let name = scalar_constant_expression ; query

    let name = query ; query

    let name = (parameterName : type [, ...]) { plain_query }; query

* *type*: `bool`, `int`, `long`, `double`, `string`, `timespan`, `datetime`, `guid`, [`dynamic`](#dynamic-type)
* *plain\_query*: Eine Abfrage ohne vorangestellte let-Klausel.

**Beispiele**

    let rows(n:long) = range steps from 1 to n step 1;
    rows(10) | ...


Selbstverknüpfung:

    let Recent = events | where timestamp > ago(7d);
    Recent | where name contains "session_started" 
    | project start = timestamp, session_id
    | join (Recent 
        | where name contains "session_ended" 
        | project stop = timestamp, session_id)
      on session_id
    | extend duration = stop - start 

### limit-Operator

     T | limit 5

Gibt höchstens die angegebene Anzahl von Zeilen aus der Eingabetabelle zurück. Es gibt keine Garantie dafür, welche Datensätze zurückgegeben werden. (Verwenden Sie [`top`](#top-operator), um bestimmte Datensätze zurückzugeben.)

**Alias** `take`

**Syntax**

    T | limit NumberOfRows


**Tipps**

`Take` ist eine einfache und effiziente Möglichkeit, ein Beispiel für die Ergebnisse anzuzeigen, wenn Sie interaktiv arbeiten. Denken Sie daran, dass es keine Garantie dafür gibt, dass bestimmte Zeilen erzeugt bzw. in einer bestimmten Reihenfolge erzeugt werden.

Für die Anzahl von Zeilen, die an den Client zurückgegeben werden, besteht eine implizite Begrenzung, auch wenn Sie `take` nicht verwenden. Um diese Begrenzung aufzuheben, verwenden Sie die Clientanforderungsoption `notruncation`.



### mvexpand-Operator

    T | mvexpand listColumn 

Erweitert eine Liste aus einer Zelle mit dynamischer Typisierung (JSON) erweitert, sodass jeder Eintrag eine separate Zeile enthält. Alle anderen Zellen in einer erweiterten Zeile sind dupliziert.

(Siehe auch [`summarize makelist`](#summarize-operator), wodurch die gegenteilige Funktion durchgeführt wird.)

**Beispiel**

Angenommen, die Eingabetabelle ist:

|A:int|B:string|D:dynamic|
|---|---|---|
|1|"hello"|{"key":"value"}|
|2|"world"|[0,1,"k","v"]|

    mvexpand D

Das Ergebnis lautet:

|A:int|B:string|D:dynamic|
|---|---|---|
|1|"hello"|{"key":"value"}|
|2|"world"|0|
|2|"world"|1|
|2|"world"|"k"|
|2|"world"|"v"|


**Syntax**

    T | mvexpand  [bagexpansion=(bag | array)] ColumnName [limit Rowlimit]

    T | mvexpand  [bagexpansion=(bag | array)] [Name =] ArrayExpression [to typeof(Typename)] [limit Rowlimit]

**Argumente**

* *ColumnName*: Im Ergebnis werden Arrays in der benannten Spalte auf mehrere Zeilen erweitert. 
* *ArrayExpression*: Ein Ausdruck, der ein Array zurückgibt. Bei Verwendung dieses Formulars wird eine neue Spalte hinzugefügt, und die vorhandene wird beibehalten.
* *Name:* Ein Name für die neue Spalte.
* *Typename:* Wandelt den erweiterten Ausdruck in einen bestimmten Typ um.
* *RowLimit:* Die maximale Anzahl von Zeilen, die aus jeder ursprünglichen Zeile generiert werden. Der Standardwert ist 128.

**Rückgabe**

Mehrere Zeilen für alle Werte in jedem Array in der benannten Spalte oder im Arrayausdruck.

Die erweiterte Spalte ist immer dynamisch typisiert. Verwenden Sie eine Umwandlung wie `todatetime()` oder `toint()`, wenn Sie Werte berechnen oder aggregieren möchten.

Zwei Erweiterungsmodi für Eigenschaftenbehälter werden unterstützt:

* `bagexpansion=bag`: Eigenschaftenbehälter werden zu Eigenschaftenbehältern mit einem einzelnen Eintrag erweitert. Dies ist die Standarderweiterung.
* `bagexpansion=array`: Eigenschaftenbehälter werden zu Arraystrukturen mit zwei Elementen `[`*Schlüssel*`,`*Wert*`]` erweitert und lassen den einheitlichen Zugriff auf Schlüssel und Werte zu (sowie z.B. auch das Ausführen einer distinct-count-Aggregation über Eigenschaftsnamen). 

**Beispiele**


    exceptions | take 1 
    | mvexpand details[0]

Teilt einen Ausnahmedatensatz für jedes Element im Feld „Details“ in Zeilen auf.



### parse-Operator

    T | parse "I am 63 next birthday" with "I am" Year:int "next birthday"

    T | parse kind=regex "My 62nd birthday" 
        with "My" Year:regex("[0..9]+") regex("..") "birthday"

Extrahiert Werte aus einer Zeichenfolge. Kann einfache oder reguläre Ausdrücke für Übereinstimmungen verwenden.

Die Elemente in der `with`-Klausel werden wiederum mit der Quellzeichenfolge verglichen. Jedes Element entnimmt ein Segment des Quelltexts. Handelt es sich dabei um eine einfache Zeichenfolge, bewegt sich der Abgleich-Cursor so weit wie die Übereinstimmung. Handelt es sich um eine Spalte mit einem Typnamen, bewegt sich der Cursor weit genug, um den angegebenen Typ zu analysieren. (Zeichenfolgenübereinstimmungen bewegen sich, bis eine Übereinstimmung mit dem nächsten Element gefunden wird.) Handelt es sich um einen RegEx, wird der reguläre Ausdruck abgeglichen (und die resultierende Spalte ist immer vom Typ „Zeichenfolge“).

**Syntax**

    T | parse StringExpression with [SimpleMatch | Column:Type] ...

    T | parse kind=regex StringExpression 
        with [SimpleMatch | Column : regex("Regex")] ...

**Argumente**

* *T*: Die Eingabetabelle.
* *kind*: einfach oder regex. Die Standardeinstellung ist „einfach“.
* *StringExpression*: Ein Ausdruck, der als Zeichenfolge ausgewertet wird oder in eine Zeichenfolge konvertiert werden kann.
* *SimpleMatch*: Eine Zeichenfolge, die mit dem nächsten Teil des Texts übereinstimmt.
* *Column*: Gibt die neue Spalte an, der eine Übereinstimmung zugewiesen wird.
* *Type*: Gibt an, wie der nächste Teil der Quellzeichenfolge zu analysieren ist.
* *Regex*: Ein regulärer Ausdruck für den Abgleich des nächsten Teils der Zeichenfolge. 

**Rückgabe**

Die Eingabetabelle, erweitert gemäß der Liste der Spalten.


**Beispiele**

Der `parse`-Operator bietet eine optimierte Methode zum Erweitern einer Tabelle mit `extend` durch Verwendung mehrerer `extract`-Anwendungen in demselben `string`-Ausdruck. Dies ist besonders hilfreich, wenn die Tabelle eine `string`-Spalte mit mehreren Werten enthält, die Sie in einzelne Spalten aufteilen möchten, z.B. eine Spalte, die von einer Entwickler-Ablaufverfolgungsanweisung („`printf`“/„`Console.WriteLine`“) erzeugt wurde.

Im folgenden Beispiel wird angenommen, dass die Spalte `EventNarrative` der Tabelle `StormEvents` Zeichenfolgen im Format `{0} at {1} crested at {2} feet around {3} on {4} {5}` enthält. Der folgende Vorgang erweitert die Tabelle mit zwei Spalten: `SwathSize` und `FellLocation`.


|EventNarrative|
|---|
|Der Green River erreichte bei Brownsville am 12. Dezember um 09.30 Uhr EST den Höchststand von 5,7 m (The Green River at Brownsville crested at 18.8 feet around 0930EST on December 12). Die Hochwassergrenze bei Brownsville beträgt 5,5 m (Flood stage at Brownsville is 18 feet). Bei diesem Pegel tritt eine geringfügige Überflutung auf (Minor flooding occurs at this level). Der Fluss tritt über Schleusenwände und flutet einige der tiefer gelegenen Uferlandschaften und landwirtschaftlichen Nutzflächen (The river overflows lock walls and some of the lower banks, along with some agricultural bottom land).|
|Der Fluss Rolling Fork erreichte bei Boston am 12. Dezember um 17.00 Uhr EST den Höchststand von 12 m (The Rolling Fork River at Boston crested at 39.3 feet around 1700EST on December 12). Die Hochwassergrenze bei Boston beträgt 10,7 m (Flood stage at Boston is 35 feet). Bei diesem Pegel tritt eine geringfügige Überflutung auf, bei der einige landwirtschaftliche Nutzflächen in der Flussniederung unter Wasser stehen (Minor flooding occurs at this level, with some agricultural bottom land covered).|
|Der Green River erreichte bei Woodbury am 16. Dezember um 06.00 Uhr EST den Höchststand von 11,2 m (The Green River at Woodbury crested at 36.7 feet around 0600EST on December 16). Die Hochwassergrenze bei Woodbury beträgt 10 m (Flood stage at Woodbury is 33 feet). Bei diesem Pegel tritt eine geringfügige Überflutung auf, bei der einige Tiefebenen um die Stadt unter Wasser stehen (Minor flooding occurs at this level, with some lowlands around the town of Woodbury covered with water).|
|Der Ohio erreichte bei Tell Stadt am 18. Dezember um 07.00 Uhr EST den Höchststand von 11,9 m (The Ohio River at Tell City crested at 39.0 feet around 7 AM EST on December 18). Die Hochwassergrenze bei Tell Stadt beträgt 11,6 m (Flood stage at Tell City is 38 feet). Bei diesem Pegel beginnt der Fluss, die darüber gelegenen Uferlandschaften zu überfluten (At this level, the river begins to overflow its banks above the gage). Der Indiana Highway 66 wird zwischen Rome und Derby überflutet (Indiana Highway 66 floods between Rome and Derby).|

```AIQL

StormEvents 
|  parse EventNarrative 
   with RiverName:string 
        "at" 
        Location:string 
        "crested at" 
        Height:double  
        "feet around" 
        Time:string 
        "on" 
        Month:string 
        " " 
        Day:long 
        "." 
        notImportant:string
| project RiverName , Location , Height , Time , Month , Day

```

|RiverName|Ort|Höhe|Time|Monat|Tag|
|---|---|---|---|---|---|
|Green River | Woodbury |11,2| 06\.00 Uhr EST | Dezember|16|
|Fluss Rolling Fork | Boston |12| 17\.00 Uhr EST | Dezember|12|
|Green River | Brownsville |5,7| 09\.30 Uhr EST | Dezember|12|
|Ohio River | Tell Stadt |11,9| 7\.00 Uhr EST | Dezember|18|

Die Verwendung von regulären Ausdrücken für den Abgleich ist ebenfalls möglich. Dies führt zum gleichen Ergebnis, alle Ergebnisspalten sind jedoch vom Typ „Zeichenfolge“:

```AIQL

StormEvents
| parse kind=regex EventNarrative 
  with RiverName:regex("(\\s?[a-zA-Z]+\\s?)+") 
  "at" Location:regex(".*") 
  "crested at " Height:regex("\\d+\\.\\d+") 
  " feet around" Time:regex(".*") 
  "on " Month:regex("(December|November|October)") 
   " " Day:regex("\\d+") 
   "." notImportant:regex(".*")
| project RiverName , Location , Height , Time , Month , Day
```


### project-Operator

    T | project cost=price*quantity, price

Wählen Sie die Spalten aus, die einbezogen, umbenannt oder gelöscht werden sollen, und fügen Sie neue berechnete Spalten ein. Die Reihenfolge der Spalten im Ergebnis wird durch die Reihenfolge der Argumente festgelegt. Nur die in den Argumenten angegebenen Spalten sind im Ergebnis enthalten: alle anderen Spalten in der Eingabe werden gelöscht. (Siehe auch `extend`.)


**Syntax**

    T | project ColumnName [= Expression] [, ...]

**Argumente**

* *T*: Die Eingabetabelle.
* *ColumnName*: Der Name einer Spalte, der in der Ausgabe angezeigt wird. Wenn kein *Ausdruck* vorhanden ist, muss die Eingabe eine Spalte mit diesem Namen enthalten. Bei [Namen](#names) muss Groß-/Kleinschreibung beachtet werden. Sie können alphabetische oder numerische Zeichen oder das Zeichen „\_“ enthalten. Verwenden Sie `['...']` oder `["..."]` zum Angeben von Schlüsselwörtern oder Namen mit anderen Zeichen.
* *Expression*: Optionaler skalarer Ausdruck, der auf die Eingabespalten verweist. 

    Das Zurückgeben einer neuen berechneten Spalte mit dem gleichen Namen wie eine vorhandene Spalte der Eingabe ist zulässig.

**Rückgabe**

Eine Tabelle, deren Spalten als Argumente benannt sind, und die ebenso viele Zeilen wie die Eingabetabelle aufweist.

**Beispiel**

Das folgende Beispiel zeigt mehrere Arten von Manipulationen, die mithilfe des `project`-Operators durchgeführt werden können. Die Eingabetabelle `T` umfasst drei Spalten vom Typ `int`: `A`, `B` und `C`.

```AIQL
T
| project
    X=C,               // Rename column C to X
    A=2*B,             // Calculate a new column A from the old B
    C=strcat("-",tostring(C)), // Calculate a new column C from the old C
    B=2*B,              // Calculate a new column B from the old B
    ['where'] = client_City // rename, using a keyword as a column name
```

### project-away-Operator

    T | project-away column1, column2, ...

Schließt bestimmte Spalten aus. Das Ergebnis enthält alle Eingabespalten mit Ausnahme der angegebenen Spalten.

### range-Operator

    range LastWeek from ago(7d) to now() step 1d

Erzeugt eine einspaltige Tabelle mit Werten. Beachten Sie, dass keine Pipeline-Eingabe vorhanden ist.

|LastWeek|
|---|
|2015-12-05 09:10:04.627|
|2015-12-06 09:10:04.627|
|...|
|2015-12-12 09:10:04.627|



**Syntax**

    range ColumnName from Start to Stop step Step

**Argumente**

* *ColumnName*: Der Name der einzelnen Spalte in der Ausgabetabelle.
* *Start*: Der kleinste Wert in der Ausgabe.
* *Stop*: Der höchste in der Ausgabe generierte Wert (oder eine Grenze für den höchsten Wert, wenn *step* diesen Wert überschreitet).
* *Step*: Die Differenz zwischen zwei aufeinanderfolgenden Werten. 

Die Argumente müssen numerische Werte, Datums- oder TimeSpan-Werte sein. Sie können nicht auf die Spalten einer Tabelle verweisen. (Wenn Sie den Bereich basierend auf einer Eingabetabelle berechnen möchten, verwenden Sie die [range*-Funktion*](#range), vielleicht mit dem [mvexpand-Operator](#mvexpand-operator).)

**Rückgabe**

Eine Tabelle mit einer einzelnen Spalte namens *ColumnName*, deren Werte *Start*, *Start* + *Step*, ... bis einschließlich *Stop* lauten.

**Beispiel**

```AIQL
range Steps from 1 to 8 step 3
```

Eine Tabelle mit einer einzelnen Spalte namens `Steps` vom Typ `long`, deren Werte `1`, `4` und `7` sind.

**Beispiel**

    range LastWeek from bin(ago(7d),1d) to now() step 1d

Eine Tabelle von Mitternacht der vergangenen sieben Tage. Die bin (floor)-Funktion reduziert jede Zeit auf den Anfang des Tages.

**Beispiel**

```AIQL
range timestamp from ago(4h) to now() step 1m
| join kind=fullouter
  (traces
      | where timestamp > ago(4h)
      | summarize Count=count() by bin(timestamp, 1m)
  ) on timestamp
| project Count=iff(isnull(Count), 0, Count), timestamp
| render timechart  
```

Zeigt, wie mit dem `range`-Operator eine kleine Ad-hoc-Dimensionstabelle erstellt werden kann, mit der anschließend Nullen eingeführt werden, wenn die Quelldaten keine Werte aufweisen.

### reduce-Operator

    exceptions | reduce by outerMessage

Versucht, ähnliche Datensätze zu gruppieren. Der Operator gibt für jede Gruppe das Muster (`Pattern`) aus, das die Gruppe wahrscheinlich am besten beschreibt, sowie die Anzahl (`Count`) der Datensätze in dieser Gruppe.


![](./media/app-insights-analytics-queries/reduce.png)

**Syntax**

    T | reduce by  ColumnName [ with threshold=Threshold ]

**Argumente**

* *ColumnName*: Die zu untersuchende Spalte. Sie muss vom Typ „Zeichenfolge“ sein.
* *Threshold*: Ein Wert im Bereich {0..1}. Der Standardwert ist 0,001. Bei großen Eingaben sollte der Schwellenwert klein sein. 

**Rückgabe**

Zwei Spalten, `Pattern` und `Count`. In vielen Fällen ist das Muster ein vollständiger Wert aus der Spalte. In einigen Fällen kann es allgemeine Begriffe identifizieren und die Variablenteile durch „*“ ersetzen.

Das Ergebnis von `reduce by city` kann z.B. Folgendes enthalten:

|Muster | Count |
|---|---|
| San * | 5182 |
| Saint * | 2846 |
| Moskau | 3726 |
| * -auf- * | 2730 |
| Paris | 27163 |


### render-Anweisung

    T | render [ table | timechart  | barchart | piechart ]

„Render“ weist die Darstellungsschicht an, wie die Tabelle angezeigt werden soll. Es sollte das letzte Element der Pipe sein. Es ist eine praktische Alternative zum Verwenden der Steuerelemente in der Anzeige, und Sie können eine Abfrage mit einer bestimmten Präsentationsmethode speichern.

### restrict-Klausel 

Gibt die Tabellennamen an, die für die folgenden Operatoren zur Verfügung stehen. Beispiel:

    let e1 = requests | project name, client_City;
    let e2 =  requests | project name, success;
    // Exclude predefined tables from the union:
    restrict access to (e1, e2);
    union * |  take 10 

### sort-Operator 

    T | sort by country asc, price desc

Sortiert die Zeilen der Eingabetabelle in Reihenfolge nach einer oder mehreren Spalten.

**Alias** `order`

**Syntax**

    T  | sort by Column [ asc | desc ] [ `,` ... ]

**Argumente**

* *T:*: Die zu sortierende Tabelleneingabe.
* *Column*: Spalte von *T*, nach der sortiert werden soll. Der Typ der Werte muss „Numerisch“, „Datum“, „Uhrzeit“ oder „Zeichenfolge“ sein.
* `asc` Sortierung in aufsteigender Reihenfolge von niedrig nach hoch. Die Standardeinstellung ist `desc`, von hoch zu niedrig.

**Beispiel**

```AIQL
Traces
| where ActivityId == "479671d99b7b"
| sort by Timestamp asc
```
Alle Zeilen in der Tabelle „Traces“ mit einer bestimmten `ActivityId`, sortiert nach ihrem Zeitstempel.

### summarize-Operator

Erzeugt eine Tabelle, die den Inhalt der Eingabetabelle aggregiert.
 
    requests
	| summarize count(), avg(duration), makeset(client_City) 
      by client_CountryOrRegion

Tabelle, die die Anzahl, durchschnittliche Anforderungsdauer und Menge von Städten in jedem Land zeigt. Die Ausgabe enthält eine Zeile für jedes unterschiedliche Land. Die Ausgabespalten zeigen die Anzahl, durchschnittliche Dauer, Städte und das Land. Alle anderen Eingabespalten werden ignoriert.


    T | summarize count() by price_range=bin(price, 10.0)

Eine Tabelle, die zeigt, wie viele Elemente in jedem Intervall [0, 10,0], [10,0, 20,0] usw. Preise aufweisen. In diesem Beispiel ist eine Spalte für die Anzahl und eine für den Preisbereich vorhanden. Alle anderen Eingabespalten werden ignoriert.


**Syntax**

    T | summarize
         [  [ Column = ] Aggregation [ `,` ... ] ]
         [ by
            [ Column = ] GroupExpression [ `,` ... ] ]

**Argumente**

* *Column*: Optionaler Name für eine Ergebnisspalte. Nimmt standardmäßig den vom Ausdruck abgeleiteten Namen an. Bei [Namen](#names) muss Groß-/Kleinschreibung beachtet werden. Sie können alphabetische oder numerische Zeichen oder das Zeichen „\_“ enthalten. Verwenden Sie `['...']` oder `["..."]` zum Angeben von Schlüsselwörtern oder Namen mit anderen Zeichen.
* *Aggregation*: Ein Aufruf einer Aggregationsfunktion wie z.B. `count()` oder `avg()` mit Spaltennamen als Argumente. Informationen hierzu finden Sie unter [Aggregationen](#aggregations).
* *GroupExpression*: Ein Ausdruck für die Spalten, der einen Satz von unterschiedlichen Werten bereitstellt. Normalerweise handelt es sich entweder um einen Spaltennamen, der bereits einen eingeschränkten Satz von Werten bereitstellt, oder um `bin()` mit einer numerischen Spalte oder Zeitspalte als Argument. 

Wenn Sie einen numerischen Ausdruck oder Zeitausdruck ohne `bin()` bereitstellen, wendet Analytics ihn automatisch mit einem Intervall von `1h` für Uhrzeiten oder von `1.0` für Zahlen an.

Wenn Sie keine *GroupExpression* angeben, wird die gesamte Tabelle in einer einzelnen Ausgabezeile zusammengefasst.



**Rückgabe**

Die Eingabezeilen sind in Gruppen mit den gleichen Werten der `by`-Ausdrücke angeordnet. Anschließend werden die angegebenen Aggregationsfunktionen über jede Gruppe berechnet, dabei wird eine Zeile für jede Gruppe erzeugt. Das Ergebnis enthält die `by`-Spalten und auch mindestens eine Spalte für jedes berechnete Aggregat. (Einige Aggregationsfunktionen geben mehrere Spalten zurück.)

Das Ergebnis umfasst ebenso viele Zeilen wie unterschiedliche Kombinationen von `by`-Werten vorhanden sind. Wenn Sie Zusammenfassungen über Bereiche von numerischen Werten erstellen möchten, verwenden Sie `bin()`, um Bereiche auf diskrete Werte zu reduzieren.

**Hinweis**

Auch wenn Sie beliebige Ausdrücke für die Aggregation und Gruppierung von Ausdrücken bereitstellen können, ist es effizienter, einfache Spaltennamen zu verwenden oder `bin()` auf eine numerische Spalte anzuwenden.



### take-Operator

Alias von [limit](#limit-operator).


### top-Operator

    T | top 5 by Name desc

Gibt die ersten *N* Datensätze nach den angegebenen Spalten sortiert zurück.


**Syntax**

    T | top NumberOfRows by Sort_expression [ `asc` | `desc` ] [, ... ]

**Argumente**

* *NumberOfRows*: Die zurückzugebende Anzahl der Zeilen von *T*.
* *Sort\_expression*: Ein Ausdruck, nach dem die Zeilen sortiert werden. Dies ist in der Regel nur ein Spaltenname. Sie können mehrere „Sort\_expression“-Angaben machen.
* Möglicherweise wird `asc` oder `desc` (Standard) angezeigt, um zu steuern, ob die tatsächliche Auswahl am „unteren“ oder „oberen“ Ende des Bereichs erfolgt.


**Tipps**

`top 5 by name` entspricht oberflächlich betrachtet `sort by name | take 5`. Allerdings ist die Ausführung schneller, und es werden immer sortierte Ergebnisse zurückgegeben; dies ist mit `take` nicht garantiert.


### union-Operator

     Table1 | union Table2, Table3

Verwendet mindestens zwei Tabellen und gibt die Zeilen aller Tabellen zurück.

**Syntax**

    T | union [ kind= inner | outer ] [ withsource = ColumnName ] Table2 [ , ...]  

    union [ kind= inner | outer ] [ withsource = ColumnName ] Table1, Table2 [ , ...]  

**Argumente**

* *Table1*, *Table2* ...
 *  Der Name einer Tabelle, wie z.B. `requests`, oder eine in einer [let-Klausel](#let-clause) definierte Tabelle; oder
 *  Ein Abfrageausdruck, wie z.B. `(requests | where success=="True")`.
 *  Ein Satz von Tabellen, die mit einem Platzhalterzeichen angegeben sind. `e*` bildet z.B. die Union aller Tabellen, die in den vorhergehenden let-Klauseln definiert wurden und deren Namen mit „e“ beginnt, zusammen mit der Tabelle der Ausnahmen.
* `kind`: 
 * `inner`: Das Ergebnis enthält die Teilmenge der Spalten, die in allen Eingabetabellen vorkommen.
 * `outer`: Das Ergebnis enthält alle Spalten, die in einer der Eingaben vorkommen. Zellen, die nicht durch eine Eingabezeile definiert wurden, werden auf `null` gesetzt.
* `withsource=`*ColumnName*: Wenn diese Einstellung festgelegt ist, enthält die Ausgabe eine Spalte namens *ColumnName*, deren Wert angibt, aus welchen Quelltabellen die einzelnen Zeilen stammen.

**Rückgabe**

Eine Tabelle, deren Anzahl an Zeilen der Anzahl in allen Eingabetabellen entspricht und deren Anzahl an Spalten mit der Anzahl an eindeutigen Spaltennamen in den Eingaben übereinstimmt.

**Beispiel**

```AIQL

let ttrr = requests | where timestamp > ago(1h);
let ttee = exceptions | where timestamp > ago(1h);
union tt* | count
```
Union aller Tabellen, deren Namen mit „tt“ beginnen.


**Beispiel**

```AIQL

union withsource=SourceTable kind=outer Query, Command
| where Timestamp > ago(1d)
| summarize dcount(UserId)
```
Die Anzahl der unterschiedlichen Benutzer, die während des Vortags entweder ein `exceptions`- oder ein `traces`-Ereignis herbeigeführt haben. Im Ergebnis gibt die Spalte „SourceTable“ entweder „Abfrage“ oder „Befehl“ an.

```AIQL
exceptions
| where Timestamp > ago(1d)
| union withsource=SourceTable kind=outer 
   (Command | where Timestamp > ago(1d))
| summarize dcount(UserId)
```

Mit dieser effizienteren Version wird dasselbe Ergebnis erzielt. Jede Tabelle wird vor dem Erstellen der Union gefiltert.

### where-Operator

     T | where fruit=="apple"

Filtert eine Tabelle auf die Teilmenge der Zeilen, die ein Prädikat erfüllen.

**Alias** `filter`

**Syntax**

    T | where Predicate

**Argumente**

* *T*: Die tabellarische Eingabe, deren Datensätze gefiltert werden sollen.
* *Predicate*: Ein `boolean`-[Ausdruck](#boolean) über die Spalten von *T*. Er wird für jede Zeile in *T* ausgewertet.

**Rückgabe**

Zeilen in *T*, für die *Predicate* `true` ist.

**Tipps**

So erzielen Sie die optimale Leistung:

* **Verwenden Sie einfache Vergleiche** zwischen Spaltennamen und Konstanten. („Konstant“ bedeutet konstant innerhalb der Tabelle, d.h. `now()` und `ago()` sind ebenso in Ordnung wie skalare Werte, die mithilfe einer [`let`-Klausel](#let-clause) zugewiesen werden.)

    Beispielsweise wird `where Timestamp >= ago(1d)` gegenüber `where floor(Timestamp, 1d) == ago(1d)` bevorzugt.

* **Einfachste Begriffe zuerst**: Wenn Sie mehrere mit `and` verbundene Klauseln haben, stellen Sie die Klauseln, die nur eine Spalte umfassen, an den Anfang. `Timestamp > ago(1d) and OpId == EventId` ist also besser als anders herum.


**Beispiel**

```AIQL
Traces
| where Timestamp > ago(1h)
    and Source == "Kuskus"
    and ActivityId == SubActivityIt 
```

Datensätze, die nicht älter als 1 Stunde sind, aus der Quelle namens „Kuskus“ stammen und zwei Spalten mit dem gleichen Wert aufweisen.

Beachten Sie, dass wir den Vergleich zwischen zwei Spalten an das Ende stellen, da der Index nicht genutzt werden kann und ein Scan erzwungen wird.



## Aggregrationen

Aggregationen dienen zum Kombinieren von Werten in Gruppen, die im [summarize-Vorgang](#summarize-operator) erstellt werden. In dieser Abfrage ist z.B. dcount() eine Aggregatfunktion:

    requests | summarize dcount(name) by success

### beliebig 

    any(Expression)

Wählt eine Zeile der Gruppe nach dem Zufallsprinzip aus, und gibt den Wert des angegebenen Ausdrucks zurück.

Dies empfiehlt sich beispielsweise, wenn eine Spalte über eine große Anzahl von ähnlichen Werten verfügt (z. B. eine Spalte „Fehlertext“) und Sie einmal pro eindeutigem Wert für den zusammengesetzten Gruppenschlüssel Stichproben aus dieser Spalte abrufen möchten.

**Beispiel**

```

traces 
| where timestamp > now(-15min)  
| summarize count(), any(message) by operation_Name 
| top 10 by count_level desc 
```

<a name="argmin"></a> <a name="argmax"></a>
### argmin, argmax

    argmin(ExprToMinimize, * | ExprToReturn  [ , ... ] )
    argmax(ExprToMaximize, * | ExprToReturn  [ , ... ] ) 

Findet eine Zeile in der Gruppe, die *ExprToMaximize* minimiert/maximiert, und gibt den Wert von *ExprToReturn* zurück (oder `*`, um die gesamte Zeile zurückzugeben).

**Tipp**: Die durchlaufenen Spalten werden automatisch umbenannt. Um sicherzustellen, dass Sie die richtigen Namen verwenden, überprüfen Sie die Ergebnisse mit `take 5`, bevor Sie die Ergebnisse an einen anderen Operator weiterreichen.

**Beispiele**

Zeigen Sie für jeden Anforderungsnamen, wann die längste Anforderung aufgetreten ist:

    requests | summarize argmax(duration, timestamp) by name

Zeigen Sie alle Details der längsten Anforderung, nicht nur den Zeitstempel:

    requests | summarize argmax(duration, *) by name


Suchen Sie den niedrigsten Wert jeder Metrik, zusammen mit dem Zeitstempel und anderen Daten:

    metrics 
    | summarize minValue=argmin(value, *) 
      by name


![](./media/app-insights-analytics-aggregations/argmin.png)
 


### avg

    avg(Expression)

Berechnet den Durchschnitt von *Expression* in der Gruppe.

### buildschema

    buildschema(DynamicExpression)

Gibt das minimale Schema zurück, das alle Werte von *DynamicExpression* zulässt.

Der Parameterspaltentyp sollte `dynamic` sein, d.h. ein Array oder ein Eigenschaftenbehälter.

**Beispiel**

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass mit `indexer` markiert wird, an welcher Stelle Sie einen numerischen Index verwenden sollten. Einige gültige Pfade für dieses Schema (vorausgesetzt diese Beispielindizes befinden sich im Bereich):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)

**Beispiel**

Wenn die Eingabespalte drei dynamische Werte hat:

| |
|---|
|`{"x":1, "y":3.5}`
|`{"x":"somevalue", "z":[1, 2, 3]}`
|`{"y":{"w":"zzz"}, "t":["aa", "bb"], "z":["foo"]}`


Lautet das resultierende Schema folgendermaßen:

    { 
      "x":["int", "string"], 
      "y":["double", {"w": "string"}], 
      "z":{"`indexer`": ["int", "string"]}, 
      "t":{"`indexer`": "string"} 
    }

Das Schema weist auf Folgendes hin:

* Das Stammobjekt ist ein Container mit den vier Eigenschaften x, y, z und t.
* Die Eigenschaft „x“ kann entweder den Typ „int“ oder „string“ haben.
* Bei der Eigenschaft „y“ kann es sich entweder um den Typ „double“ handeln oder um einen anderen Container mit einer Eigenschaft namens „w“ vom Typ „string“.
* Das ``indexer``-Schlüsselwort gibt an, dass „z“ und „t“ Arrays sind.
* Jedes Element im Array „z“ ist entweder eine ganze Zahl oder eine Zeichenfolge (string).
* „t“ ist ein Array von Zeichenfolgen.
* Jede Eigenschaft ist implizit optional, und jedes Array kann leer sein.

##### Schemamodell

Die Syntax des zurückgegebenen Schemas lautet folgendermaßen:

    Container ::= '{' Named-type* '}';
    Named-type ::= (name | '"`indexer`"') ':' Type;
	Type ::= Primitive-type | Union-type | Container;
    Union-type ::= '[' Type* ']';
    Primitive-type ::= "int" | "string" | ...;

Sie entsprechen einer Teilmenge der TypeScript-Typanmerkungen, die als dynamischer Wert codiert sind. In TypeScript würde das Beispielschema folgendermaßen lauten:

    var someobject: 
    { 
      x?: (number | string), 
      y?: (number | { w?: string}), 
      z?: { [n:number] : (int | string)},
      t?: { [n:number]: string } 
    }


### count

    count([ Predicate ])

Gibt die Anzahl von Zeilen zurück, für die *Predicate* als `true` ausgewertet wird. Wenn *Predicate* nicht angegeben ist, wird die Gesamtzahl von Datensätzen in der Gruppe zurückgegeben.

**Leistungstipp**: Verwenden Sie `summarize count(filter)` anstelle von `where filter | summarize count()`.

> [AZURE.NOTE] Vermeiden Sie count() zum Ermitteln der Anzahl von Anforderungen, Ausnahmen oder anderen Ereignissen, die aufgetreten sind. Wenn gerade ein [Sampling](app-insights-sampling.md) durchgeführt wird, ist die Anzahl von Datenpunkten geringer als die Anzahl von tatsächlichen Ereignissen. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die Eigenschaft „itemCount“ gibt die Anzahl von ursprünglichen Ereignissen wieder, die von jedem vermerkten Datenpunkt dargestellt werden.

### countif

    countif(Predicate)

Gibt die Anzahl von Zeilen zurück, für die *Predicate* als `true` ausgewertet wird.

**Leistungstipp**: Verwenden Sie `summarize countif(filter)` anstelle von `where filter | summarize count()`.

> [AZURE.NOTE] Verwenden Sie „countif()“ nicht zum Ermitteln der Anzahl von Anforderungen, Ausnahmen oder anderen Ereignissen, die aufgetreten sind. Wenn gerade ein [Sampling](app-insights-sampling.md) durchgeführt wird, ist die Anzahl von Datenpunkten geringer als die Anzahl von tatsächlichen Ereignissen. Verwenden Sie stattdessen `summarize sum(itemCount)...`. Die Eigenschaft „itemCount“ gibt die Anzahl von ursprünglichen Ereignissen wieder, die von jedem vermerkten Datenpunkt dargestellt werden.

### dcount

    dcount( Expression [ ,  Accuracy ])

Gibt eine Schätzung der Anzahl von unterschiedlichen Werten für *Expr* in der Gruppe zurück. (Verwenden Sie zum Auflisten der unterschiedlichen Werte [`makeset`](#makeset).)

Mit *Accuracy* wird, sofern angegeben, der Ausgleich zwischen Geschwindigkeit und Genauigkeit gesteuert.

 * `0` ist die am wenigsten präzise und schnellste Berechnung.
 * `1` ist die Standardeinstellung, die Genauigkeit und Berechnungszeit ausgleicht; etwa 0,8% Fehlerwahrscheinlichkeit.
 * `2` ist die präziseste und langsamste Berechnung; etwa 0,4% Fehlerwahrscheinlichkeit.

**Beispiel**

    pageViews 
    | summarize cities=dcount(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-aggregations/dcount.png)

### makelist

    makelist(Expr [ ,  MaxListSize ] )

Gibt ein `dynamic`-Array (JSON) aller Werte von *Expr* in der Gruppe zurück.

* *MaxListSize* ist eine optionale Integer-Beschränkung für die maximale Anzahl zurückgegebener Elemente (Standardwert: *128*).

### makeset

    makeset(Expression [ , MaxSetSize ] )

Gibt ein `dynamic`-Array (JSON) des Satzes von unterschiedlichen Werten zurück, die *Expr* in der Gruppe annimmt. (Tipp: Verwenden Sie zum Zählen der unterschiedlichen Werte [`dcount`](#dcount).)
  
*  *MaxSetSize* ist eine optionale Integer-Beschränkung für die maximale Anzahl zurückgegebener Elemente (Standardwert: *128*).

**Beispiel**

    pageViews 
    | summarize cities=makeset(client_City) 
      by client_CountryOrRegion

![](./media/app-insights-analytics-aggregations/makeset.png)

Siehe auch den [`mvexpand`-Operator](#mvexpand-operator) für die umgekehrte Funktion.


### max, min

    max(Expr)

Berechnet das Maximum von *Expr*.
    
    min(Expr)

Berechnet das Minimum von *Expr*.

**Tipp**: Damit erhalten Sie die Mindest- oder Maximalwerte, z.B. den höchsten oder niedrigsten Preis. Wenn Sie jedoch andere Spalten in der Zeile abrufen möchten, z.B. den Namen des Lieferanten mit dem niedrigsten Preis, verwenden Sie [argmin oder argmax](#argmin-argmax).


<a name="percentile"></a> <a name="percentiles"></a>
### percentile, percentiles

    percentile(Expression, Percentile)

Gibt eine Schätzung für *Expression* des angegebenen Quantils in der Gruppe zurück. Die Genauigkeit hängt von der Bevölkerungsdichte in der Region des Quantils ab.
    
    percentiles(Expression, Percentile1 [ , Percentile2 ] )

Ähnlich wie `percentile()`, berechnet jedoch eine Reihe von Quantilwerten (was schneller ist, als jedes Quantil einzeln zu berechnen).

**Beispiele**


Der Wert von `duration`, der größer als 95% und kleiner als 5% des Beispielsatzes ist, berechnet für jeden Anforderungsnamen:

    request 
    | summarize percentile(duration, 95)
      by name

Lassen Sie „by...“ aus, um den Wert für die gesamte Tabelle zu berechnen.

Berechnen Sie gleichzeitig mehrere Quantile für andere Anforderungsnamen:

    
    requests 
    | summarize 
        percentiles(duration, 5, 20, 50, 80, 95) 
      by name

![](./media/app-insights-analytics-aggregations/percentiles.png)

Die Ergebnisse zeigen, dass für die Anforderung „/Events/Index“ auf 5 % der Anforderungen in weniger als 2,44 Sekunden reagiert wird, auf die Hälfte in 3,52 Sekunden und auf 5 % langsamer als 6,85 Sekunden.


Berechnen Sie mehrere Statistiken:

    requests 
    | summarize 
        count(), 
        avg(Duration),
        percentiles(Duration, 5, 50, 95)
      by name

##### Schätzungsfehler in Quantilen

Das Quantilaggregat bietet einen ungefähren Wert mithilfe von [T-Digest](https://github.com/tdunning/t-digest/blob/master/docs/t-digest-paper/histo.pdf).

Einige wichtige Punkte:

* Die Grenzen für den Schätzungsfehler variieren je nach dem Wert des angeforderten Quantils. Die beste Genauigkeit erhalten Sie an den Enden der Skala von [0 bis 100]. Die Quantile 0 und 100 sind die Mindest- und Maximalwerte für die Verteilung. Die Genauigkeit nimmt zur Mitte der Skala hin ab. Am Mittelpunkt ist die Genauigkeit am unpräzisesten und auf 1 % begrenzt. 
* Fehlergrenzen werden in Bezug auf den Rang, nicht auf den Wert sichtbar. Beispiel: Quantil(X, 50) hat den Wert Xm zurückgegeben. Die Schätzung garantiert, dass mindestens 49 % und höchstens 51 % der Werte von X kleiner sind als Xm. Es gibt keine theoretische Beschränkung hinsichtlich des Unterschieds zwischen Xm und dem tatsächlichen Mittelwert von X.

### stdev

     stdev(Expr)

Gibt die Standardabweichung von *Expr* für die Gruppe zurück.

### variance

    variance(Expr)

Gibt die Varianz von *Expr* für die Gruppe zurück.

### sum

    sum(Expr)

Gibt die Summe von *Expr* für die Gruppe zurück.


## Skalare

[casts](#casts) | [comparisons](#scalar-comparisons) <br/> [gettype](#gettype) | [hash](#hash) | [iff](#iff)| [isnull](#isnull) | [isnotnull](#isnotnull) | [notnull](#notnull)

Die unterstützten Typen sind:

| Typ | Weitere(r) Name(n) | Entsprechender .NET-Typ |
| --------- | -------------------- | -------------------- |
| `bool` | `boolean` | `System.Boolean` |
| `datetime`| `date` | `System.DateTime` |
| `dynamic` | | `System.Object` |
| `guid` | `uuid`, `uniqueid` | `System.Guid` |
| `int` | | `System.Int32` |
| `long` | | `System.Int64` |
| `double` | `real` | `System.Double` |
| `string` | | `System.String` |
| `timespan`| `time` | `System.TimeSpan` |

### Typumwandlungen

Sie können einen Typ in einen anderen umwandeln. Wenn die Konvertierung sinnvoll ist, wird sie im Allgemeinen auch funktionieren:

    todouble(10), todouble("10.6")
    toint(10.6) == 11
    floor(10.6) == 10
	toint("200")
    todatetime("2016-04-28 13:02")
    totimespan("1.5d"), totimespan("1.12:00:00")
    toguid("00000000-0000-0000-0000-000000000000")
    tostring(42.5)
    todynamic("{a:10, b:20}")

### Skalare Vergleiche

||
---|---
`<` |Kleiner
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Not Equals
`!=`|Not Equals 
`in`| Rechter Operand ist ein (dynamisches) Array, und linker Operand entspricht einem seiner Elemente.
`!in`| Rechter Operand ist ein (dynamisches) Array, und linker Operand entspricht nicht einem seiner Elemente.




### gettype

**Rückgabe**

Eine Zeichenfolge, die den zugrunde liegenden Speichertyp des einzigen Arguments darstellt. Dies ist besonders dann nützlich, wenn Sie über Werte vom Typ `dynamic` verfügen: In diesem Fall zeigt `gettype()`, wie ein Wert codiert ist.

**Beispiele**

|||
---|---
`gettype("a")` |`"string" `
`gettype(111)` |`"long" `
`gettype(1==1)` |`"int8" (*) `
`gettype(now())` |`"datetime" `
`gettype(1s)` |`"timespan" `
`gettype(parsejson('1'))` |`"int" `
`gettype(parsejson(' "abc" '))` |`"string" `
`gettype(parsejson(' {"abc":1} '))` |`"dictionary"` 
`gettype(parsejson(' [1, 2, 3] '))` |`"array"` 
`gettype(123.45)` |`"real" `
`gettype(guid(12e8b78d-55b4-46ae-b068-26d7a0080254))` |`"guid"` 
`gettype(parsejson(''))` |`"null"`



### hash

**Syntax**

    hash(source [, mod])

**Argumente**

* *source*: Der Quellskalar, mit dem der Hash berechnet wird.
* *mod*: Der Modulowert, der auf das Hashergebnis angewendet werden soll.

**Rückgabe**

Der xxhash (long)-Wert des angegebenen Skalars, Modulo des angegebenen mod-Werts (sofern angegeben).

**Beispiele**

```
hash("World")                   // 1846988464401551951
hash("World", 100)              // 51 (1846988464401551951 % 100)
hash(datetime("2015-01-01"))    // 1380966698541616202
```
### iff

Die `iff()`-Funktion wertet das erste Argument (Prädikat) aus und gibt entweder den Wert der zweiten oder dritten Argumente zurück, abhängig davon, ob das Prädikat `true` oder `false` ist. Die zweiten und dritten Argumente müssen vom gleichen Typ sein.

**Syntax**

    iff(predicate, ifTrue, ifFalse)


**Argumente**

* *predicate*: Ein Ausdruck, der als `boolean`-Wert ausgewertet wird.
* *ifTrue*: Ein Ausdruck, der ausgewertet und dessen Wert von der Funktion zurückgegeben wird, wenn *predicate* als `true` ausgewertet wird.
* *ifFalse*: Ein Ausdruck, der ausgewertet und dessen Wert von der Funktion zurückgegeben wird, wenn *predicate* als `false` ausgewertet wird.

**Rückgabe**

Diese Funktion gibt den Wert von *ifTrue* zurück, wenn *predicate* als `true` ausgewertet wird, oder andernfalls den Wert von *ifFalse*.

**Beispiel**

```
iff(floor(timestamp, 1d)==floor(now(), 1d), "today", "anotherday")
```

<a name="isnull"/></a> <a name="isnotnull"/></a> <a name="notnull"/></a>
### isnull, isnotnull, notnull

    isnull(parsejson("")) == true

Akzeptiert ein einzelnes Argument und gibt an, ob es null ist.

**Syntax**


    isnull([value])


    isnotnull([value])


    notnull([value])  // alias for isnotnull

**Rückgabe**

True oder False, je nachdem, ob ist der Wert null oder nicht null ist.


|x|isnull(x)
|---|---
| "" | false
|"x" | false
|parsejson("")|true
|parsejson("")|false
|parsejson("{}")|false

**Beispiel**

    T | where isnotnull(PossiblyNull) | count

Beachten Sie, dass es andere Möglichkeiten gibt, diesen Effekt zu erreichen:

    T | summarize count(PossiblyNull)




## Boolean 

### Boolesche Literale

	true == 1
    false == 0
    gettype(true) == "int8"
    typeof(bool) == typeof(int8)

### Boolesche Operatoren

	and 
    or 

    

## Zahlen

[bin](#bin) | [floor](#floor) | [rand](#rand) | [range](#range) | [sqrt](#sqrt) | [todouble](#todouble) | [toint](#toint) | [tolong](#tolong)

### Numerische Literale

|||
|---|---
|`42`|`long`
|`42.0`|`real`

### Arithmetische Operatoren

|| |
|---|-------------|
| + | Hinzufügen |
| - | Subtrahieren | 
| * | Multiplizieren | 
| / | Dividieren | 
| % | Modulo | 
|| 
|`<` |Kleiner 
|`<=`|Kleiner oder gleich 
|`>` |Größer 
|`>=`|Größer oder gleich 
|`<>`|Ungleich 
|`!=`|Ungleich




### bin

Rundet Werte auf eine ganze Zahl ab, die ein Vielfaches der angegebenen bin-Größe ist. Wird häufig in der [`summarize by`](#summarize-operator)-Abfrage verwendet. Wenn Sie über einen verstreuten Satz von Werten verfügen, werden sie zu einem kleineren Satz bestimmter Werte gruppiert.

Alias `floor`.

**Syntax**

     bin(value, roundTo)

**Argumente**

* *value*: Eine Zahl, ein Datum oder ein Zeitraum. 
* *roundTo*: Die bin-Größe. Eine Zahl, ein Datum oder ein Zeitraum zum Teilen des Werts (*value*). 

**Rückgabe**

Das nächste Vielfache von *roundTo* unter dem Wert (*value*).
 
    (toint((value/roundTo)-0.5)) * roundTo

**Beispiele**

Ausdruck | Ergebnis
---|---
`bin(4.5, 1)` | `4.0`
`bin(time(16d), 7d)` | `14d`
`bin(datetime(1953-04-15 22:25:07), 1d)`| `datetime(1953-04-15)`


Der folgende Ausdruck berechnet ein Histogramm der Dauer mit einer Bucketgröße von 1 Sekunde:

```AIQL

    T | summarize Hits=count() by bin(Duration, 1s)
```
### exp

    exp(v)   // e raised to the power v
    exp2(v)  // 2 raised to the power v
    exp10(v) // 10 raised to the power v



### floor

Ein Alias für [`bin()`](#bin).


### log

    log(v)    // Natural logarithm of v
    log2(v)   // Logarithm base 2 of v
    log10(v)  // Logarithm base 10 of v


`v` sollte eine reelle Zahl sein, die größer als 0 ist. Andernfalls wird Null zurückgegeben.

### rand

Ein Zufallszahlengenerator.

* `rand()`: eine reelle Zahl zwischen 0,0 und 1,0.
* `rand(n)`: eine ganze Zahl zwischen 0 und n-1.




### sqrt

Die Quadratwurzelfunktion.

**Syntax**

    sqrt(x)

**Argumente**

* *x:* eine reelle Zahl >= 0.

**Rückgabe**

* Eine positive Zahl, sodass `sqrt(x) * sqrt(x) == x`.
* `null`, wenn das Argument negativ ist oder nicht in einen `real`-Wert konvertiert werden kann. 




### toint

    toint(100)        // cast from long
    toint(20.7) == 21 // nearest int from double
    toint(20.4) == 20 // nearest int from double
    toint("  123  ")  // parse string
    toint(a[0])       // cast from dynamic
    toint(b.c)        // cast from dynamic

### tolong

    tolong(20.7) == 21 // conversion from double
    tolong(20.4) == 20 // conversion from double
    tolong("  123  ")  // parse string
    tolong(a[0])       // cast from dynamic
    tolong(b.c)        // cast from dynamic


### todouble

    todouble(20) == 20.0 // conversion from long or int
    todouble(" 12.34 ")  // parse string
    todouble(a[0])       // cast from dynamic
    todouble(b.c)        // cast from dynamic



## Datum und Uhrzeit


[ago](#ago) | [dayofweek](#dayofweek) | [getmonth](#getmonth)| [getyear](#getyear) | [now](#now) | [startofmonth](#startofmonth) | [startofyear](#startofyear) | [todatetime](#todatetime) | [totimespan](#totimespan)

### Datum und Uhrzeit – Literale

|||
---|---
**datetime**|
`datetime("2015-12-31 23:59:59.9")`<br/>`datetime("2015-12-31")`|Zeiten werden immer in UTC angegeben. Durch das Auslassen des Datums wird eine Zeit am heutigen Tag angegeben.
`now()`|Die aktuelle Zeit.
`now(`-*timespan*`)`|`now()-`*timespan*
`ago(`*timespan*`)`|`now()-`*timespan*
**timespan**|
`2d`|2 Tage
`1.5h`|1,5 Stunden 
`30m`|30 Minuten
`10s`|10 Sekunden
`0.1s`|0,1 Sekunde
`100ms`| 100 Millisekunden
`10microsecond`|
`1tick`|100 ns
`time("15 seconds")`|
`time("2")`| 2 Tage
`time("0.12:34:56.7")`|`0d+12h+34m+56.7s`

### Datum und Uhrzeit – Ausdrücke

Ausdruck |Ergebnis
---|---
`datetime("2015-01-02") - datetime("2015-01-01")`| `1d`
`datetime("2015-01-01") + 1d`| `datetime("2015-01-02")`
`datetime("2015-01-01") - 1d`| `datetime("2014-12-31")`
`2h * 24` | `2d`
`2d` / `2h` | `24`
`datetime("2015-04-15T22:33") % 1d` | `timespan("22:33")`
`bin(datetime("2015-04-15T22:33"), 1d)` | `datetime("2015-04-15T00:00")`
||
`<` |Kleiner
`<=`|Kleiner oder gleich
`>` |Größer
`>=`|Größer oder gleich
`<>`|Not Equals
`!=`|Not Equals 




### ago

Subtrahiert den angegebenen Zeitraum von der aktuellen UTC-Uhrzeit. Wie `now()` kann diese Funktion mehrmals in einer Anweisung verwendet werden, und die UTC-Uhrzeit, auf die verwiesen wird, ist für alle Instanziierungen identisch.

**Syntax**

    ago(a_timespan)

**Argumente**

* *a\_timespan*: Intervall, das von der aktuellen UTC-Uhrzeit (`now()`) subtrahiert werden soll.

**Rückgabe**

    now() - a_timespan

**Beispiel**

Alle Zeilen mit einem Zeitstempel der letzten Stunde:

```AIQL

    T | where timestamp > ago(1h)
```



### dayofweek

    dayofweek(datetime("2015-12-14")) == 1d  // Monday

Die Anzahl von Tagen (als ganze Zahl) seit dem vorherigen Sonntag als `timespan`-Element.

**Syntax**

    dayofweek(a_date)

**Argumente**

* `a_date`: Ein `datetime`-Wert.

**Rückgabe**

`timespan` seit Mitternacht zu Beginn des vorhergehenden Sonntags, abgerundet auf die Anzahl von Tagen als ganze Zahl.

**Beispiele**

```AIQL
dayofweek(1947-11-29 10:00:05)  // time(6.00:00:00), indicating Saturday
dayofweek(1970-05-11)           // time(1.00:00:00), indicating Monday
```

### getmonth

Rufen Sie die Monatsnummer (1-12) aus einem datetime-Wert ab.

**Beispiel**

    ... | extend month = getmonth(datetime(2015-10-12))

    --> month == 10

### getyear

Rufen Sie das Jahr aus einem datetime-Wert ab.

**Beispiel**

    ... | extend year = getyear(datetime(2015-10-12))

    --> year == 2015

### now

    now()
    now(-2d)

Die aktuelle UTC-Uhrzeit, die optional durch einen angegebenen Zeitraum versetzt wird. Diese Funktion kann mehrmals in einer Anweisung verwendet werden, und die Uhrzeit, auf die verwiesen wird, ist für alle Instanzen identisch.

**Syntax**

    now([offset])

**Argumente**

* *offset:* Ein `timespan`-Wert, der der aktuellen UTC-Uhrzeit hinzugefügt wird. Standard: 0.

**Rückgabe**

Die aktuelle UTC-Uhrzeit als `datetime`.

    now() + offset

**Beispiel**

Bestimmt das Intervall seit dem vom Prädikat identifizierten Ereignis:

```AIQL
T | where ... | extend Elapsed=now() - timestamp
```

### startofmonth

    startofmonth(date)

Der Beginn des Monats, der das Datum enthält.

### startofyear

    startofyear(date)

Der Beginn des Jahres, das das Datum enthält.


### todatetime

Alias `datetime()`.

     todatetime("2016-03-28")
     todatetime("03/28/2016")
     todatetime("2016-03-28 14:34")
     todatetime("03/28/2016 2:34pm")
     todatetime("2016-03-28T14:34.5Z")
     todatetime(a[0])  // cast a dynamic type
     todatetime(b.c)   // cast a dynamic type

### totimespan

Alias `timespan()`.

    totimespan("21d")
    totimespan("21h")
    totimespan(request.duration)


## String

[countof](#countof) | [extract](#extract) | [extractjson](#extractjson) | [isempty](#isempty) | [isnotempty](#isnotempty) | [notempty](#notempty) | [replace](#replace) | [split](#split) | [strcat](#strcat) | [strlen](#strlen) | [substring](#substring) | [tolower](#tolower) | [tostring](#tostring) | [toupper](#toupper)


### Zeichenfolgenliterale

Die Regeln sind mit JavaScript identisch.

Zeichenfolgen können entweder in einfachen oder doppelten Anführungszeichen eingeschlossen sein.

Es wird ein umgekehrter Schrägstrich (``) verwendet, um Zeichen wie z.B. `\t` (Tab), `\n` (Zeilenvorschub) und Instanzen der einschließenden Anführungszeichen mit einem Escapezeichen zu versehen.

* `'this is a "string" literal in single \' quotes'`
* `"this is a 'string' literal in double " quotes"`
* `@"C:\backslash\not\escaped\with @ prefix"`

### Verborgene Zeichenfolgenliterale

Verborgene Zeichenfolgenliterale sind Zeichenfolgen, die Analytics bei der Ausgabe der Zeichenfolge ausblendet (etwa bei der Ablaufverfolgung). Beim Ausblenden werden alle verborgenen Zeichen durch ein Start-Zeichen (`*`) ersetzt.

Stellen Sie zum Erstellen eines verborgenen Zeichenfolgenliterals `h` oder „H“ voran. Beispiel:

```
h'hello'
h@'world' 
h"hello"
```

### Zeichenfolgenvergleiche

Operator|Beschreibung|Groß-/Kleinschreibung|Beispiel für „True“
---|---|---|---
`==`|Equals |Ja| `"aBc" == "aBc"`
`<>`|Ungleich|Ja| `"abc" <> "ABC"`
`=~`|Equals |Nein| `"abc" =~ "ABC"`
`!~`|Ungleich |Nein| `"aBc" !~ "xyz"`
`has`|Rechte Seite (RS) ist ein ganzer Begriff innerhalb der linken Seite (LS)|Nein| `"North America" has "america"`
`!has`|RS ist kein vollständiger Begriff innerhalb der LS|Nein|`"North America" !has "amer"` 
`contains` | RS tritt als Untersequenz der LS auf|Nein| `"FabriKam" contains "BRik"`
`!contains`| RS tritt nicht in LS auf|Nein| `"Fabrikam" !contains "xyz"`
`containscs` | RS tritt als Untersequenz der LS auf|Ja| `"FabriKam" contains "Kam"`
`!containscs`| RS tritt nicht in LS auf|Ja| `"Fabrikam" !contains "Kam"`
`startswith`|RS ist eine anfängliche Untersequenz der LS|Nein|`"Fabrikam" startswith "fab"`
`matches regex`|LS enthält eine Übereinstimmung für RS|Ja| `"Fabrikam" matches regex "b.*k"`


Verwenden Sie `has` oder `in`, wenn Sie auf das Vorhandensein eines gesamten lexikalischen Begriffs hin testen, d.h. ein Symbol oder ein alphanumerisches Wort, begrenzt durch nicht-alphanumerische Zeichen oder den Anfang oder das Ende des Felds. `has` wird schneller ausgeführt als `contains` oder `startswith`. Die erste dieser Abfragen wird schneller ausgeführt:

    EventLog | where continent has "North" | count;
	EventLog | where continent contains "nor" | count





### countof

    countof("The cat sat on the mat", "at") == 3
    countof("The cat sat on the mat", @"\b.at\b", "regex") == 3

Zählt die Vorkommnisse einer Teilzeichenfolge in einer Zeichenfolge. Einfache Zeichenfolgenübereinstimmungen überlappen sich möglicherweise; bei regex-Übereinstimmungen ist dies nicht Fall.

**Syntax**

    countof(text, search [, kind])

**Argumente**

* *text*: Eine Zeichenfolge.
* *search*: Die einfache Zeichenfolge oder der reguläre Ausdruck zum Abgleich in *text*.
* *kind*: `"normal"|"regex"`, Standardwert: `normal`. 

**Rückgabe**

Angabe, wie oft die Suchzeichenfolge im Container abgeglichen werden kann. Einfache Zeichenfolgenübereinstimmungen überlappen sich möglicherweise; bei regex-Übereinstimmungen ist dies nicht Fall.

**Beispiele**

|||
|---|---
|`countof("aaa", "a")`| 3 
|`countof("aaaa", "aa")`| 3 (nicht 2!)
|`countof("ababa", "ab", "normal")`| 2
|`countof("ababa", "aba")`| 2
|`countof("ababa", "aba", "regex")`| 1
|`countof("abcabc", "a.c", "regex")`| 2
    



### extract

    extract("x=([0-9.]+)", 1, "hello x=45.6|wo") == "45.6"

Ruft eine Übereinstimmung für einen [regulären Ausdruck](#regular-expressions) aus einer Textzeichenfolge ab. Optional konvertiert es dann die extrahierte Teilzeichenfolge in den angegebenen Typ.

**Syntax**

    extract(regex, captureGroup, text [, typeLiteral])

**Argumente**

* *regex:* Ein [regulärer Ausdruck](#regular-expressions).
* *captureGroup:* Eine positive `int`-Konstante, die die zu extrahierende Erfassungsgruppe angibt. 0 steht für die vollständige Übereinstimmung, 1 für den mit der ersten „("Klammer")“ übereinstimmenden Wert im regulären Ausdruck, 2 oder höher für nachfolgende Klammern.
* *text:* Ein zu suchender `string`.
* *typeLiteral:* Ein optionales Typliteral (z.B. `typeof(long)`). Die extrahierte Teilzeichenfolge wird, sofern angegeben, in diesen Typ konvertiert. 

**Rückgabe**

Wenn mit *regex* eine Übereinstimmung in *text* gefunden wird: die mit der angegebenen Erfassungsgruppe *captureGroup* abgeglichene Teilzeichenfolge, optional konvertiert in *typeLiteral*.

Wenn keine Übereinstimmung vorhanden ist oder bei der Typkonvertierung ein Fehler auftritt: `null`.

**Beispiele**

Die Beispielzeichenfolge `Trace` wird nach einer Definition für `Duration` durchsucht. Die Übereinstimmung wird in `real` konvertiert und dann mit einer Zeitkonstanten (`1s`) multipliziert, damit `Duration` den Typ `timespan` erhält. In diesem Beispiel entspricht dies 123,45 Sekunden:

```AIQL
...
| extend Trace="A=1, B=2, Duration=123.45, ..."
| extend Duration = extract("Duration=([0-9.]+)", 1, Trace, typeof(real)) * time(1s) 
```

Dieses Beispiel entspricht `substring(Text, 2, 4)`:

```AIQL
extract("^.{2,2}(.{4,4})", 1, Text)
```

<a name="notempty"></a> <a name="isnotempty"></a> <a name="isempty"></a>
### isempty, isnotempty, notempty

    isempty("") == true

„True“, wenn das Argument eine leere Zeichenfolge oder null ist. Siehe auch [isnull](#isnull).


**Syntax**

    isempty([value])


    isnotempty([value])


    notempty([value]) // alias of isnotempty

**Rückgabe**

Gibt an, ob das Argument eine leere Zeichenfolge oder isnull ist.

|x|isempty(x)
|---|---
| "" | true
|"x" | false
|parsejson("")|true
|parsejson("")|false
|parsejson("{}")|false


**Beispiel**


    T | where isempty(fieldName) | count




### replace

Ersetzen Sie alle regex-Übereinstimmungen mit einer anderen Zeichenfolge.

**Syntax**

    replace(regex, rewrite, text)

**Argumente**

* *regex*: Der [reguläre Ausdruck](https://github.com/google/re2/wiki/Syntax) zum Durchsuchen von *text*. Er kann Erfassungsgruppen in „('Klammern')“ enthalten. 
* *rewrite*: Der reguläre Ersatzausdruck für jede Übereinstimmung, die mit *matchingRegex* erzielt wurde. Verwenden Sie `\0`, um auf die gesamte Übereinstimmung zu verweisen, `\1` für die erste Erfassungsgruppe, `\2` usw. für nachfolgende Erfassungsgruppen.
* *text*: Eine Zeichenfolge.

**Rückgabe**

*text* nach dem Ersetzen aller Übereinstimmungen von *regex* durch Auswertungen von *rewrite*. Übereinstimmungen überlappen sich nicht.

**Beispiel**

Diese Anweisung:

```AIQL
range x from 1 to 5 step 1
| extend str=strcat('Number is ', tostring(x))
| extend replaced=replace(@'is (\d+)', @'was: \1', str)
```

Bietet die folgenden Ergebnisse:

| x | str | replaced|
|---|---|---|
| 1 | Nummer lautet 1.000000 | Nummer lautete 1.000000|
| 2 | Nummer lautet 2.000000 | Nummer lautete 2.000000|
| 3 | Nummer lautet 3.000000 | Nummer lautete 3.000000|
| 4 | Nummer lautet 4.000000 | Nummer lautete 4.000000|
| 5 | Nummer lautet 5.000000 | Nummer lautete 5.000000|
 



### split

    split("aaa_bbb_ccc", "_") == ["aaa","bbb","ccc"]

Teilt eine angegebene Zeichenfolge gemäß einem angegebenen Trennzeichen, und gibt ein Zeichenfolgenarray mit den enthaltenen untergeordneten Zeichenfolgen zurück. Optional kann eine bestimmte Teilzeichenfolge zurückgegeben werden, sofern vorhanden.

**Syntax**

    split(source, delimiter [, requestedIndex])

**Argumente**

* *source*: Die Quellzeichenfolge, die dem angegebenen Trennzeichen entsprechend geteilt wird.
* *delimiter*: Das Trennzeichen, das zum Teilen der Quellzeichenfolge verwendet wird.
* *requestedIndex*: Ein optionaler nullbasierter Index `int`. Sofern angegeben, enthält das zurückgegebene Zeichenfolgenarray die angeforderte Teilzeichenfolge, sofern vorhanden. 

**Rückgabe**

Ein Zeichenfolgenarray, das die Teilzeichenfolgen der angegebenen Quellzeichenfolge enthält, die durch das angegebene Trennzeichen getrennt sind.

**Beispiele**

```
split("aa_bb", "_")           // ["aa","bb"]
split("aaa_bbb_ccc", "_", 1)  // ["bbb"]
split("", "_")                // [""]
split("a__b")                 // ["a","","b"]
split("aabbcc", "bb")         // ["aa","cc"]
```




### strcat

    strcat("hello", " ", "world")

Verkettet zwischen 1 und 16 Argumente, bei denen es sich um Zeichenfolgen handeln muss.

### strlen

    strlen("hello") == 5

Länge einer Zeichenfolge.

### substring

    substring("abcdefg", 1, 2) == "bc"

Extrahiert eine Teilzeichenfolge aus einer angegebenen Quellzeichenfolge, beginnend bei einem angegebenen Index. Optional kann die Länge der angeforderten Teilzeichenfolge angegeben werden.

**Syntax**

    substring(source, startingIndex [, length])

**Argumente**

* *source*: Die Quellzeichenfolge, aus der die Teilzeichenfolge entnommen wird.
* *startingIndex*: Die nullbasierte Position des Anfangszeichens der angeforderten Teilzeichenfolge.
* *length*: Ein optionaler Parameter, der zur Angabe der angeforderten Anzahl von Zeichen in der Teilzeichenfolge verwendet werden kann. 

**Rückgabe**

Eine Teilzeichenfolge aus der angegebenen Zeichenfolge. Die Teilzeichenfolge beginnt bei der startingIndex (nullbasierten) Zeichenposition und wird bis zum Ende der Zeichenfolge oder der Längenzeichen fortgesetzt, sofern angegeben.

**Beispiele**

```
substring("123456", 1)        // 23456
substring("123456", 2, 2)     // 34
substring("ABCD", 0, 2)       // AB
```

### tolower

    tolower("HELLO") == "hello"

Konvertiert eine Zeichenfolge in Kleinbuchstaben.

### toupper

    toupper("hello") == "HELLO"

Konvertiert eine Zeichenfolge in Großbuchstaben.



## GUIDs

    guid(00000000-1111-2222-3333-055567f333de)


## Arrays, Objekte und Dynamik

[literals](#dynamic-literals) | [casting](#casting-dynamic-objects) | [operators](#operators) | [let clauses](#dynamic-objects-in-let-clauses) <br/> [arraylength](#arraylength) | [extractjson](#extractjson) | [parsejson](#parsejson) | [range](#range) | [treepath](#treepath) | [todynamic](#todynamic)


Hier ist das Ergebnis einer Abfrage für eine Application Insights-Ausnahme. Der Wert unter `details` ist ein Array.

![](./media/app-insights-analytics-scalars/310.png)

**Indizierung**: Arrays und Objekte werden wie in JavaScript indiziert:

    exceptions | take 1
    | extend 
        line = details[0].parsedStack[0].line,
        stackdepth = arraylength(details[0].parsedStack)

* Verwenden Sie jedoch `arraylength` und andere Analytics-Funktionen (nicht „.length“!).

**Umwandlung**: In einigen Fällen ist es erforderlich, ein Element umzuwandeln, das Sie aus einem Objekt extrahieren, da der Typ variieren kann. `summarize...to` benötigt beispielsweise einen bestimmten Typ:

    exceptions 
    | summarize count() 
      by toint(details[0].parsedStack[0].line)

    exceptions 
    | summarize count() 
      by tostring(details[0].parsedStack[0].assembly)

**Literale**: Um ein explizites Array oder ein Eigenschaftenbehälter-Objekt zu erstellen, schreiben Sie es als JSON-Zeichenfolge mit Typumwandlung:

    todynamic('[{"x":"1", "y":"32"}, {"x":"6", "y":"44"}]')


**mvexpand**: Verwenden Sie „mvexpand“, um die Eigenschaften eines Objekts in separate Zeilen aufzuteilen:

    exceptions | take 1 
    | mvexpand details[0].parsedStack[0]


![](./media/app-insights-analytics-scalars/410.png)


**treepath**: Zum Suchen aller Pfade in einem komplexen Objekt:

    exceptions | take 1 | project timestamp, details 
    | extend path = treepath(details) 
    | mvexpand path


![](./media/app-insights-analytics-scalars/420.png)

**buildschema**: Zum Suchen des minimalen Schemas, das alle Werte des Ausdrucks in der Tabelle zulässt:

    exceptions | summarize buildschema(details)

Ergebnis:

    { "`indexer`":
     {"id":"string",
       "parsedStack":
       { "`indexer`": 
         {  "level":"int",
            "assembly":"string",
            "fileName":"string",
            "method":"string",
            "line":"int"
         }},
      "outerId":"string",
      "message":"string",
      "type":"string",
      "rawStack":"string"
    }}

Beachten Sie, dass mit `indexer` markiert wird, an welcher Stelle Sie einen numerischen Index verwenden sollten. Einige gültige Pfade für dieses Schema (vorausgesetzt diese Beispielindizes befinden sich im Bereich):

    details[0].parsedStack[2].level
    details[0].message
    arraylength(details)
    arraylength(details[0].parsedStack)



### Array- und Objektliterale

Verwenden Sie Folgendes zum Erstellen eines dynamischen Literals `parsejson` (Alias `todynamic`) mit einem JSON-Zeichenfolgenargument:

* `parsejson('[43, 21, 65]')`: ein Array von Zahlen
* `parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')` 
* `parsejson('21')`: ein einzelner Wert vom Typ „dynamisch“ mit einer Zahl
* `parsejson('"21"')`: ein einzelner Wert vom Typ „dynamisch“ mit einer Zeichenfolge

Beachten Sie, dass bei JSON, im Gegensatz zu JavaScript, die Verwendung doppelter Anführungszeichen (`"`) um Zeichenfolgen unbedingt erforderlich ist. Daher ist es im Allgemeinen einfacher, ein JSON-codiertes Zeichenfolgenliteral mit einfachen Anführungszeichen (`'`) zu kennzeichnen.

In diesem Beispiel wird ein dynamischer Wert erstellt, und anschließend werden dessen Felder verwendet:

```

T
| extend person = parsejson('{"name":"Alan", "age":21, "address":{"street":432,"postcode":"JLK32P"}}')
| extend n = person.name, add = person.address.street
```


## Dynamische Objektfunktionen

|||
|---|---|
| *Wert* `in` *Array*| „True“, wenn ein Element von *Array* vorhanden ist, das *Wert* entspricht.<br/>`where City in ('London', 'Paris', 'Rome')`
| *Wert* `!in` *Array*| „True“, wenn kein Element von *Array* vorhanden ist, das *Wert* entspricht.
|[`arraylength(`array`)`](#arraylength)| Null, wenn es sich nicht um ein Array handelt.
|[`extractjson(`path,object`)`](#extractjson)|Verwendet den Pfad zum Navigieren in das Objekt.
|[`parsejson(`source`)`](#parsejson)| Wandelt eine JSON-Zeichenfolge in ein dynamisches Objekt um.
|[`range(`from,to,step`)`](#range)| Ein Array von Werten
|[`mvexpand` listColumn](#mvexpand-operator) | Repliziert eine Zeile für jeden Wert in einer Liste in eine angegebene Zelle.
|[`summarize buildschema(`column`)`](#buildschema) |Leitet das Typschema aus dem Spalteninhalt ab.
|[`summarize makelist(`column`)` ](#makelist)| Reduziert die Zeilengruppen und setzt die Werte der Spalte in ein Array.
|[`summarize makeset(`column`)`](#makeset) | Reduziert die Zeilengruppen und setzt die Werte der Spalte in ein Array ohne Duplizierung.

### Dynamische Objekte in Let-Klauseln


[Let-Klauseln](#let-clause) speichern dynamische Werte als Zeichenfolgen, sodass diese beiden Klauseln gleichwertig sind und beide vor der Verwendung `parsejson` (oder `todynamic`) benötigen:

    let list1 = '{"a" : "somevalue"}';
    let list2 = parsejson('{"a" : "somevalue"}');

    T | project parsejson(list1).a, parsejson(list2).a




### arraylength

Die Anzahl der Elemente in einem dynamischen Array.

**Syntax**

    arraylength(array)

**Argumente**

* *array:* Ein `dynamic`-Wert.

**Rückgabe**

Die Anzahl von Elementen in *array* oder `null`, wenn *array* kein Array ist.

**Beispiele**

```
arraylength(parsejson('[1, 2, 3, "four"]')) == 4
arraylength(parsejson('[8]')) == 1
arraylength(parsejson('[{}]')) == 1
arraylength(parsejson('[]')) == 0
arraylength(parsejson('{}')) == null
arraylength(parsejson('21')) == null
```



### extractjson

    extractjson("$.hosts[1].AvailableMB", EventText, typeof(int))

Rufen Sie ein angegebenes Element aus einem JSON-Text mit einem Pfadausdruck ab. Konvertieren Sie optional die extrahierte Zeichenfolge in einen bestimmten Typ.


**Syntax**

```

    string extractjson(jsonPath, dataSource)​​ 
    resulttype extractjson(jsonPath, dataSource, typeof(resulttype))​​
```


**Rückgabe**

Diese Funktion führt eine JsonPath-Abfrage in dataSource durch, die eine gültige JSON-Zeichenfolge enthält. Optional wird dieser Wert, je nach drittem Argument, in einen anderen Typ konvertiert.



**Beispiel**

Die [Klammer]-Notation und die Punktnotation sind gleichwertig:

    ... | extend AvailableMB = extractjson("$.hosts[1].AvailableMB", EventText, typeof(int)) | ...

    ... | extend AvailableMD = extractjson("$['hosts'][1]['AvailableMB']", EventText, typeof(int)) | ...



**Leistungstipps**

* Wenden Sie vor der Verwendung von `extractjson()` where-Klauseln an.
* Erwägen Sie stattdessen den Abgleich mit einem regulären Ausdruck mit [extract](#extract). Dies kann sehr viel schneller ausgeführt werden und ist effektiv, wenn die JSON aus einer Vorlage erstellt wird.
* Verwenden Sie `parsejson()`, wenn Sie mehrere Werte aus der JSON extrahieren müssen.
* Ziehen Sie es in Betracht, die JSON zum Zeitpunkt der Erfassung zu analysieren, indem Sie den Typ der Spalte als dynamisch deklarieren.

### JSON Path-Ausdrücke

|||
|---|---|
|`$`|Stammobjekt|
|`@`|Aktuelles Objekt|
|`[0]`|Array-Subscript|
|`.` oder `[0]` | Untergeordnet|

*(Derzeit werden keine Platzhalter, Rekursionen, Vereinigungen oder Segmente implementiert.)*




### parsejson

Interpretiert `string` als [JSON-Wert](http://json.org/) und gibt den Wert als `dynamic` zurück. Dies ist der Nutzung von `extractjson()` vorzuziehen, wenn Sie mehrere Elemente eines zusammengesetzten JSON-Objekts extrahieren müssen.

**Syntax**

    parsejson(json)

**Argumente**

* *json*: Ein JSON-Dokument.

**Rückgabe**

Ein Objekt vom Typ `dynamic`, angegeben durch *json*.

**Beispiel**

Für das folgende Beispiel gilt, wenn `context_custom_metrics` ein `string` ist, der folgendermaßen aussieht:

```
{"duration":{"value":118.0,"count":5.0,"min":100.0,"max":150.0,"stdDev":0.0,"sampledValue":118.0,"sum":118.0}}
```

ruft das folgende Fragment zunächst den Wert des `duration`-Slots im Objekt und anschließend zwei Slots daraus ab: `duration.value` und `duration.min` (bzw. `118.0` und `110.0`).

```AIQL
T
| ...
| extend d=parsejson(context_custom_metrics) 
| extend duration_value=d.duration.value, duration_min=d["duration"]["min"]
```



### range

Die `range()`-Funktion (nicht zu verwechseln mit dem `range`-Operator) erzeugt ein dynamisches Array mit einer Reihe gleichmäßig verteilter Werte.

**Syntax**

    range(start, stop, step)

**Argumente**

* *start*: Der Wert des ersten Elements im resultierenden Array. 
* *stop*: Der Wert des letzten Elements im resultierenden Array oder der kleinste Wert, der größer ist als das letzte Element im resultierenden Array und in einem Vielfachen (ganze Zahl) von *step* von *start*.
* *step*: Die Differenz zwischen zwei aufeinanderfolgenden Elementen des Arrays.

**Beispiele**

Das folgende Beispiel gibt `[1, 4, 7]` zurück:

```AIQL
range(1, 8, 3)
```

Das folgende Beispiel gibt ein Array mit allen Tagen im Jahr 2015 zurück:

```AIQL

    range(datetime(2015-01-01), datetime(2015-12-31), 1d)
```

### todynamic

    todynamic('{"a":"a1", "b":["b1", "b2"]}')

Konvertiert eine Zeichenfolge in einen dynamischen Wert.

### treepath

    treepath(dynamic_object)

Listet alle Path-Ausdrücke auf, die Verzweigungen in einem dynamischen Objekt identifizieren.

**Rückgabe**

Ein Array von Path-Ausdrücken.

**Beispiele**

    treepath(parsejson('{"a":"b", "c":123}')) 
    =>       ["['a']","['c']"]
    treepath(parsejson('{"prop1":[1,2,3,4], "prop2":"value2"}'))
    =>       ["['prop1']","['prop1'][0]","['prop2']"]
    treepath(parsejson('{"listProperty":[100,200,300,"abcde",{"x":"y"}]}'))
    =>       ["['listProperty']","['listProperty'][0]","['listProperty'][0]['x']"]

Beachten Sie, dass „[0]“ auf das Vorhandensein eines Arrays hinweist, aber nicht den von einem bestimmten Pfad verwendeten Index angibt.

## Namen

Namen können bis zu 1024 Zeichen lang sein. Die Groß-/Kleinschreibung wird beachtet, und sie können Buchstaben, Ziffern und Unterstriche (`_`) enthalten.

Geben Sie einen Namen mit ['... '] oder [" ... "] an, um andere Zeichen einzubeziehen oder ein Schlüsselwort als Name zu verwenden. Beispiel:

```AIQL

    requests | 
    summarize  ["distinct urls"] = dcount(name) // non-alphanumerics
    by  ['where'] = client_City, // using a keyword as a name
        ['outcome!'] = success // non-alphanumerics
```


|||
|---|---|
|['path\\file\\n'x''] | Verwenden Sie \\ für Zeichen als Escapezeichen.|
|["d-e.=/f#\\n"] | |
|[@'path\\file'] | Keine Escapezeichen – \\ ist literal.|
|[@"\\now & then"] | |
|[where] | Verwenden eines Programmiersprachenschlüsselworts als Name|

[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!---HONumber=AcomDC_0427_2016-->