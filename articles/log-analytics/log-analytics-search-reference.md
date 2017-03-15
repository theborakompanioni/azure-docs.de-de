---
title: Referenz zur Azure Log Analytics-Suche | Microsoft-Dokumentation
description: "Die Referenz zur Log Analytics-Suche enthält eine Beschreibung der Suchsprache und die allgemeinen Optionen zur Abfragesyntax, die Sie beim Suchen von Daten und Filtern von Ausdrücken verwenden können, um Ihre Suche einzugrenzen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 402615a2-bed0-4831-ba69-53be49059718
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: a0c8af30fbed064001c3fd393bf0440aa1cb2835
ms.openlocfilehash: a294ef357ea3c656e4e6392e89ad1d90b829e2cb
ms.lasthandoff: 02/28/2017


---
# <a name="log-analytics-search-reference"></a>Referenz zur Log Analytics-Suche
Im folgenden Referenzabschnitt zur Suchsprache werden die allgemeinen Optionen zur Abfragesyntax beschrieben, die Sie beim Suchen von Daten und Filtern von Ausdrücken verwenden können, um Ihre Suche einzugrenzen. Außerdem werden die Befehle beschrieben, mit denen Sie Aktionen für die abgerufenen Daten ausführen können.

Sie erhalten Informationen über die von der Suche zurückgegebenen Felder und die Facetten, mit denen Sie in ähnliche Datenkategorien im [Abschnitt „Search-Feld- und Facetreferenz](#search-field-and-facet-reference)eintauchen können.

## <a name="general-query-syntax"></a>Allgemeine Abfragesyntax
Syntax:

```
filterExpression | command1 | command2 …
```

Der Filterausdruck (`filterExpression`) bestimmt die „Where“-Bedingung für die Abfrage. Die Befehle gelten für die von der Abfrage zurückgegebenen Ergebnisse. Mehrere Befehle müssen durch einen senkrechten Strich (|) getrennt werden.

### <a name="general-syntax-examples"></a>Beispiele für die allgemeine Syntax
Beispiele:

```
system
```

Diese Abfrage gibt Ergebnisse mit dem Wort "System" in jedem Feld zurück, das für die Volltextsuche oder bestimmte Suchbegriffe indiziert wurde.

> [!NOTE]
> Nicht alle Felder sind auf diese Weise indiziert, aber die am häufigsten verwendeten Textfelder (z. B. Beschreibungen und Namen) sind es in der Regel schon.
>
>

```
system error
```

Diese Abfrage gibt Ergebnisse zurück, die die Wörter *system* und *error* enthalten.

```
system error | sort ManagementGroupName, TimeGenerated desc | top 10
```

Diese Abfrage gibt Ergebnisse zurück, die die Wörter *system* und *error* enthalten. Dann sortiert die Abfrage die Ergebnisse nach dem Feld *ManagementGroupName* (in aufsteigender Reihenfolge), und anschließend nach *TimeGenerated* (in absteigender Reihenfolge). Nur die ersten zehn Ergebnisse werden genommen.

> [!IMPORTANT]
> Bei allen Feldnamen und Werten für die Zeichenfolgen- und Textfelder muss die Groß-und Kleinschreibung beachtet werden.
>
>

## <a name="filter-expression"></a>Filtern von Ausdrücken
In den folgenden Unterabschnitten werden Filterausdrücke erläutert.

### <a name="string-literals"></a>Zeichenfolgenliterale
Ein Zeichenfolgenliteral ist eine Zeichenfolge, die vom Parser nicht als ein Schlüsselwort oder ein vordefinierter Datentyp (z. B. eine Zahl oder ein Datum) erkannt wird.

Beispiele:

```
These all are string literals
```

Diese Abfrage sucht nach Ergebnissen, die Vorkommen mit allen fünf Wörtern enthalten. Schließen Sie das Zeichenfolgenliteral in Anführungszeichen ein, um eine komplexe Zeichenfolgensuche durchzuführen, beispielsweise:

```
" Windows Server"
```

Hierbei werden nur Ergebnisse mit genauen Übereinstimmungen für „Windows Server“ zurückgegeben.

### <a name="numbers"></a>Zahlen
Der Parser unterstützt die Syntax mit ganzen Dezimalzahlen und Gleitkommazahlen für numerische Felder.

Beispiele:

```
Type:Perf 0.5
```

```
HTTP 500
```

### <a name="datetime"></a>Datum/Uhrzeit
Jedes Datenelement im System besitzt eine *TimeGenerated*-Eigenschaft, die das ursprüngliche Datum und die Uhrzeit des Datensatzes darstellt. Einige Datentypen können darüber hinaus weitere Datums-/Uhrzeitfelder haben (z.B. *LastModified*).

Das Zeitachsen-Diagramm bzw. der Uhrzeit-Selektor in Log Analytics zeigt eine Verteilung von Ergebnissen im Zeitverlauf an (gemäß der aktuell ausgeführten Abfrage), basierend auf dem Feld *TimeGenerated*. Datums-/Uhrzeit-Felder verfügen über ein bestimmtes Zeichenfolgenformat, das in Abfragen verwendet werden kann, um diese auf einen bestimmten Zeitraum zu beschränken. Sie können mit der Syntax auch auf relative Zeitintervalle verweisen (z. B. "zwischen vor 3 Tagen und vor 2 Stunden") .

Syntax:

```
yyyy-mm-ddThh:mm:ss.dddZ
```

```
yyyy-mm-ddThh:mm:ss.ddd
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm:ss
```

```
yyyy-mm-ddThh:mm
```

```
yyyy-mm-dd
```


Beispiel:

```
TimeGenerated:2013-10-01T12:20
```

Der vorherige Befehl gibt nur Datensätze mit einem *TimeGenerated*-Wert von genau 12:20 Uhr, 1. Oktober 2013 zurück. Es ist unwahrscheinlich, dass Sie ein Ergebnis erhalten, aber Sie verstehen nun das Konzept.

Der Parser unterstützt auch den mnemonischen Datums-/Uhrzeitwert "NOW".

In diesem Fall ist es unwahrscheinlich, dass dies ein Ergebnis gibt, da Daten nicht so schnell durch das System gehen.

Diese Beispiele sind die Bausteine für relative und absolute Datumsangaben. In den nächsten drei Unterabschnitten erläutern wir die Verwendung in erweiterten Filter mit Beispielen, bei denen ein relativer Datumsbereich verwendet wird.

### <a name="datetime-math"></a>Datums-/Uhrzeit-Mathematik
Verwenden Sie mathematische Datums-/Uhrzeit-Operatoren, um Datums-/Uhrzeitwerte zu versetzen oder zu runden, indem einfache Datums-/Uhrzeit-Berechnungen verwendet werden.

Syntax:

```
datetime/unit
```

```
datetime[+|-]count unit
```

| Operator | Beschreibung |
| --- | --- |
| / |Datum/Uhrzeit, in der angegebenen Einheit gerundet. Beispiel: NOW/DAY rundet das aktuelle Datum/die aktuelle Uhrzeit auf Mitternacht des aktuellen Tages. |
| + oder - |Versetzt Datum und Uhrzeit um die angegebene Anzahl von Einheiten. Beispiele: NOW+1HOUR Stunde versetzt den aktuellen Zeitpunkt (Datum/Uhrzeit) um eine Stunde nach vorn. 2013-10-01T12:00-10DAYS versetzt den Datumswert um 10 Tage zurück. |

Sie können Datum/Zeit mit mathematischen Operatoren verketten, z. B.:

```
NOW+1HOUR-10MONTHS/MINUTE
```

Die folgende Tabelle enthält die unterstützten Datums-/Uhrzeit-Einheiten.

| Einheit für Datum/Uhrzeit | Beschreibung |
| --- | --- |
| YEAR, YEARS |Rundet auf das aktuelle Jahr oder versetzt um die angegebene Anzahl von Jahren. |
| MONTH, MONTHS |Rundet auf den aktuellen Monat oder versetzt um die angegebene Anzahl von Monaten. |
| DAY, DAYS, DATE |Rundet auf den aktuellen Tag des Monats oder versetzt um die angegebene Anzahl von Tagen. |
| HOUR, HOURS |Rundet auf die aktuelle Stunde oder versetzt um die angegebene Anzahl von Stunden. |
| MINUTE, MINUTES |Rundet auf die aktuelle Minute oder versetzt um die angegebene Anzahl von Minuten. |
| SECOND, SECONDS |Rundet auf die aktuelle Sekunde oder versetzt um die angegebene Anzahl von Sekunden. |
| MILLISECOND, MILLISECONDS, MILLI, MILLIS |Rundet auf die aktuelle Millisekunde oder versetzt um die angegebene Anzahl von Millisekunden. |

### <a name="field-facets"></a>Feldfacetten
Mithilfe von Feldfacetten können Sie die Suchbedingung für bestimmte Felder und deren genaue Werte festlegen, im Gegensatz zum Schreiben von Abfragen mit "freiem Text" mit verschiedenen Begriffe über den gesamten Index hinweg. Wir haben diese Syntax bereits in mehreren Beispielen in den vorherigen Abschnitten verwendet. Es folgen komplexere Beispiele.

**Syntax**

```
field:value
```

```
field=value
```

**Beschreibung**

Durchsucht das Feld nach dem spezifischen Wert. Der Wert kann ein Zeichenfolgenliteral, eine Nummer oder Datum/Uhrzeit sein.

Beispiel:

```
TimeGenerated:NOW
```

```
ObjectDisplayName:"server01.contoso.com"
```

```
SampleValue:0.3
```

**Syntax**

*Feld>Wert*

*Feld<Wert*

*Feld>=Wert*

*Feld<=Wert*

*Feld!=Wert*

**Beschreibung**

Stellt Vergleiche bereit.

Beispiel:

```
TimeGenerated>NOW+2HOURS
```

**Syntax**

```
field:[from..to]
```

**Beschreibung**

Enthält eine Bereichsfacettierung.

Beispiel:

```
TimeGenerated:[NOW..NOW+1DAY]
```

```
SampleValue:[0..2]
```

### <a name="regular-expressions"></a>Reguläre Ausdrücke
Sie können mit dem Schlüsselwort „Regex“ eine Suchbedingung für ein Feld mit einem regulären Ausdruck angeben.  Unter [Verwenden regulärer Ausdrücke zum Filtern der Protokollsuchen in Log Analytics](log-analytics-log-searches-regex.md) erhalten Sie eine vollständige Beschreibung der Syntax, die Sie in regulären Ausdrücken verwenden können.

**Syntax**

```
field:Regex("Regular Expression")
```

```
field=Regex("Regular Expression")
```

**Beispiel**

```
Computer=Regex("C.*")
```

```
Computer=Regex("^C.*")
```

### <a name="logical-operators"></a>Logische Operatoren
Abfragesprachen unterstützen die logischen Operatoren (*AND*, *OR* und *NOT*) und ihre Aliase im C-Format (*&&*, *||* und *!*). Sie können Klammern verwenden, um diese Operatoren zu gruppieren.

Beispiele:

```
system OR error

```

```
Type:Alert AND NOT(Severity:1 OR ObjectId:"8066bbc0-9ec8-ca83-1edc-6f30d4779bcb8066bbc0-9ec8-ca83-1edc-6f30d4779bcb")
```

Sie können den logischen Operator für Filterargumente der obersten Ebene auslassen. In diesem Fall wird vom AND-Operator ausgegangen.

| Filtern von Ausdrücken | Entspricht |
| --- | --- |
| system error |system AND error |
| system "Windows Server" OR Severity:1 |system AND ("Windows Server" OR Severity:1) |

### <a name="wildcarding"></a>Verwenden von Platzhaltern
Die Abfragesprache unterstützt die Verwendung des Zeichens ( \* ), um in einer Abfrage ein oder mehrere Zeichen für einen Wert darzustellen.

Beispiele:

 Suchen nach allen Computern mit „SQL“ im Namen, z.B. „Redmond-SQL“.

```
Type=Event Computer=*SQL*
```

> [!NOTE]
> Platzhalter können derzeit nicht in Anführungszeichen verwendet werden. In „Message=`"*This text*"`“ wird (\*) als literales Zeichen (\*) verwendet.
>
> ## <a name="commands"></a>Befehle
>

Die Befehle gelten für die von der Abfrage zurückgegebenen Ergebnisse. Verwenden Sie den senkrechten Strich (|), um einen Befehl auf die abgerufenen Ergebnisse anzuwenden. Mehrere Befehle müssen durch einen senkrechten Strich (|) getrennt werden.

> [!NOTE]
> Befehlsnamen können in Groß- oder Kleinbuchstaben geschrieben werden, im Gegensatz zu den Feldnamen und den Daten.
>
>

### <a name="sort"></a>Sortieren
Syntax:

    sort field1 asc|desc, field2 asc|desc, …

Sortiert die Ergebnisse nach bestimmten Feldern. Das Präfix "Asc/Desc" ist optional. Wenn es ausgelassen wird, wird von der *Asc*-Sortierreihenfolge ausgegangen. Wenn eine Abfrage den Befehl *Sortieren* nicht explizit verwendet, ist „**TimeGenerated** abst. sortieren“ das Standardverhalten. Dabei werden die neuesten Ergebnisse immer zuerst zurückgegeben.

### <a name="toplimit"></a>"Top"/"Limit"
Syntax:

    top number


    limit number
Schränkt die Antwort auf die ersten N-Ergebnisse ein.

Beispiel:

    Type:Alert errors detected | top 10

Gibt die höchsten 10 übereinstimmenden Ergebnisse zurück.

### <a name="skip"></a>Überspringen
Syntax:

    skip number

Überspringt die Anzahl der aufgelisteten Ergebnisse.

Beispiel:

    Type:Alert errors detected | top 10 | skip 200

Gibt die höchsten 10 übereinstimmenden Ergebnisse zurück, beginnend bei 200 Ergebnissen.

### <a name="select"></a>Wählen Sie
Syntax:

    select field1, field2, ...

Beschränkt die Ergebnisse auf die Felder, die Sie auswählen.

Beispiel:

    Type:Alert errors detected | select Name, Severity

Schränkt die zurückgegebenen Ergebnisfelder auf *Name* und *Severity*eintauchen können.

### <a name="measure"></a>"Measure"
Der Befehl *Measure* wird verwendet, um statistische Funktionen auf unformatierte Suchergebnisse anzuwenden. Dies ist sehr nützlich, um einen ersten *gruppierten* Überblick über die Daten zu erhalten. Wenn Sie den Befehl *measure* verwenden, zeigt die Log Analytics-Suche eine Tabelle mit aggregierten Ergebnissen an.

Syntax:

    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]] by groupField1 [, groupField2 [, groupField3]]  [interval interval]


    measure aggregateFunction1([aggregatedField]) [as fieldAlias1] [, aggregateFunction2([aggregatedField2]) [as fieldAlias2] [, ...]]  interval interval



Aggregiert die Ergebnisse nach *groupField* und berechnet die aggregierten „Measure“-Werte mit *aggregatedField*.

| Measure-Statistikfunktion | Beschreibung |
| --- | --- |
| *aggregateFunction* |Der Name der Aggregatfunktion (ohne Berücksichtigung der Groß-/Kleinschreibung). Die folgenden Aggregatfunktionen werden unterstützt: COUNT, MAX, MIN, SUM, AVG, STDDEV, COUNTDISTINCT, PERCENTILE## oder PCT## (## ist eine beliebige Zahl zwischen 1 und 99). |
| *aggregatedField* |Das Feld, das aggregiert wird. Dieses Feld ist für die Aggregatfunktion COUNT optional, aber es muss sich um ein vorhandenes numerisches Feld für SUM, MAX, MIN, AVG, STDDEV oder PERCENTILE## bzw. PCT## handeln (## ist eine beliebige Zahl zwischen 1 und 99). Bei aggregatedField kann es sich auch um eine der von „Extend“ unterstützten Funktionen handeln. |
| *fieldAlias* |Der (optionale) Alias für den berechneten aggregierten Wert. Wenn keine Angabe gemacht wird, lautet der Feldname „AggregatedValue“. |
| *groupField* |Der Name des Felds, nach dem das Resultset gruppiert ist. |
| *Intervall* |Das Zeitintervall im Format**nnnNAME** , wobei nnn die positive ganze Zahl ist. **NAME** ist der Intervallname. Beispiele für unterstützte Intervallnamen (mit Berücksichtigung der Groß-/Kleinschreibung): MILLISECOND[S], SECOND[S], MINUTE[S], HOUR[S], DAY[S], MONTH[S], YEAR[S]. |

Die Intervalloption kann nur in Datums-/Uhrzeit-Gruppenfeldern verwendet werden (z.B. *TimeGenerated* und *TimeCreated*). Aktuell, dies wird nicht vom Dienst erzwungen, aber ein Feld ohne Datum/Uhrzeit, das an das Back-End übergeben wird, führt zu einem Laufzeitfehler. Wenn die Schemaüberprüfung implementiert wird, lehnt die Dienst-API Abfragen ab, die ohne Datum/Uhrzeit-Felder für die Intervall-Aggregation verwenden werden. Die aktuelle *Measure*-Implementierung unterstützt Intervallgruppierungen für jede Aggregatfunktion.

Wenn die BY-Klausel weggelassen, aber ein Intervall angegeben wird (als zweite Syntax), wird das *TimeGenerated* -Feld standardmäßig angenommen.

Beispiele:

**Beispiel 1**

    Type:Alert | measure count() as Count by ObjectId

*Erklärung*

Gruppiert die Warnungen nach *ObjectID* und berechnet die Anzahl der Warnungen für jede Gruppe. Der aggregierte Wert wird als das *Count* -Feld (Alias) zurückgegeben.

**Beispiel 2**

    Type:Alert | measure count() interval 1HOUR

*Erklärung*

Gruppiert die Warnungen in stündlichen Intervallen mithilfe des *TimeGenerated* -Felds, und gibt die Anzahl der Warnungen in jedem Intervall zurück.

**Beispiel 3**

    Type:Alert | measure count() as AlertsPerHour interval 1HOUR

*Erklärung*

Wie im vorherigen Beispiel, jedoch mit einem aggregierten Feldalias (*AlertsPerHour*).

**Beispiel 4**

    * | measure count() by TimeCreated interval 5DAYS

*Erklärung*

Gruppiert die Ergebnisse in Intervallen von fünf Tagen mithilfe des *TimeCreated* -Felds, und gibt die Anzahl der Warnungen in jedem Intervall zurück.

**Beispiel 5**

    Type:Alert | measure max(Severity) by WorkflowName

*Erklärung*

Gruppiert die Warnungen anhand des Workloadnamens und gibt den Wert des maximalen Schweregrads der Warnung für jeden Workflow zurück.

**Beispiel 6**

    Type:Alert | measure min(Severity) by WorkflowName

*Erklärung*

Wie im vorherigen Beispiel, jedoch mit der aggregierten *Min* -Funktion.

**Beispiel 7**

    Type:Perf | measure avg(CounterValue) by Computer

*Erklärung*

Gruppiert die Leistung nach Computer und berechnet den Mittelwert (avg).

**Beispiel 8**

    Type:Perf | measure sum(CounterValue) by Computer

*Erklärung*

Wie im vorherigen Beispiel, verwendet jedoch *Sum*.

**Beispiel 9**

    Type:Perf | measure stddev(CounterValue) by Computer

*Erklärung*

Wie im vorherigen Beispiel, verwendet jedoch *STDDEV*.

**Beispiel 10**

    Type:Perf | measure percentile70(CounterValue) by Computer

*Erklärung*

Wie im vorherigen Beispiel, aber es wird *PERCENTILE70*verwendet.

**Beispiel 11**

    Type:Perf | measure pct70(CounterValue) by Computer

*Erklärung*

Wie im vorherigen Beispiel, aber es wird *PCT70*verwendet. Beachten Sie, dass *PCT##* nur ein Alias für die Funktion *PERCENTILE##* ist.

**Beispiel 12**

    Type:Perf | measure avg(CounterValue) by Computer, CounterName

*Erklärung*

Gruppiert die Leistung zunächst nach „Computer“ und dann nach „CounterName“ und berechnet den Durchschnitt (avg).

**Beispiel 13**

    Type:Alert | measure count() as Count by WorkflowName | sort Count desc | top 5

*Erklärung*

Ruft die oberen fünf Workflows mit der maximalen Anzahl von Warnungen ab.

**Beispiel 14**

    * | measure countdistinct(Computer) by Type

*Erklärung*

Zählt die Anzahl von eindeutigen Computern, die für jeden Typ gemeldet werden.

**Beispiel 15**

    * | measure countdistinct(Computer) Interval 1HOUR

*Erklärung*

Zählt die Anzahl von eindeutigen Computern, die für jede Stunde Meldungen liefern.

**Beispiel 16**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total” | measure avg(CounterValue) by Computer Interval 1HOUR
```

*Erklärung*

Gruppiert „% Processor Time“ nach Computer und gibt den Mittelwert für jede Stunde zurück.

**Beispiel 17**

    Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES

*Erklärung*

Gruppiert W3CIISLog nach Methode und gibt den maximalen Wert jeweils für fünf Minuten zurück.

**Beispiel 18**

```
Type:Perf CounterName=”% Processor Time” InstanceName=”_Total”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer Interval 1HOUR
```

*Erklärung*

Gruppiert „% Processor Time“ nach Computer und gibt für jede Stunde Minimalwert, Mittelwert, 75. Perzentil und Maximalwert zurück.

**Beispiel 19**

```
Type:Perf CounterName=”% Processor Time”  | measure min(CounterValue) as MIN, avg(CounterValue) as AVG, percentile75(CounterValue) as PCT75, max(CounterValue) as MAX by Computer, InstanceName Interval 1HOUR
```

*Erklärung*

Gruppiert „% Processor Time“ zunächst nach „Computer“ und dann nach „InstanceName“ und gibt für jede Stunde Minimalwert, Mittelwert, 75. Perzentil und Maximalwert zurück

**Beispiel 20**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | measure max(product(CounterValue,60)) as MaxDWPerMin by InstanceName Interval 1HOUR
```

*Erklärung*

Berechnet die maximale Anzahl von Schreibvorgängen auf dem Datenträger pro Minute für jeden Datenträger auf dem Computer.

### <a name="where"></a>Where
Syntax:

```
**where** AggregatedValue>20
```

Kann nur nach einem *Measure*-Befehl verwendet werden, um die aggregierten Ergebnisse weiter zu filtern, die die *Measure*-Aggregationsfunktion erzeugt hat.

Beispiele:

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) as MAXCPU by Computer

    Type:Perf CounterName:"% Total Run Time" | Measure max(CounterValue) by Computer | where (AggregatedValue>50 and AggregatedValue<90)

### <a name="in"></a>IN
Das Schlüsselwort **IN** ermöglicht Ihnen die Auswahl aus einer Liste von Werten.  Je nach verwendeter Syntax kann dies eine einfache Liste mit von Ihnen angegebenen Werten oder eine Liste mit Werten aus einer Aggregation sein.

Syntax 1:

```
field IN {value1,value2,value3,...}
```

Beschreibung: Mithilfe dieser Syntax können Sie alle Werte in einer einfachen Liste einbeziehen.



Beispiele:

```
EventID IN {1201,1204,1210}
```

```
Computer IN {"srv01.contoso.com","srv02.contoso.com"}
```

Syntax 2:

```
(Outer Query) (Field to use with inner query results) IN {Inner query | measure count() by (Field to send to outer query)} (rest  of outer query)  
```

Beschreibung: Mit dieser Syntax können Sie eine Aggregation erstellen und die Liste mit den Werten aus dieser Aggregation in eine andere äußere (primäre) Suche einfügen, bei der nach Ereignissen mit diesem Wert gesucht wird. Hierzu setzen Sie die innere Suche in Klammern und fügen deren Ergebnisse als mögliche Werte für ein Feld in der äußeren Suche ein, indem Sie den Operator IN verwenden.

Beispiel für innere Abfrage: *Computer mit derzeit fehlenden Sicherheitsupdates* mit der folgenden Aggregationsabfrage:

```
Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer
```    

Die letzte Abfrage der Art *Alle Windows-Ereignisse für Computer mit derzeit fehlenden Sicherheitsupdates* sieht dann wie folgt aus:

```
Type=Event Computer IN {Type:Update Classification="Security Updates"  UpdateState=needed TimeGenerated>NOW-25HOURS | measure count() by Computer}
```

### <a name="dedup"></a>Dedup
**Syntax**

    Dedup FieldName

**Beschreibung** Gibt das erste Dokument zurück, das für jeden eindeutigen Wert des jeweiligen Felds gefunden wird.

**Beispiel**

    Type=Event | Dedup EventID | sort TimeGenerated DESC

Im obigen Beispiel wird ein Ereignis (das letzte, da wir DESC für TimeGenerated verwenden) pro EventID zurückgegeben.

### <a name="extend"></a>Extend
**Beschreibung** Ermöglicht Ihnen die Erstellung von Laufzeitfeldern in Abfragen. Sie können nach „Extend“ auch den Befehl „measure“ verwenden, wenn Sie eine Aggregation ausführen möchten.

**Beispiel 1**

    Type=SQLAssessmentRecommendation | Extend product(RecommendationScore, RecommendationWeight) AS RecommendationWeightedScore
Anzeigen einer gewichteten Empfehlungsbewertung für SQL Assessment-Empfehlungen

**Beispiel 2**

    Type=Perf CounterName="Private Bytes" | EXTEND div(CounterValue,1024) AS KBs | Select CounterValue,Computer,KBs
Anzeigen des Zählerwerts in KB anstelle von Byte

**Beispiel 3**

    Type=WireData | EXTEND scale(TotalBytes,0,100) AS ScaledTotalBytes | Select ScaledTotalBytes,TotalBytes | SORT TotalBytes DESC
Skalieren des Werts von WireData TotalBytes, sodass alle Ergebnisse zwischen 0 und 100 liegen

**Beispiel 4**

```
Type=Perf CounterName="% Processor Time" | EXTEND if(map(CounterValue,0,50,0,1),"HIGH","LOW") as UTILIZATION
```
Kennzeichnen von Leistungsindikatorwerten von unter 50% als LOW, andernfalls als HIGH

**Beispiel 5**

```
Type= Perf CounterName="Disk Writes/sec" Computer="BaconDC01.BaconLand.com" | Extend product(CounterValue,60) as DWPerMin| measure max(DWPerMin) by InstanceName Interval 1HOUR
```
Berechnet die maximale Anzahl von Schreibvorgängen auf dem Datenträger pro Minute für jeden Datenträger auf dem Computer.

**Unterstützte Funktionen**

| Funktion | Beschreibung | Syntaxbeispiele |
| --- | --- | --- |
| abs |Gibt den absoluten Wert des angegebenen Werts oder der Funktion zurück. |`abs(x)` <br> `abs(-5)` |
| acos |Gibt den Arcuscosinus eines Werts oder einer Funktion zurück |`acos(x)` |
| und |Gibt den Wert „TRUE“ zurück, wenn sich für alle Operanden das Ergebnis „TRUE“ ergibt. |`and(not(exists(popularity)),exists(price))` |
| asin |Gibt den Arcussinus eines Werts oder einer Funktion zurück |`asin(x)` |
| atan |Gibt den Arcustangens eines Werts oder einer Funktion zurück |`atan(x)` |
| atan2 |Gibt den Winkel zurück, der sich aus der Konvertierung der kartesischen Koordinaten x und y zu Polarkoordinaten ergibt |`atan2(x,y)` |
| cbrt |Kubikwurzel |`cbrt(x)` |
| ceil |Rundet auf eine ganze Zahl auf |`ceil(x)`  <br> `ceil(5.6)` – Gibt 6 zurück |
| cos |Gibt den Kosinus eines Winkels zurück |`cos(x)` |
| cosh |Gibt den hyperbolischen Kosinus eines Winkels zurück |`cosh(x)` |
| def |def ist die Abkürzung für „default“ (Standard). Gibt den Wert des Felds „field“ zurück. Falls das Feld nicht vorhanden ist, wird der angegebene Standardwert zurückgegeben, und es ergibt sich der erste Wert, für den Folgendes gilt: `exists()==true`. |`def(rating,5)` – Diese def()-Funktion gibt die Bewertung zurück. Falls keine Bewertung im Dokument angegeben ist, gibt sie „5“ zurück. <br> `def(myfield, 1.0)` – entspricht `if(exists(myfield),myfield,1.0)` |
| deg |Konvertiert Bogenmaße zu Graden |`deg(x)` |
| div |`div(x,y)` dividiert x durch y. |`div(1,y)` <br> `div(sum(x,100),max(y,1))` |
| dist |Gibt den Abstand zwischen zwei Vektoren (Punkten) in einem n-dimensionalen Raum zurück. Verwendet die Exponenten, plus zwei oder mehr, von ValueSource-Instanzen und berechnet die Abstände zwischen den beiden Vektoren. Jedes ValueSource-Element muss eine Zahl sein. Es muss eine gerade Anzahl von ValueSource-Instanzen übergeben werden. Für die Methode wird angenommen, dass die erste Hälfte den ersten Vektor und die zweite Hälfte den zweiten Vektor darstellt. |`dist(2, x, y, 0, 0)` – Berechnet den euklidischen Abstand zwischen (0,0) und (x,y) für jedes Dokument <br> `dist(1, x, y, 0, 0)` – Berechnet den Abstand „Manhattan“ (Taxi) zwischen (0,0) und (x,y) für jedes Dokument <br> `dist(2,,x,y,z,0,0,0)` – Der euklidischen Abstand zwischen (0,0,0) und (x,y,z) für jedes Dokument<br>`dist(1,x,y,z,e,f,g)` – Die Entfernung „Manhattan“ zwischen (x,y,z) und (e,f,g), wobei jeder Buchstabe für einen Feldnamen steht |
| exists |Gibt TRUE zurück, wenn ein Member des Felds vorhanden ist. |`exists(author)` – Gibt TRUE für jedes Dokument zurück, das im Feld „author“ einen Wert enthält<br>`exists(query(price:5.00))` – Gibt TRUE zurück, wenn „price“ mit „5.00“ übereinstimmt. |
| exp |Gibt die Potenz X der Eulerschen Zahl zurück |`exp(x)` |
| floor |Rundet auf eine ganze Zahl ab |`floor(x)`  <br> `floor(5.6)` – gibt 5 zurück |
| hypo |Gibt sqrt(sum(pow(x,2),pow(y,2))) ohne zwischenzeitlichen Über- bzw. Unterlauf zurück |`hypo(x,y)`  <br> ` |
| if |Ermöglicht bedingte Funktionsabfragen. In `if(test,value1,value2)` – „test“ ist oder bezieht sich auf einen logischen Wert oder Ausdruck, der einen logischen Wert (TRUE oder FALSE) zurückgibt.  `value1` ist der Wert, der von der Funktion zurückgegeben wird, wenn „test“ TRUE ergibt. `value2` ist der Wert, der von der Funktion zurückgegeben wird, wenn „test“ FALSE ergibt. Ein Ausdruck kann jede Funktion sein, mit der boolesche Werte ausgegeben werden, oder es kann sich um Funktionen handeln, die numerische Werte zurückgeben. In diesem Fall wird der Wert 0 als „false“ oder „strings“ interpretiert, sodass eine leere Zeichenfolge als „false“ interpretiert wird. |`if(termfreq(cat,'electronics'),popularity,42)` : Mit dieser Funktion wird jedes Dokument darauf überprüft, ob es im Feld „cat“ den Begriff „electronics“ enthält. Wenn ja, wird der Wert des Felds „popularity“ zurückgegeben, und andernfalls wird der Wert 42 zurückgegeben. |
| linear |Implementiert `m*x+c` , wobei m und c Konstanten sind und x eine beliebige Funktion ist. Dies ist äquivalent zu `sum(product(m,x),c)`, aber etwas effizienter, da die Implementierung als einzelne Funktion durchgeführt wird. |`linear(x,m,c) linear(x,2,4)` gibt `2*x+4` zurück |
| ln |Gibt den natürlichen Logarithmus für die angegebene Funktion zurück |`ln(x)` |
| log |Gibt den Logarithmus zur Basis 10 für die angegebene Funktion zurück. |`log(x)   log(sum(x,100))` |
| map |Ordnet alle Werte einer Eingabefunktion x, die innerhalb des Minimal- und Maximalwerts liegen (einschließlich), dem angegebenen Ziel zu. Bei den Argumenten „min“ und „max“ muss es sich um Konstanten handeln. Das Ziel und der Standardwert der Argumente können Konstanten oder Funktionen sein. Wenn der Wert von x nicht zwischen „min“ und „max“ liegt, wird entweder der Wert von x zurückgegeben, oder es wird ein Standardwert zurückgegeben, wenn er als fünftes Argument angegeben ist. |`map(x,min,max,target) map(x,0,0,1)` : Ändert alle Werte von 0 bis 1. Dies kann beim Umgang mit standardmäßigen 0-Werten nützlich sein.<br> `map(x,min,max,target,default)    map(x,0,100,1,-1)` – Ändert alle Werte zwischen 0 und 100 auf 1 und alle anderen Werte auf -1.<br>  `map(x,0,100,sum(x,599),docfreq(text,solr))` : Ändert alle Werte zwischen 0 und 100 in x+599 und alle anderen Werte in die Häufigkeit des Begriffs „solr“ im Feldtext. |
| max |Gibt den numerischen Maximalwert von mehreren geschachtelten Funktionen oder Konstanten an, die als Argumente angegeben werden: `max(x,y,...)`. Die Funktion „max“ kann auch für die „Bodenbildung“ einer anderen Funktion oder eines Felds für eine angegebene Konstante nützlich sein.  Verwenden Sie die Syntax `field(myfield,max)` , um den Maximalwert eines einzelnen mehrwertigen Felds auszuwählen. |`max(myfield,myotherfield,0)` |
| Min |Gibt den numerischen Minimalwert für mehrere geschachtelte Funktionen von Konstanten zurück, die als Argumente angegeben werden: `min(x,y,...)`. Die Funktion „min“ kann auch hilfreich sein, um über eine Konstante für eine Funktion eine „Obergrenze“ anzugeben. Verwenden Sie die Syntax `field(myfield,min)` , um den Minimalwert eines einzelnen mehrwertigen Felds auszuwählen. |`min(myfield,myotherfield,0)` |
| mod |Ermittelt den Restwert einer Ganzzahldivision der Funktionswerte von X und Y. |`mod(1,x)` <br> `mod(sum(x,100), max(y,1))` |
| ms |Gibt die Millisekunden für die Differenz zwischen den Argumenten zurück. Datumsangaben gelten relativ zum Unix- oder POSIX-Zeitbereich, Mitternacht, 1. Januar 1970 UTC. Argumente können der Name eines indizierten TrieDateField sein oder Datumsberechnungen basierend auf einem Konstantendatum oder NOW. `ms()` stimmt mit `ms(NOW)` überein, die Anzahl von Millisekunden seit diesem Zeitpunkt. `ms(a)` gibt die Anzahl von Millisekunden seit dem Zeitpunkt zurück, der vom Argument dargestellt wird. `ms(a,b)` gibt die Anzahl von Millisekunden zurück, die vor a für b auftauchen, was `a - b` ergibt. |`ms(NOW/DAY)`<br>`ms(2000-01-01T00:00:00Z)`<br>`ms(mydatefield)`<br>`ms(NOW,mydatefield)`<br>`ms(mydatefield,2000-01-01T00:00:00Z)`<br>`ms(datefield1,datefield2)` |
| not |Der logisch negierte Wert der Funktion „wrapped“. |`not(exists(author))` – nur TRUE, wenn `exists(author)` FALSE ist. |
| oder |Eine logische Disjunktion. |`or(value1,value2)` : TRUE, wenn entweder value1 oder value2 „true“ ist. |
| pow |Potenziert die angegebene Basis mit dem angegebenen Potenzwert. `pow(x,y)` wird x mit y potenziert. |`pow(x,y)`<br>`pow(x,log(y))`<br>`pow(x,0.5)` : Entspricht sqrt. |
| product |Gibt das Produkt mehrerer Werte oder Funktionen zurück, die in einer kommagetrennten Liste angegeben sind. `mul(...)` kann auch als Alias für diese Funktion verwendet werden. |`product(x,y,...)`<br>`product(x,2)`<br>`product(x,y)`<br>`mul(x,y)` |
| recip |Führt eine reziproke Funktion mit `recip(x,m,a,b)` durch, bei der `a/(m*x+b)` implementiert wird, wobei m,a,b Konstanten sind, und x eine beliebige komplexe Funktion darstellt. Wenn a und b gleich sind und x>=0 gilt, hat diese Funktion einen Maximalwert von 1, der kleiner wird, wenn sich x erhöht. Das gemeinsame Erhöhen des Werts von a und b führt zu einer Verschiebung der gesamten Funktion in einen flacheren Teil der Kurve. Diese Eigenschaften können dies zu einer idealen Funktion zum Fördern aktuellerer Dokumente machen, wenn für x `rord(datefield)` gilt. |`recip(myfield,m,a,b)`<br>`recip(rord(creationDate),1,1000,1000)` |
| rad |Konvertiert Grade in Bogenmaße |`rad(x)` |
| rint |Rundet auf die nächste ganze Zahl auf |`rint(x)`  <br> `rint(5.6)` – gibt 6 zurück |
| sin |Gibt den Sinus eines Winkels zurück |`sin(x)` |
| sinh |Gibt den hyperbolischen Sinus eines Winkels zurück |`sinh(x)` |
| Skalierung |Skaliert Werte der Funktion x so, dass sie zwischen die angegebenen Werte für minTarget und maxTarget fallen (einschließlich). Die aktuelle Implementierung durchläuft alle Funktionswerte, um den Minimal- und Maximalwert abzurufen, damit die richtige Skalierung gewählt werden kann. Bei der aktuellen Implementierung kann nicht zwischen Dokumenten, die gelöscht wurden, und Dokumenten, die keinen Wert aufweisen, unterschieden werden. In diesen Fällen werden 0,0-Werte verwendet. Dies bedeutet Folgendes: Wenn Werte normalerweise größer als 0,0 sind, kann sich trotzdem 0,0 als Minimalwert für die Zuordnung ergeben. In diesen Fällen kann eine geeignete `map()`-Funktion als Problemumgehung verwendet werden, um 0,0 in einen Wert im realen Bereich zu ändern. Dies ist hier gezeigt: `scale(map(x,0,0,5),1,2)`. |`scale(x,minTarget,maxTarget)`<br>`scale(x,1,2)` : Skaliert die Werte von x so, dass alle Werte zwischen 1 und 2 liegen (einschließlich). |
| sqrt |Gibt die Quadratwurzel des angegebenen Werts oder der Funktion zurück. |`sqrt(x)`<br>`sqrt(100)`<br>`sqrt(sum(x,100))` |
| strdist |Berechnet den Abstand zwischen zwei Zeichenfolgen. Verwendet die StringDistance-Schnittstelle der Lucene-Rechtschreibprüfung und unterstützt alle Implementierungen, die in diesem Paket verfügbar sind. Sie ermöglicht Anwendungen die Verwendung eigener Implementierungen über die Ressourcenladefunktionen von Solr. Für „strdist“ wird `(string1, string2, distance measure)` verwendet. Mögliche Werte für die Abstandsmessung sind: <br>jw: Jaro-Winkler<br>edit: Levenstein- oder Edit-Abstand<br>ngram: Wenn NGramDistance angegeben ist, kann optional auch in die ngram-Größe übergeben werden. Standard ist 2.<br>FQN: Vollqualifizierter Klassenname für eine Implementierung der StringDistance-Schnittstelle. Muss über einen no-arg-Konstruktor verfügen. |`strdist("SOLR",id,edit)` |
| sub |Gibt x-y aus `sub(x,y)`zurück. |`sub(myfield,myfield2)`<br>`sub(100,sqrt(myfield))` |
| Sum |Gibt die Summe mehrerer Werte oder Funktionen zurück, die in einer durch Kommas getrennte Liste angegeben sind. `add(...)` kann auch als Alias für diese Funktion verwendet werden. |`sum(x,y,...)`<br>`sum(x,1)`<br>`sum(x,y)`<br>`sum(sqrt(x),log(y),z,0.5)`<br>`add(x,y)` |
| termfreq |Gibt zurück, wie oft ein „Begriff“ in einem Feld (hier „Text“) eines Dokuments auftaucht. |termfreq(Text,'Begriff') |
| tan |Gibt den Tangens eines Winkels zurück |`tan(x)` |
| tanh |Gibt den hyperbolischen Tangens eines Winkels zurück |`tanh(x)` |

## <a name="search-field-and-facet-reference"></a>Search-Feld- und Facetreferenz
Wenn Sie die Protokollsuche verwenden, um Daten zu finden, zeigen die Ergebnisse verschiedene Felder und Facetten an. Jedoch sind einige Informationen, die Sie sehen, möglicherweise  nicht sehr aussagekräftig. Mit den folgenden Informationen können Sie die Ergebnisse besser verstehen.

| Feld | Suchtyp | Beschreibung |
| --- | --- | --- |
| TenantId |Alle |Verwendet für die Datenpartitionierung |
| TimeGenerated |Alle |Zur Ausführung der Zeitachse, des Timeselectors (Suche und andere Bildschirme) verwendet. Hier wird dargestellt, wann die Datenmenge (in der Regel auf dem Agent) generiert wurde. Die Zeit wird im ISO-Format ausgedrückt und ist immer UTC. Im Fall von „Typen“, die auf einer vorhandenen Instrumentation basieren (d.h. die Ereignisse in einem Protokoll), ist dies in der Regel Echtzeit, in der Eintrag/Zeile/Protokolldatensatz protokolliert wurde. Für einige andere Typen, die erstellt werden, entweder über Verwaltungspacks oder in der Cloud – d.h. Empfehlungen/Warnungen/Updateagent/usw. Dies ist der Zeitpunkt, zu dem neue Datenelemente mit einer Momentaufnahme einer Konfiguration erfasst wurden oder eine Empfehlung/Warnung erzeugt wurde, die darauf basiert. |
| EventId |Ereignis |EventID im Windows-Ereignisprotokoll |
| EventLog |Ereignis |Ereignisprotokoll, in dem das Ereignis von Windows protokolliert wurde. |
| EventLevelName |Ereignis |Kritisch / Warnung / Information / Erfolg |
| EventLevel |Ereignis |Numerischer Wert für Kritisch / Warnung / Information / Erfolg (verwenden Sie stattdessen EventLevelName für leichtere/lesbarere Abfragen) |
| SourceSystem |Alle |Woher die Daten stammen (im Hinblick auf den Modus „Anfügen“ für den Dienst, d.h. Operations Manager, OMS (= generiert die Daten in der Cloud), Azure Storage (Daten aus WAD) usw. |
| ObjectName |PerfHourly |Name des Windows-Leistungsobjekts |
| InstanceName |PerfHourly |Name der Windows-Leistungsindikatorinstanzen |
| CountName |PerfHourly |Name des Windows-Leistungsindikators |
| ObjectDisplayName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Anzeigename des Objekts, das das Ziel einer Leistungssammlungsregel in Operations Manager oder das Objekt ist, das von Operational Insights ermittelt wurde, oder für das die Warnung generiert wurde. |
| RootObjectName |PerfHourly, ConfigurationAlert, ConfigurationObject, ConfigurationObjectProperty |Anzeigename des übergeordneten Elements des übergeordneten Elements (in einer doppelten Hostbeziehung: d. h. SqlDatabase gehostet von SqlInstance gehostet von Windows-Computer) des Objekts, das das Ziel einer Leistungssammlungsregel in Operations Manager oder das Objekt ist, das von Operational Insights ermittelt wurde, oder für das die Warnung generiert wurde. |
| Computer |Die meisten Typen. |Computername, zu dem die Daten gehören. |
| DeviceName |ProtectionStatus |Computername, zu dem die Daten gehören (identisch mit "Computer"). |
| DetectionId |ProtectionStatus | |
| ThreatStatusRank |ProtectionStatus |"ThreatStatusRank" ist eine numerische Darstellung des Bedrohungsstatus und ähnelt HTTP-Antwortcodes. Wir haben zwischen den Zahlen Lücken gelassen (weshalb keine Bedrohungen 150 und nicht 100 oder 0 ist), sodass Platz für das Hinzufügen eines neuen Status ist. Wenn ein Rollup für den Bedrohungs- und Schutzstatus durchgeführt wird, soll der schlechteste Status angezeigt werden, in dem sich der Computer während des ausgewählten Zeitraums befand. Zur Bewertung der verschiedenen Zustände werden Zahlen verwendet, sodass nach dem Datensatz mit der höchsten Nummer gesucht werden kann. |
| ThreatStatus |ProtectionStatus |Beschreibung des "ThreatStatus", 1:1 "ThreatStatusRank" zugeordnet. |
| TypeofProtection |ProtectionStatus |Antimalware-Produkt, das auf dem Computer erkannt wird: keins, Microsoft Malware Removal Tool, Forefront, usw.. |
| ScanDate |ProtectionStatus | |
| SourceHealthServiceId |ProtectionStatus, RequiredUpdate |Integritätsdienst-ID für den Agent dieses Computers. |
| HealthServiceId |Die meisten Typen. |Integritätsdienst-ID für den Agent dieses Computers. |
| ManagementGroupName |Die meisten Typen. |Name der Verwaltungsgruppe für mit Operations Manager verbundene Agents; andernfalls null/leer. |
| ObjektType |ConfigurationObject |Typ (wie in Management Pack „Type“/Klasse von Operations Manager) für dieses Objekt, das von der Log Analytics-Konfigurationsbewertung ermittelt wurde. |
| UpdateTitle |RequiredUpdate |Name des Updates, das als nicht installiert erkannt wurde. |
| PublishDate |RequiredUpdate |Wann fand das Update statt, das auf Microsoft Update veröffentlicht wurde? |
| Server |RequiredUpdate |Computername, zu dem die Daten gehören (identisch mit "Computer"). |
| product |RequiredUpdate |Produkt, auf das die Aktualisierung angewendet wird. |
| UpdateClassification |RequiredUpdate |Updatetyp (Updaterollup, Servicepack, usw.). |
| KBID |RequiredUpdate |Die KB-Artikel-ID, die diese bewährte Methode  oder das Update beschreibt. |
| WorkflowName |ConfigurationAlert |Name der Regel oder des Monitors, die oder der die Warnung erzeugt hat. |
| Severity |ConfigurationAlert |Schweregrad der Warnung. |
| Priorität |ConfigurationAlert |Priorität der Warnung. |
| IsMonitorAlert |ConfigurationAlert |Wird diese Warnung von einem Monitor (True) oder einer Regel (False) generiert? |
| AlertParameters |ConfigurationAlert |XML-Code mit den Parametern der Log Analytics-Warnung |
| Kontext |ConfigurationAlert |XML-Code mit dem „Kontext“ der Log Analytics-Warnung |
| Workload |ConfigurationAlert |Technologie oder "Arbeitsauslastung", auf die sich die Warnung bezieht. |
| AdvisorWorkload |Empfehlung |Technologie oder "Arbeitsauslastung", auf die sich die Empfehlung bezieht. |
| Beschreibung |ConfigurationAlert |Beschreibung der Warnung (kurz) |
| DaysSinceLastUpdate |UpdateAgent |Vor wie vielen Tagen (in Bezug auf 'TimeGenerated' für diesen Datensatz) hat dieser Agent ein Update von WSUS oder Microsoft Update installiert? |
| DaysSinceLastUpdateBucket |UpdateAgent |Eine Kategorisierung in "Perioden" basierend auf DaysSinceLastUpdate, wann zuletzt ein Update von WSUS/Microsoft Update installiert wurde. |
| AutomaticUpdateEnabled |UpdateAgent |Ist die Überprüfung der automatischen Aktualisierung auf diesem Agent aktiviert oder deaktiviert? |
| AutomaticUpdateValue |UpdateAgent |Ist die Überprüfung der automatischen Aktualisierung auf automatisches Herunterladen und Installieren, nur Herunterladen oder nur Überprüfen festgelegt? |
| WindowsUpdateAgentVersion |UpdateAgent |Versionsnummer des Microsoft Update-Agents |
| WSUSServer |UpdateAgent |Auf welchen WSUS-Server ist dieser Update-Agent ausgerichtet? |
| OSVersion |UpdateAgent |Version des Betriebssystems, auf dem dieser Update-Agent ausgeführt wird. |
| NAME |Empfehlung, ConfigurationObjectProperty |Der Name/Titel der Empfehlung oder der Name der Eigenschaft von der Log Analytics-Konfigurationsbewertung |
| Wert |ConfigurationObjectProperty |Wert einer Eigenschaft der Log Analytics-Konfigurationsbewertung |
| KBLink |Empfehlung |URL zum KB-Artikel, der diese bewährte Methode  oder das Update beschreibt. |
| RecommendationStatus |Empfehlung |Empfehlungen gehören zu den wenigen Typen, deren Datensätze 'aktualisiert' und nicht nur dem Suchindex hinzugefügt werden. Mit diesem Status wird geändert, ob die Empfehlung aktiv/offen ist oder ob Log Analytics erkennt, dass das Problem gelöst wurde. |
| RenderedDescription |Ereignis |Gerenderte Beschreibung (wiederverwendeter Text mit aufgefüllten Parametern) eines Windows-Ereignisses. |
| ParameterXml |Ereignis |XML mit den Parametern im Abschnitt 'Data' eines Windows-Ereignisses (wie in der Ereignisanzeige dargestellt). |
| EventData |Ereignis |XML mit dem gesamten 'Data'-Abschnitt eines Windows-Ereignisses (wie in der Ereignisanzeige dargestellt). |
| Quelle |Ereignis |Ereignisprotokoll-Quelle, die das Ereignis generiert hat. |
| EventCategory |Ereignis |Ereigniskategorie, direkt aus dem Windows-Ereignisprotokoll. |
| UserName |Ereignis |Benutzername des Windows-Ereignisses (in der Regel NT AUTHORITY\LOCALSYSTEM). |
| SampleValue |PerfHourly |Durchschnittswert für die stündliche Aggregation eines Leistungsindikators. |
| Min |PerfHourly |Mindestwert im stündlichen Intervall eines stündlich aggregierten Leistungsindikators. |
| max |PerfHourly |Maximalwert im stündlichen Intervall eines stündlich aggregierten Leistungsindikators. |
| Percentile95 |PerfHourly |Der 95. Prozentwert für den stündlichen Intervall eines stündlich aggregierten Leistungsindikators. |
| SampleCount |PerfHourly |Wie viele Proben von „rohen“ Leistungsindikatoren verwendet wurden, um diesen stündlich aggregierten Datensatz zu erstellen. |
| Bedrohung |ProtectionStatus |Name der Malware gefunden |
| StorageAccount |W3CIISLog |Azure-Speicherkonto, aus dem das Protokoll gelesen wurde. |
| AzureDeploymentID |W3CIISLog |Die Azure-Bereitstellungs-ID des Cloud-Dienstes, zu dem das Protokoll gehört. |
| Rolle |W3CIISLog |Rolle des Azure-Cloud-Dienstes, zu dem das Protokoll gehört. |
| RoleInstance |W3CIISLog |RoleInstance der Azure-Rolle, zu dem das Protokoll gehört. |
| sSiteName |W3CIISLog |IIS-Website, zu der das Protokoll gehört (Metabasis-Notation); das  "s-Sitename "-Feld im ursprünglichen Protokoll. |
| sComputerName |W3CIISLog |Das "s-Computername "-Feld im ursprünglichen Protokoll. |
| sIP |W3CIISLog |IP-Adresse des Servers, an die die HTTP-Anforderung adressiert wurde. Das "s-ip"-Feld im ursprünglichen Protokoll. |
| csMethod |W3CIISLog |HTTP-Methode (GET, POST, usw.), die vom Client in der HTTP-Anforderung verwendet wurde. Die cs-Methode im ursprünglichen Protokoll. |
| cIP |W3CIISLog |IP-Adresse des Client, von dem die HTTP-Anforderung stammt. Das "c-ip"-Feld im ursprünglichen Protokoll. |
| csUserAgent |W3CIISLog |HTTP-Benutzer-Agent, der vom Client deklariert wurde (im Browser oder auf andere Weise). Der "cs-user"-Agent im ursprünglichen Protokoll. |
| scStatus |W3CIISLog |HTTP-Statuscode (200/403/500/usw.), der vom Server an den Client zurückgegeben wurde. Der "cs"-Status im ursprünglichen Protokoll. |
| TimeTaken |W3CIISLog |Dauer (in Millisekunden), die die Anforderung bis zum Abschluss gebraucht hat. Das "timetaken"-Feld im ursprünglichen Protokoll. |
| csUriStem |W3CIISLog |Relativer Uri (ohne Hostadresse, d. h. "/ Suchen"), der angefordert wurde. Das "cs-uristem"-Feld im ursprünglichen Protokoll. |
| csUriQuery |W3CIISLog |URI-Abfrage. URI-Abfragen sind nur für dynamische Seiten erforderlich, z. B. ASP-Seiten, also enthält dieses Feld in der Regel einen Bindestrich für statische Seiten. |
| sPort |W3CIISLog |Serverport, an den die HTTP-Anforderung gesendet wurde (und die IIS überwacht, da sie davon übernommen wurde). |
| csUserName |W3CIISLog |Authentifizierter Benutzername, wenn die Anforderung authentifiziert und nicht anonym ist. |
| csVersion |W3CIISLog |Die in der Anforderung verwendete HTTP-Protokoll-Version (d. h.  "HTTP/1.1") |
| csCookie |W3CIISLog |Cookie-Informationen |
| csReferer |W3CIISLog |Website, die der Benutzer zuletzt besucht hat. Diese Website stellte eine Verknüpfung zur aktuellen Website her. |
| csHost |W3CIISLog |Angeforderter Hostheader (d. h. "www.mysite.com"). |
| scSubStatus |W3CIISLog |Untergeordneter Fehlercode |
| scWin32Status |W3CIISLog |Windows-Statuscode |
| csBytes |W3CIISLog |In der Anforderung vom Client an den Server gesendete Bytes. |
| scBytes |W3CIISLog |In der Antwort vom Server an den Client zurückgegebene Bytes. |
| ConfigChangeType |ConfigurationChange |Art der Änderung (WindowsServices / Software / usw.) |
| ChangeCategory |ConfigurationChange |Kategorie der Änderung (Geändert / Hinzugefügt / Entfernt) |
| SoftwareType |ConfigurationChange |Softwaretyp (Update / Anwendung) |
| SoftwareName |ConfigurationChange |Name der Software (gilt nur für Softwareänderungen). |
| Herausgeber |ConfigurationChange |Hersteller, der die Software veröffentlicht (gilt nur für Softwareänderungen). |
| SvcChangeType |ConfigurationChange |Typ der Änderung, die auf einen Windowsdienst angewendet wurde (State / StartupType / Path / ServiceAccount) – gilt nur für Änderungen eines Windows-Dienstes. |
| SvcDisplayName |ConfigurationChange |Anzeigename des Dienstes, der geändert wurde. |
| SvcName |ConfigurationChange |Name des Dienstes, der geändert wurde. |
| SvcState |ConfigurationChange |Neuer (aktueller) Status des Dienstes. |
| SvcPreviousState |ConfigurationChange |Vorheriger bekannter Dienststatus (gilt nur, wenn der Dienstzustand geändert wurde). |
| SvcStartupType |ConfigurationChange |Starttyp des Dienstes |
| SvcPreviousStartupType |ConfigurationChange |Vorheriger Starttyp des Dienstes (gilt nur, wenn der Starttyp des Diensts geändert wurde). |
| SvcAccount |ConfigurationChange |Dienstkonto |
| SvcPreviousAccount |ConfigurationChange |Vorheriges Dienstkonto (gilt nur, wenn das Dienstkonto geändert wurde). |
| SvcPath |ConfigurationChange |Pfad zur ausführbaren Datei des Windows-Dienstes |
| SvcPreviousPath |ConfigurationChange |Vorherige Pfad der ausführbaren Datei des Windows-Dienstes (gilt nur, wenn dieser geändert wurde). |
| SvcDescription |ConfigurationChange |Beschreibung des Dienstes |
| Zurück |ConfigurationChange |Vorheriger Zustand dieser Software (Installiert / Nicht Installiert / vorherige Version) |
| Aktuell |ConfigurationChange |Neuester Zustand dieser Software (Installiert / Nicht Installiert / aktuelle Version) |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Protokollsuchen:

* Machen Sie sich mit [Protokollsuchvorgängen](log-analytics-log-searches.md) vertraut, um ausführliche Informationen anzuzeigen, die von Lösungen gesammelt werden.
* Lesen Sie [Benutzerdefinierte Felder in Log Analytics](log-analytics-custom-fields.md) , um mehr über Protokollsuchen zu erfahren.

