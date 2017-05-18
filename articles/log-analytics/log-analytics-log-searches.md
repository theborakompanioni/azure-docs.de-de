---
title: Suchen von Daten mithilfe der Protokollsuchen in Azure Log Analytics | Microsoft Docs
description: "Protokollsuchen ermöglichen Ihnen das Kombinieren und Korrelieren beliebiger Computerdaten aus mehreren Quellen in Ihrer Umgebung."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 0d7b6712-1722-423b-a60f-05389cde3625
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: b005d0fb25483f3dce14133038d7759dff07fc7c
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="find-data-using-log-searches"></a>Suchen von Daten mithilfe der Protokollsuche

Der Kern von Log Analytics ist die Protokollsuchfunktion, die Ihnen das Kombinieren und Korrelieren beliebiger Computerdaten aus mehreren Quellen in Ihrer Umgebung ermöglicht. Lösungen werden auch von der Protokollsuche beim Bereitstellen von Metriken unterstützt, die eine Pivotierung in Bezug auf einen bestimmten Problembereich bieten.

Auf der Seite Search können Sie eine Abfrage erstellen und die Ergebnisse dann bei der Suche anhand von Facetsteuerelementen filtern. Sie können auch erweiterte Abfragen zum Transformieren, Filtern und Berichten für Ihre Ergebnisse erstellen.

Allgemeine Protokollsuchabfragen werden auf den meisten Lösungsseiten angezeigt. In der gesamten OMS-Konsole können Sie anhand der Protokollsuche auf Kacheln klicken oder einen Drilldown in andere Elemente ausführen, um Details zum Element anzuzeigen.

In diesem Lernprogramm werden Beispiele zu den Grundlagen der Verwendung von Protokollsuchen veranschaulicht.

Es geht los mit einfachen und praktischen Beispielen, auf denen dann aufgebaut wird, sodass Sie einen Überblick über praktische Anwendungsfälle erhalten, wie Sie die Syntax zur Gewinnung gewünschter Erkenntnisse aus den Daten verwenden.

Nachdem Sie sich mit den Suchtechniken vertraut gemacht haben, können Sie sich die [Referenz zur Log Analytics-Suche](log-analytics-search-reference.md)ansehen.

## <a name="use-basic-filters"></a>Verwenden einfacher Filter
Als Erstes sollten Sie wissen, dass der erste Teil einer Suchabfrage vor einem senkrechten Strich (Pipe-Zeichen „|“) immer ein *Filter* ist. Sie können sich dies als WHERE-Klausel in TSQL vorstellen: Der Filter bestimmt, *welche* Teilmenge der Daten aus dem Datenspeicher von OMS abgerufen werden soll. Beim Suchen im Datenspeicher geht es größtenteils um die Angabe der Merkmale der Daten, die Sie extrahieren möchten. Daher ist es normal, dass eine Abfrage mit der WHERE-Klausel beginnt.

Die grundlegendsten Filter, die Sie verwenden können, sind *Schlüsselwörter*wie 'Error' oder 'Timeout', oder ein Computername. Diese Typen von einfachen Abfragen geben in der Regel verschiedene Datenformen innerhalb des gleichen Ergebnissatzes zurück. Dies liegt daran, dass Log Analytics unterschiedliche *Arten* von Daten im System enthält.

### <a name="to-conduct-a-simple-search"></a>Durchführen einer einfachen Suche
1. Klicken Sie im OMS-Portal auf **Protokollsuche**.  
    ![Kachel „Protokollsuche“](./media/log-analytics-log-searches/oms-overview-log-search.png)
2. Geben Sie in das Abfragefeld `error` ein und klicken Sie dann auf **Search**.  
    ![Suche nach „error“ (Fehler)](./media/log-analytics-log-searches/oms-search-error.png)  
    Angenommen, die Abfrage für `error` gibt in der folgenden Abbildung 100.000 **Ereignisdatensätze** zurück (von Log Management erfasst), 18 **ConfigurationAlert**-Datensätze (von der Konfigurationsbewertung generiert) und 12 **ConfigurationChange**-Datensätze (von der Änderungsnachverfolgung aufgezeichnet).   
    ![Suchergebnisse](./media/log-analytics-log-searches/oms-search-results01.png)  

Diese Filter sind keine echten Objekttypen/-klassen. *Type* ist nur ein Tag, eine Eigenschaft oder eine Zeichenfolge/ein Name/eine Kategorie, der oder die einem Teil der Daten zugeordnet ist. Einige Dokumente im System sind als **Type:ConfigurationAlert**, **Type:Perf** oder **Type:Event** usw. gekennzeichnet. Alle Suchergebnisse, Dokumente, Datensätze oder Einträge zeigen die Basiseigenschaften und deren Werte für jedes dieser Datenelemente. Sie können die Feldnamen im Filter angeben, wenn Sie nur die Datensätze abrufen möchten, in dem das Feld den angegebenen Wert besitzt.

*Type* ist im Prinzip nur ein Feld, über das alle Datensätze verfügen, es unterscheidet sich nicht von anderen Feldern. Dies wurde basierend auf dem Wert des Type-Felds festgelegt. Dieser Datensatz hat eine unterschiedliche Form oder Art. Übrigens entspricht **Type=Perf** oder **Type=Event** auch der Syntax, die Sie lernen müssen, um Leistungsdaten oder Ereignisse abzufragen.

Sie können hinter dem Feldnamen und vor dem Wert entweder einen Doppelpunkt (:) oder ein Gleichheitszeichen (=) verwenden. **Type:Event** und **Type=Event** sind bedeutungsgleich, sodass Sie sich das gewünschte Format aussuchen können.

Wenn also die Type=Perf-Datensätze ein Feld namens „CounterName“ aufweisen, können Sie eine Abfrage wie `Type=Perf CounterName="% Processor Time"`schreiben.

Dadurch erhalten Sie nur die Leistungsdaten, bei denen der Name des Leistungsindikators "Prozessorzeit (%)" ist.

### <a name="to-search-for-processor-time-performance-data"></a>Suche nach Leistungsdaten zur Prozessorzeit
* Geben Sie im Suchabfragefeld `Type=Perf CounterName="% Processor Time"`

Sie können auch spezifischer sein und **InstanceName=_'Total'** in der Abfrage verwenden, was ein Windows-Leistungsindikator ist. Sie können auch eine Facette und einen weiteren Wert **field:value**auswählen. Der Filter wird automatisch zu Ihrem Filter in der Abfrageleiste hinzugefügt. Dies wird in der folgenden Abbildung veranschaulicht. Hier erfahren Sie, wo Sie zum Hinzufügen von **InstanceName:'_Total'** zur Abfrage klicken müssen, ohne etwas einzugeben.

!["search facet" (Facette durchsuchen)](./media/log-analytics-log-searches/oms-search-facet.png)

Ihre Abfrage wird jetzt zu `Type=Perf CounterName="% Processor Time" InstanceName="_Total"`

In diesem Beispiel müssen Sie **Type=Perf** nicht angeben, um dieses Ergebnis zu erhalten. Da die Felder „CounterName“ und „InstanceName“ nur für Datensätze vom Typ „Type=Perf“ vorhanden sind, ist die Abfrage spezifisch genug, um die gleichen Ergebnisse wie die vorherige längere Abfrage zurückzugeben:

```
CounterName="% Processor Time" InstanceName="_Total"
```

Das liegt daran, dass alle Filter in der Abfrage als in *UND* miteinander ausgewertet werden. Je mehr Felder Sie zu den Kriterien hinzufügen, desto geringer die Anzahl der Ergebnisse, die spezifischer und verfeinerter sind.

Zum Beispiel ist die Abfrage `Type=Event EventLog="Windows PowerShell"` identisch mit `Type=Event AND EventLog="Windows PowerShell"`. Sie gibt alle Ereignisse zurück, die in und aus dem Windows PowerShell-Ereignisprotokoll erfasst wurden. Wenn Sie einen Filter mehrmals durch wiederholtes Auswählen der gleichen Facette hinzufügen, handelt es sich um ein rein kosmetisches Problem: Möglicherweise ist die Suchleiste überladen, es werden jedoch weiterhin dieselben Ergebnisse zurückgegeben, da der implizite AND-Operator immer vorhanden ist.

Sie können den impliziten AND-Operator ganz einfach umkehren, indem Sie explizit einen NOT-Operator verwenden. Beispiel:

Die Abfrage `Type:Event NOT(EventLog:"Windows PowerShell")` oder deren Entsprechung `Type=Event EventLog!="Windows PowerShell"` gibt alle Ereignisse aus allen anderen Protokollen zurück, bei denen es sich NICHT um das Windows PowerShell-Protokoll handelt.

Oder Sie verwenden einen anderen booleschen Operators wie 'OR'. Die folgende Abfrage gibt die Datensätze zurück, für die das Ereignisprotokoll entweder eine Anwendung ODER ein System ist.

```
EventLog=Application OR EventLog=System
```

Verwenden Sie obige Abfrage, um Einträge für beide Protokolle im gleichen Ergebnissatz zu erhalten.

Wenn Sie jedoch den OR-Operator entfernen, indem Sie das implizite AND stehen lassen, gibt die folgende Abfrage keine Ergebnisse zurück, da es keinen Eintrag im Ereignisprotokoll gibt, der zu BEIDEN Protokollen gehört. Jeder Eintrag im Ereignisprotokoll wurde nur in eins der beiden Protokolle geschrieben.

```
EventLog=Application EventLog=System
```


## <a name="use-additional-filters"></a>Verwenden zusätzlicher Filter
Die folgende Abfrage gibt Einträge für zwei Ereignisprotokolle für alle Computer zurück, die Daten gesendet haben.

```
EventLog=Application OR EventLog=System
```

!["search results" (Ergebnisse durchsuchen)](./media/log-analytics-log-searches/oms-search-results03.png)

Durch Auswahl eines der Felder oder Filter wird die Abfrage auf einen bestimmten Computer beschränkt, und alle anderen werden ausgeschlossen. Die resultierende Abfrage ähnelt der folgenden.

```
EventLog=Application OR EventLog=System Computer=SERVER1.contoso.com
```

Dies entspricht aufgrund des impliziten AND der folgenden Abfrage.

```
EventLog=Application OR EventLog=System AND Computer=SERVER1.contoso.com
```

Jede Abfrage wird in der folgenden expliziten Reihenfolge ausgewertet. Beachten Sie die Klammer.

```
(EventLog=Application OR EventLog=System) AND Computer=SERVER1.contoso.com
```

Wie beim Ereignisprotokoll-Feld können Sie Daten nur für einen Satz bestimmter Computern durch Hinzufügen von OR abrufen. Beispiel:

```
(EventLog=Application OR EventLog=System) AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com OR Computer=SERVER3.contoso.com)
```

Ähnlich dazu wird die folgende Abfrage **CPU-Zeit (%)** nur für die beiden ausgewählten Computer zurückgegeben.

```
CounterName="% Processor Time"  AND InstanceName="_Total" AND (Computer=SERVER1.contoso.com OR Computer=SERVER2.contoso.com)
```


### <a name="boolean-operators"></a>Boolesche Operatoren
Mit „datetime“- und numerischen Feldern können Sie Werte mit *größer als*, *kleiner als*, und *kleiner als oder gleich* suchen. Sie können einfache Operatoren wie z. B. >, <, >=, < =,! = in der Suchabfrageleiste verwenden.

Sie können ein bestimmtes Ereignisprotokoll für einen bestimmten Zeitraum abfragen. Beispielsweise werden die letzten 24 Stunden mit dem folgenden mnemonischen Ausdruck dargestellt.

```
EventLog=System TimeGenerated>NOW-24HOURS
```


#### <a name="to-search-using-a-boolean-operator"></a>Suchen mithilfe eines booleschen Operators
* Geben Sie im Suchabfragefeld `EventLog=System TimeGenerated>NOW-24HOURS`  
    ![Boolesche Suche](./media/log-analytics-log-searches/oms-search-boolean.png)

Sie können das Zeitintervall zwar grafisch steuern, was die meiste Zeit auch sinnvoll ist, es hat jedoch auch Vorteile, einen Zeitfilter direkt in die Abfrage einzuschließen. Beispielsweise funktioniert dies hervorragend mit Dashboards, bei denen Sie die Zeit für jede Kachel überschreiben können, unabhängig von der *globalen* Zeitauswahl auf der Dashboardseite. Weitere Informationen finden Sie unter [Time Matters in Dashboard](http://cloudadministrator.wordpress.com/2014/10/19/system-center-advisor-restarted-time-matters-in-dashboard-part-6/)("Zeit spielt in Dashboards eine Rolle", auf Englisch).

Beim Filtern nach Zeit sollten Sie bedenken, dass Sie Resultate für die *Schnittmenge* der beiden Zeiträume erhalten: den im OMS-Portal (S1) und den in der Abfrage (S2) angegebenen Zeitraum.

![Schnittmenge](./media/log-analytics-log-searches/oms-search-intersection.png)

Das heißt, wenn die Zeiträume sich nicht überschneiden, z.B. wenn im OMS-Portal **diese Woche** und in der Abfrage **letzte Woche** definiert ist, gibt es keine Überschneidung, und Sie erhalten keine Ergebnisse.

Vergleichsoperatoren für das Feld "TimeGenerated" sind auch in anderen Situationen nützlich. Beispielsweise in numerischen Feldern.

Beispielsweise wird angenommen, dass Warnungen für die Konfigurationsbewertung die folgenden Schweregrade haben:

* 0 = Information
* 1 = Warnung
* 2 = Kritischer Fehler

Sie können mit der folgenden Abfrage sowohl nach Warnungen als auch kritischen Fehler filtern und Informationen ausschließen:

```
Type=ConfigurationAlert  Severity>=1
```


Sie können auch Bereichsabfragen verwenden. Das bedeutet, dass Sie Anfang und Ende der Wertebereiche in einer Sequenz bereitstellen können. Wenn Sie z. B. Ereignisse aus dem Ereignisprotokoll von Operations Manager erhalten möchten, bei denen die EventID größer als oder gleich 2100, aber nicht größer als 2199 ist, so gibt dies die folgende Abfrage zurück.

```
Type=Event EventLog="Operations Manager" EventID:[2100..2199]
```


> [!NOTE]
> Die zu verwendende Bereichssyntax ist der Doppelpunkt (:) "Feld:Trennzeichen"  und *nicht* das Gleichheitszeichen (=). Schließen Sie das untere und obere Ende des Bereichs in eckige Klammern ein, und trennen Sie diese durch zwei Punkte (..).
>
>

## <a name="manipulate-search-results"></a>Bearbeiten von Suchergebnissen
Wenn Sie nach Daten suchen, sollten Sie Ihre Suchabfrage verfeinern und die Ergebnisse gut kontrollieren können. Wenn Ergebnisse abgerufen werden, können Sie die Befehle anwenden, um sie zu verändern.

Befehle in Log Analytics-Suchen *müssen* nach dem senkrechten Strich (|) folgen. Ein Filter muss immer der erste Teil einer Abfragezeichenfolge sein. Er definiert den Datensatz, mit dem Sie arbeiten, und "leitet" diese Ergebnisse dann in einen Befehl. Mit der Pipe können Sie anschließend zusätzliche Befehle hinzufügen. Dies ist in etwa mit der Windows PowerShell-Pipeline vergleichbar.

Im Allgemeinen versucht die Log Analytics-Suchsprache, sich am PowerShell-Stil und den Richtlinien zu orientieren, um IT-Experten den Einstieg zu erleichtern und die Lernkurve zu verbessern.

Befehle bestehen aus Verben, sodass Sie leicht erkennen können, was diese tun.  

### <a name="sort"></a>Sortieren
Mit dem Sortierbefehl können Sie die Sortierreihenfolge durch ein oder mehrere Felder festlegen. Auch wenn Sie ihn in der Standardeinstellung nicht verwenden, wird eine zeitlich absteigende Reihenfolge erzwungen. Die aktuellsten Ergebnisse stehen immer am Anfang der Suchergebnisse. Das bedeutet, dass beim Ausführen einer Suche mit `Type=Event EventID=1234` Folgendes wirklich ausgeführt wird:

```
Type=Event EventID=1234 **| Sort TimeGenerated desc**
```

Das liegt daran, dass dies die Art von Erfahrung ist, die Sie aus Protokollen kennen. Beispielsweise in der Windows-Ereignisanzeige.

Sie können die Funktion "Sortieren" verwenden, um die Form der zurückgegebenen Ergebnisse zu ändern. Die folgenden Beispiele veranschaulichen die Funktionsweise.

```
Type=Event EventID=1234 | Sort TimeGenerated asc
```

```
Type=Event EventID=1234 | Sort Computer asc
```

```
Type=Event EventID=1234 | Sort Computer asc,TimeGenerated desc
```


Die einfachen Beispiele oben zeigen die Funktionsweise von Befehlen – sie ändern die Form der Ergebnisse, die der Filter zurückgegeben hat.

### <a name="limit-and-top"></a>"Limit" und "Top"
Eine weiterer wenig bekannter Befehl ist LIMIT. "Limit" ist ein PowerShell-ähnliches Verb. "Limit" ist funktionell identisch zum TOP-Befehl. Die folgenden Abfragen geben die gleichen Ergebnisse zurück.

```
Type=Event EventID=600 | Limit 1
```

```
Type=Event EventID=600 | Top 1
```


#### <a name="to-search-using-top"></a>Suchen mit "Top"
* Geben Sie im Suchabfragefeld `Type=Event EventID=600 | Top 1`  ein  
    ![Suchen mit „Top“](./media/log-analytics-log-searches/oms-search-top.png)

In der Abbildung oben gibt es 358.000 Datensätze mit EventID=600. Die Felder, Facetten und Filter auf der linken Seite zeigen immer Informationen zu den Ergebnissen an, die *vom Filterteil* der Abfrage zurückgegeben werden, also jeweils dem Teil vor dem senkrechten Strich. Im **Ergebnis** -Bereich wird nur das einzelne neueste Ergebnis zurückgegeben, da der Beispielbefehl die Ergebnisse geformt und transformiert hat.

### <a name="select"></a>Wählen Sie
Der SELECT-Befehl verhält sich wie das Select-Objekt in PowerShell. Er gibt gefilterte Ergebnisse zurück, die nicht all ihre ursprünglichen Eigenschaften aufweisen. Stattdessen wählt er nur die Eigenschaften aus, die Sie angeben.

#### <a name="to-run-a-search-using-the-select-command"></a>Suchen mit dem "Select"-Befehl
1. Geben Sie in der Suche `Type=Event` ein und klicken Sie dann auf **Search**.
2. Klicken Sie in einem der Ergebnisse auf **+ mehr anzeigen** , um alle Eigenschaften der Ergebnisse anzuzeigen.
3. Wählen Sie einige davon explizit aus, und die Abfrage ändert sich zu `Type=Event | Select Computer,EventID,RenderedDescription`.  
    ![Suchen mit „Select“](./media/log-analytics-log-searches/oms-search-select.png)

Dieser Befehl ist besonders nützlich, wenn Sie die Suchergebnisse steuern möchten und nur die Teile der Daten wählen, die für Ihre Untersuchung eine Rolle spielen, also häufig nicht der vollständige Datensatz. Dies ist auch nützlich, wenn Datensätze verschiedener Typen *einige* gemeinsame Eigenschaften aufweisen, dies aber nicht für *alle* Eigenschaften gilt. Sie können Ausgaben generieren, die natürlicher wie eine Tabelle aussehen oder gut funktionieren, wenn sie in eine CSV-Datei exportiert und dann in Excel bearbeitet werden.

## <a name="use-the-measure-command"></a>Verwenden des "Measure"-Befehls
MEASURE ist einer der vielseitigsten Befehle von Log Analytics-Suchen. Sie können damit statistische *Funktionen* auf Ihre Daten anwenden und die Ergebnisse nach einem bestimmten Feld gruppieren. Es gibt mehrere statistische Funktionen, die "Measure" unterstützt.

### <a name="measure-count"></a>"Measure count()"
Die erste statistische Funktion, die verwendet werden kann und leicht zu verstehen ist, ist die *count()* -Funktion.

Ergebnisse aus einer beliebigen Suchabfrage wie z. B. `Type=Event` zeigen auf der linken Seite der Suchergebnisse Filter, die auch als Facetten bezeichnet werden. Die Filter zeigen eine Verteilung der Werte anhand eines bestimmten Felds für die Ergebnisse in der ausgeführten Suche an.

!["search measure count" ("Measure"-Count durchsuchen)](./media/log-analytics-log-searches/oms-search-measure-count01.png)

Zum Beispiel sehen Sie in der Abbildung oben das **Computer**-Feld, das anzeigt, dass innerhalb der fast 739 Millionen Ereignisse in den Ergebnissen 68 einmalige und eindeutige Werte für das **Computer**-Feld in diesen Datensätzen vorhanden sind. Die Kachel zeigt nur die fünf obersten an, welche die fünf häufigsten Werte sind, die in die **Computer** -Felder geschrieben werden, sortiert nach der Anzahl der Dokumente, die diesen bestimmten Wert in dem Feld enthalten. In der Abbildung sehen Sie, dass unter diesen fast 369.000 Ereignissen insgesamt 90.000 aus dem „OpsInsights04.contoso.com“-Computer stammen, 83.000 aus dem „DB03.contoso.com“-Computer usw.

Was geschieht, wenn Sie alle Werte sehen möchten, obwohl die Kachel nur die ersten fünf anzeigt?

Dies kann der "Measure"-Befehl mithilfe der "Count()"-Funktion erledigen. Diese Funktion verwendet keine Parameter. Geben Sie einfach das zu gruppierende Feld an, in diesem Fall das **Computer** -Feld:

`Type=Event | Measure count() by Computer`

!["search measure count" ("Measure"-Count durchsuchen)](./media/log-analytics-log-searches/oms-search-measure-count-computer.png)

Allerdings ist **Computer** nur ein Feld, das *in* jedem Datenelement verwendet wird. Es sind keine relationalen Datenbanken vorhanden und es gibt nirgendwo ein separates **Computer**-Objekt. Nur die Werte *in* den Daten beschreiben, durch welche Entität sie generiert wurden, und eine Anzahl weiterer Merkmale und Aspekte der Daten – daher der Begriff *Facette*. Allerdings können Sie auch eine Gruppierung nach anderen Feldern vornehmen. Da die ursprünglichen Ergebnisse von fast 739.000 Ereignissen, die in den „Measure“-Befehl geleitet werden, auch über ein Feld namens **EventID** verfügen, können Sie das gleiche Verfahren zum Gruppieren nach diesem Feld verwenden und eine Anzahl der Ereignisse nach EventID erhalten:

```
Type=Event | Measure count() by EventID
```

Wenn Sie sich nicht für die tatsächliche Anzahl an Datensätzen interessieren, die einen bestimmten Wert enthalten, sondern nur eine Liste der Werte selbst benötigen, können Sie einen *Select* -Befehl am Ende hinzufügen, und einfach die erste Spalte auswählen:

```
Type=Event | Measure count() by EventID | Select EventID
```

Dann können Sie die Ergebnisse verfeinern und sie in der Abfrage Vorsortieren, oder Sie klicken einfach auf die Spalten im Raster.

```
Type=Event | Measure count() by EventID | Select EventID | Sort EventID asc
```

#### <a name="to-search-using-measure-count"></a>Suchen mit dem "Measures"-Count
* Geben Sie im Suchabfragefeld `Type=Event | Measure count() by EventID`
* Fügen Sie `| Select EventID` an das Ende der Abfrage.
* Fügen Sie schließlich `| Sort EventID asc` an das Ende der Abfrage.

Einige wichtige Punkte sind zu beachten und hervorzuheben:

Erstens sind die angezeigten Ergebnisse nicht mehr die ursprünglichen Rohergebnisse. Stattdessen handelt es sich um aggregierte Ergebnisse – im Wesentlichen Gruppen von Ergebnissen. Während dies kein Problem ist, sollten Sie dennoch wissen, dass Sie mit einer sehr unterschiedliche Form von Daten interagieren, die sich von der ursprünglichen Rohform unterscheidet, die als Ergebnis der Aggregation/statistischen Funktion dynamisch erstellt wird.

Zweitens gibt der **Measure-Count** zurzeit nur die ersten 100 unterschiedlichen Ergebnisse zurück. Diese Beschränkung gilt nicht für andere statistische Funktionen. Daher müssen Sie in der Regel zunächst einen genaueren Filter für bestimmte Elemente zur Suche angeben, bevor Sie "Measure count()" anwenden.

## <a name="use-the-max-and-min-functions-with-the-measure-command"></a>Verwenden der "Max"- und "Min"-Funktion mit dem "Measure"-Befehl
Es gibt verschiedene Szenarios, in denen **Measure Max()** und **Measure Min()** nützlich sind. Da die Funktionen jedoch genau gegensätzlich sind, illustrieren wie "Max()", sodass Sie mit "Min()" selbst experimentieren können.

Wenn Sie nach Sicherheitsereignissen abfragen, haben diese eine **Ebenen**-Eigenschaft, die variieren kann. Beispiel:

```
Type=SecurityEvent
```

!["search measure count start" (Durchsuchen von "Measure"-Count starten)](./media/log-analytics-log-searches/oms-search-measure-max01.png)

Wenn Sie den höchsten Wert für alle Sicherheitsereignisse für einen gemeinsamen Computer nach Feld gruppiert erhalten möchten, verwenden Sie

```
Type=ConfigurationAlert | Measure Max(Level) by Computer
```

!["search measure max computer" ("Measure max"-Computer durchsuchen)](./media/log-analytics-log-searches/oms-search-measure-max02.png)

Zeigt dies für die Computer an, die **Ebenen**-Datensätze hatten. Die meisten von ihnen haben mindestens Ebene 8, viele Ebene 16.

```
Type=ConfigurationAlert | Measure Max(Severity) by Computer
```

!["search measure max time generated computer" ("Measure max time" generierten Computer durchsuchen)](./media/log-analytics-log-searches/oms-search-measure-max03.png)

Diese Funktion funktioniert gut mit Zahlen, aber auch mit DateTime-Feldern. Es empfiehlt sich, den letzten oder neuesten Zeitstempel für alle Daten zu prüfen, die für jeden Computer indiziert wurden. Zum Beispiel: Wann wurde das jeweils neueste Sicherheitsereignis für jeden Computer gemeldet?

```
Type=ConfigurationChange | Measure Max(TimeGenerated) by Computer
```

## <a name="use-the-avg-function-with-the-measure-command"></a>Verwenden der "Avg"-Funktion mit dem Measure-Befehl
Die statistische "Avg()"-Funktion kann zusammen mit "Measure" verwendet werden, sodass Sie den Durchschnittswert für ein Feld berechnen, und die Ergebnisse nach dem gleichen oder einem anderen Feld gruppieren können. Dies ist in einer Vielzahl von Fällen nützlich, z. B. für Leistungsdaten.

Wir beginnen mit den Leistungsdaten. Beachten Sie, dass mit OMS derzeit Leistungsindikatoren für Windows- und Linux-Computer erfasst werden.

Die grundlegende Abfrage zum Suchen *aller* Leistungsdaten ist:

```
Type=Perf
```

!["search avg start" ("Avg"-Suche starten)](./media/log-analytics-log-searches/oms-search-avg01.png)

Das Erste, was Sie sehen werden, sind drei Perspektiven, die Log Analytics anzeigt: Liste, die die eigentlichen Datensätze hinter den Diagrammen zeigt, Tabelle, die eine tabellarische Ansicht der Leistungsindikatordaten zeigt, und Metriken, die Diagramme für die Leistungsindikatoren zeigen.

In der Abbildung oben gibt es zwei Sätze von gekennzeichneten Feldern, die auf Folgendes hinweisen:

* Der erste Satz identifiziert den Namen des Windows-Leistungsindikators, den Objektnamen und den Instanznamen im Abfragefilter. Dies sind die Felder, die Sie wahrscheinlich am häufigsten als Facetten/Filter verwenden.
* **CounterValue** ist der tatsächliche Wert des Zählers. In diesem Beispiel ist der Wert *75*.
* **TimeGenerated** ist 12:51 Uhr im 24-Stunden-Format.

Hier finden Sie eine Ansicht der Metriken in einem Diagramm.

!["search avg start" ("Avg"-Suche starten)](./media/log-analytics-log-searches/oms-search-avg02.png)

Nachdem Sie sich über die Form des „Perf“-Datensatzes und andere Suchtechniken informiert haben, können Sie „measure avg()“ für die Aggregation dieser Art von numerischen Daten verwenden.

Hier ist ein einfaches Beispiel:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" | Measure Avg(CounterValue) by Computer
```

!["search avg samplevalue" ("Avg samplevalue" suchen)](./media/log-analytics-log-searches/oms-search-avg03.png)

In diesem Beispiel wählen Sie den Leistungsindikator für die CPU-Gesamtzeit und den Durchschnitt pro Computer. Wenn Sie die Ergebnisse auf die letzten sechs Stunden beschränken möchten, können Sie entweder das Zeitfilter-Steuerelement verwenden oder in der Abfrage Folgendes angeben:

```
Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer
```

### <a name="to-search-using-the-avg-function-with-the-measure-command"></a>Verwenden der "Avg"-Funktion zur Suche mit dem "Measure"-Befehl
* Geben Sie im Suchabfragefeld `Type=Perf  ObjectName:Processor  InstanceName:_Total  CounterName:"% Processor Time" TimeGenerated>NOW-6HOURS | Measure Avg(CounterValue) by Computer`ein.

Sie können Daten von *mehreren* Computern aggregieren und korrelieren. Angenommen, Sie haben eine Reihe von Hosts in einer Farm, in der jeder Knoten dem anderen entspricht und alle die gleiche Art von Arbeit verrichten und die Last ungefähr ausgeglichen ist. Sie können alle Indikatoren mit der folgenden Abfrage in einem Durchgang abrufen und Durchschnittswerte für die gesamte Farm erhalten. Sie beginnen damit, die Computer wie in folgendem Beispiel auszuwählen:

```
Type=Perf AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Sie verfügen nun über die Computer und sollten jetzt nur zwei Key Performance Indicators (KPIs) auswählen: "CPU-Auslastung in %" und "freier Speicherplatz in %". Daher wird dieser Teil der Abfrage zu:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS
```

Jetzt können Sie Computer und Indikatoren anhand des folgenden Beispiels hinzufügen:

```
Type=Perf InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03")
```

Da Sie eine sehr spezifische Auswahl haben, kann der **Measure Avg()** -Befehl den Durchschnitt nicht nur pro Computer, sondern über die gesamte Farm hinweg zurückgeben, und zwar ganz einfach durch die Gruppierung nach "CounterName". Beispiel:

```
Type=Perf  InstanceName:_Total  ((ObjectName:Processor AND CounterName:"% Processor Time") OR (ObjectName="LogicalDisk" AND CounterName="% Free Space")) AND TimeGenerated>NOW-4HOURS AND (Computer="AzureMktg01" OR Computer="AzureMktg02" OR Computer="AzureMktg03") | Measure Avg(CounterValue) by CounterName
```

Dadurch erhalten Sie eine nützliche, kompakte Ansicht von einigen KPIs in Ihrer Umgebung.

!["search avg grouping" ("Avg"-Gruppierung durchsuchen)](./media/log-analytics-log-searches/oms-search-avg04.png)

Sie können die Suchabfrage problemlos in einem Dashboard verwenden. Sie können zum Beispiel die Suchabfrage speichern und ein Dashboard daraus erstellen mit der Bezeichnung *Web Farm KPIs*. Weitere Informationen zur Verwendung von Dashboards finden Sie unter [Erstellen eines benutzerdefinierten Dashboards in Log Analytics](log-analytics-dashboards.md).

!["search avg dashboard" ("Avg"-Dashboard durchsuchen)](./media/log-analytics-log-searches/oms-search-avg05.png)

### <a name="use-the-sum-function-with-the-measure-command"></a>Verwenden der "Sum"-Funktion mit dem "Measure"-Befehl
Die "Sum"-Funktion ähnelt anderen Funktionen des "Measure"-Befehls. Ein Beispiel zur Verwendung der "Sum"-Funktion finden Sie auf [W3C IIS Logs Search in Microsoft Azure Operational Insights](http://blogs.msdn.com/b/dmuscett/archive/2014/09/20/w3c-iis-logs-search-in-system-center-advisor-limited-preview.aspx)("W3C-IIS-Protokollsuche in Microsoft Azure Operational Insights", auf Englisch).

Sie können „Max()“ und „Min()“ mit Zahlen, Datums-/Uhrzeitangaben und Textzeichenfolgen verwenden. Mit Textzeichenfolgen sind diese alphabetisch sortiert, und Sie erhalten die erste und letzte.

Sie können "Sum()" ausschließlich mit numerischen Feldern verwenden. Dies gilt auch für "Avg()".

### <a name="use-the-percentile-function-with-the-measure-command"></a>Verwenden der „percentile“-Funktion mit dem „Measure“-Befehl
Die Funktion „percentile“ ähnelt „Avg()“ und „Sum()“ darin, dass Sie sie nur für numerische Felder verwenden können. Sie können ein beliebiges Perzentil zwischen 1 und 99 in einem numerischen Feld verwenden. Außerdem können Sie sowohl den Befehl **percentile** als auch den Befehl **pct** verwenden. Hier sind einige Beispiele angegeben:  

```
Type:Perf CounterName:"DiskTransers/sec" |measure percentile95(CurrentValue) by Computer
```
```
Type:Perf ObjectName=LogicalDisk CounterName="Current Disk Queue Length" Computer="MyComputerName" | measure pct65(CurrentValue) by InstanceName
```

## <a name="use-the-where-command"></a>Verwenden des "Where"-Befehls
Der Befehl funktioniert wie ein Filter, kann aber in der Pipeline angewendet werden, um aggregierte Ergebnisse weiter zu filtern, die von einem "Measure"-Befehl erzeugt wurden – im Gegensatz zu reinen Ergebnisse, die am Beginn einer Abfrage gefiltert werden.

Beispiel:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer
```

Sie können einen weiteren senkrechten Strich („|“) und den „Where“-Befehl hinzufügen, um nur Computer abzurufen, deren durchschnittliche CPU-Nutzung über 80% liegt. Siehe folgendes Beispiel:

```
Type=Perf  CounterName="% Processor Time"  InstanceName="_Total" | Measure Avg(CounterValue) as AVGCPU by Computer | Where AVGCPU>80
```

Wenn Sie mit Microsoft System Center – Operations Manager vertraut sind, können Sie sich den "Where"-Befehl in der Management Pack-Terminologie denken. Wenn das Beispiel eine Regel wäre, wäre der erste Teil der Abfrage die Datenquelle und der "Where"-Befehl wäre die Bedingungserkennung.

Sie können die Abfrage als Kachel in **Mein Dashboard**verwenden, als eine Art Überwachung, wenn die CPUs des Computers stark ausgelastet sind. Weitere Informationen zu Dashboards finden Sie unter [Erstellen eines benutzerdefinierten Dashboards in Log Analytics](log-analytics-dashboards.md). Sie können Dashboards auch mithilfe der mobilen App erstellen und verwenden. Weitere Informationen finden Sie im Artikel zur [mobilen OMS-App ](http://www.windowsphone.com/en-us/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865). In den unteren beiden Kacheln der folgenden Abbildung bekommen Sie den Monitor als Liste und als Zahl angezeigt. Im Wesentlichen sollte die Zahl immer 0 (null) sein und die Liste leer. Andernfalls könnte es eine Warnungsbedingung geben. Bei Bedarf können sie  die Abfrage auch nutzen, um einen Blick auf überlastete Computer zu werden.

!["mobile dashboard" ("mobiles Dashboard")](./media/log-analytics-log-searches/oms-search-mobile.png)

## <a name="use-the-in-operator"></a>Verwenden des Operators „IN“
Mit dem Operator *IN* zusammen mit *NOT IN* können Sie so genannte „Untersuchen“ (Subsearches) verwenden. Dies sind Suchen, die eine weitere Suche als Argument enthalten. Sie sind in einer anderen *primären* oder *äußeren* Suche in geschweiften Klammern ({}) angeordnet. Das Ergebnis einer Untersuche, wobei es sich häufig um eine Liste mit unterschiedlichen Ergebnissen handelt, wird dann als Argument der primären Suche verwendet.

Sie können Untersuchen verwenden, um Übereinstimmungen für Teilmengen der Daten zu erzielen, die Sie nicht direkt in einem Suchausdruck beschreiben können, aber die aus einer Suche generiert werden können. Wenn Sie beispielsweise daran interessiert sind, mit einer Suche alle Ereignisse von *Computern mit fehlenden Sicherheitsupdates* zu finden, gehen Sie wie folgt vor: Entwerfen Sie eine Untersuche, mit der zuerst identifiziert wird, dass *Computer mit fehlenden Sicherheitsupdates* vorhanden sind, bevor nach Ereignissen gesucht wird, die zu diesen Hosts gehören.

Sie können den Fall *Computer, auf denen derzeit erforderliche Sicherheitsupdates fehlen* also mit der folgenden Abfrage ausdrücken:

```
Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer
```    

![Beispiel für Suche mit IN](./media/log-analytics-log-searches/oms-search-in01-revised.png)

Nachdem Sie die Liste ermittelt haben, können Sie die Suche als innere Suche verwenden, um die Liste mit den Computern in eine äußere (primäre) Suche einzufügen, mit der nach Ereignissen für diese Computer gesucht wird. Hierzu setzen Sie die innere Suche in Klammern und fügen deren Ergebnisse als mögliche Werte für einen Filter oder ein Feld in der äußeren Suche ein, indem Sie den Operator IN verwenden. Die Abfrage sieht dann in etwa wie folgt aus:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer}
```
![Beispiel für Suche mit IN](./media/log-analytics-log-searches/oms-search-in02-revised.png)

Beachten Sie auch den Zeitfilter, der für die innere Suche verwendet wird, da von System Update Assessment alle 24 Stunden eine Momentaufnahme aller Computer erstellt wird. Sie können die innere Abfrage einfacher und präziser gestalten, indem Sie nur nach einem Tag suchen. Für die äußere Suche wird dagegen die Zeitauswahl der Benutzeroberfläche verwendet, und es werden Ereignisse der letzten sieben Tage abgerufen. Weitere Informationen zu Zeitoperatoren finden Sie unter [Boolesche Operatoren](#boolean-operators) .

Da Sie eigentlich nur die Ergebnisse der inneren Suche als Filterwert für die äußere Suche nutzen, können Sie auf die äußere Suche weiterhin Befehle anwenden. Beispielsweise können Sie die obigen Ereignisse weiterhin mit einem anderen „Measure“-Befehl gruppieren:

```
Type=Event Computer IN {Type:Update UpdateState=Needed Optional=false Classification="Security Updates" TimeGenerated>NOW-24HOURS | measure count() by Computer} | measure count() by Source
```

![Beispiel für Suche mit IN](./media/log-analytics-log-searches/oms-search-in03-revised.png)

Im Allgemeinen möchten Sie, dass Ihre innere Abfrage schnell ausgeführt wird, da Log Analytics über dienstseitige Timeouts dafür verfügt, und außerdem soll eine kleine Menge an Ergebnissen zurückgegeben werden. Wenn die innere Abfrage mehr Ergebnisse zurückgibt, wird die Ergebnisliste abgeschnitten. Dies kann unter Umständen dazu führen, dass die äußere Suche fehlerhafte Ergebnisse zurückgibt.

Eine weitere Regel lautet, dass die innere Suche derzeit *aggregierte* Ergebnisse liefern muss. Anders ausgedrückt: Sie muss einen *Measure*-Befehl enthalten. Momentan ist es nicht möglich, Ergebnisse in Rohform in eine äußere Suche einzufügen.

Außerdem darf nur ein IN-Operator vorhanden sein, und es muss der letzte Filter in der Abfrage sein. Mehrere IN-Operatoren können nicht mit OR verbunden werden, sodass die Ausführung mehrerer Untersuchen praktisch verhindert wird. Der wichtige Punkt hierbei ist, dass für jede äußere Suche nur eine Untersuche bzw. innere Suche möglich ist.

Auch mit diesen Einschränkungen ermöglicht IN viele Arten von korrelierten Suchen und die Definition von Elementen, die Gruppen ähneln, z.B. Computer, Benutzer oder Dateien – was auch immer die Felder in Ihren Daten enthalten. Weitere Beispiele:

**Alle fehlenden Updates auf Computern, auf denen die Einstellung „Automatisches Update“ deaktiviert ist**

```
Type=Update UpdateState=Needed Optional=false Computer IN {Type=UpdateSummary WindowsUpdateSetting=Manual | measure count() by Computer} | measure count() by KBID
```

**Alle Fehlerereignisse auf Computern, auf denen SQL Server ausgeführt wird (Ausführung von SQL Assessment)**

```
Type=Event EventLevelName=error Computer IN {Type=SQLAssessmentRecommendation | measure count() by Computer}
```

**Alle Sicherheitsereignisse auf Computern, bei denen es sich um Domänencontroller handelt (Ausführung von AD Assessment)**

```
Type=SecurityEvent Computer IN { Type=ADAssessmentRecommendation | measure count() by Computer }
```

**Welche anderen Konten haben sich an den Computern angemeldet, auf denen eine Anmeldung mit dem Konto BACONLAND\jochan erfolgt ist?**

```
Type=SecurityEvent EventID=4624   Account!="BACONLAND\\jochan" Computer IN { Type=SecurityEvent EventID=4624   Account="BACONLAND\\jochan" | measure count() by Computer } | measure count() by Account
```

## <a name="use-the-distinct-command"></a>Verwenden des Befehls „DISTINCT“
Wie der Name schon vermuten lässt, wird mit diesem Befehl eine Liste mit unterschiedlichen Werten für ein Feld bereitgestellt. Er ist erstaunlich einfach, aber gleichzeitig sehr nützlich. Das gleiche Ergebnis lässt sich wie unten gezeigt mit dem Befehl „measure count()“ erzielen.

```
Type=Event | Measure count() by Computer
```

![Beispiel für den Suchbefehl DISTINCT](./media/log-analytics-log-searches/oms-search-distinct01-revised.png)

Falls Sie aber nur an einer Liste mit unterschiedlichen Werten und nicht an der Anzahl von Dokumenten mit diesen Werten interessiert sind, liefert DISTINCT eine Ausgabe, die aufgeräumter und leichter zu lesen ist und eine kürzere Syntax hat. Dies ist unten dargestellt.

```
Type=Event | Distinct Computer
```
![Beispiel für den Suchbefehl DISTINCT](./media/log-analytics-log-searches/oms-search-distinct02-revised.png)

## <a name="use-the-countdistinct-function-with-the-measure-command"></a>Verwenden der Funktion „countdistinct“ mit dem „Measure“-Befehl
Mit der Funktion „countdistinct“ wird die Anzahl von unterschiedlichen Werten in einer Gruppe gezählt. Sie kann beispielsweise verwendet werden, um die Anzahl von eindeutigen Computern zu zählen, die für jeden Typ Meldungen durchführen:

```
* | measure countdistinct(Computer) by Type
```

![OMS-countdistinct](./media/log-analytics-log-searches/oms-countdistinct.png)

## <a name="use-the-measure-interval-command"></a>Verwenden des Befehls „measure interval“
Mit der Erfassung von Leistungsdaten nahezu in Echtzeit können Sie alle Leistungsindikatoren in Log Analytics erfassen und visualisieren. Mit der einfachen Eingabe der Abfrage **Type:Perf** werden Tausende von Metrikendiagrammen zurückgegeben, und zwar basierend auf der Anzahl von Indikatoren und Servern in Ihrer Log Analytics-Umgebung. Mit der bedarfsgesteuerten Metrikaggregation können Sie sich die allgemeinen Gesamtmetriken Ihrer Umgebung ansehen und dann auf genauere Daten zugreifen, wenn dies erforderlich ist.

Angenommen, Sie möchten herausfinden, welcher durchschnittliche CPU-Wert für Ihre Computer gilt. Unter Umständen ist es nicht hilfreich, wenn Sie sich den durchschnittlichen CPU-Wert für jeden Computer ansehen, da die Ergebnisse ggf. geglättet werden. Wenn Sie weitere Details wünschen, können Sie das Ergebnis in kleineren Zeitfenstern aggregieren und sich eine Zeitreihe über verschiedene Dimensionen hinweg ansehen. Beispielsweise können Sie wie folgt den stündlichen Mittelwert der CPU-Nutzung für alle Computer ermitteln:

```
Type:Perf CounterName="% Processor Time" InstanceName="_Total" | measure avg(CounterValue) by Computer Interval 1HOUR
```

![measure average interval](./media/log-analytics-log-searches/oms-measure-avg-interval.png)

Standardmäßig werden diese Ergebnisse in einem interaktiven Liniendiagramm mit mehreren Reihen angezeigt.  Dieses Diagramm unterstützt die Reihenumschaltung (mit Neuskalierung der y-Achse), das Zooming und das Zeigen mit der Maus.  Die Option für die Tabellenanzeige ist weiterhin verfügbar, um bei Bedarf die unformatierten Daten anzuzeigen.

Sie können auch nach anderen Feldern gruppieren. In diesem Beispiel sehe ich mir alle Prozentindikatoren für einen bestimmten Computer an und möchte wissen, wie die stündlichen 70. Perzentile für jeden Indikator lauten:

```
Type:Perf Computer=beefpatty4 CounterName=%* InstanceName=_Total | measure percentile70(CounterValue) by CounterName Interval 1HOUR
```
Dabei ist zu beachten ist, dass diese Abfragen nicht auf Leistungsindikatoren beschränkt sind. Sie können sie auf eine beliebige Metrik anwenden. In diesem Beispiel sehe ich mir W3C-IIS-Protokolle an. Ich möchte herausfinden, wie lange die Verarbeitung jeder Anforderung für ein Intervall von fünf Minuten maximal dauert:

```
Type:W3CIISLog | measure max(TimeTaken) by csMethod Interval 5MINUTES
```

### <a name="use-multiple-aggregates-in-one-query"></a>Verwenden mehrerer Aggregate in einer Abfrage
Sie können in einem Measure-Befehl mehrere Aggregatklauseln angeben.  Jede kann einzeln mit einem Alias versehen werden.  Wenn kein Alias vergeben wird, wird als Feldname die verwendete Aggregatfunktion genutzt (also z.B. „avg(CounterValue)“ für „avg(CounterValue)“).

 ```
Type=WireData | measure avg(ReceivedBytes), avg(SentBytes) by Direction interval 1hour
```
![OMS-multiaggregates1](./media/log-analytics-log-searches/oms-multiaggregates1.png)

Hier ist ein weiteres Beispiel angegeben:

 ```
* | measure countdistinct(Computer) as Computers, count() as TotalRecords by Type
```


## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu Protokollsuchen finden Sie hier:

* Verwenden Sie [benutzerdefinierte Felder in Log Analytics](log-analytics-custom-fields.md) , um Protokollsuchen zu erweitern.
* Lesen Sie sich die [Referenz zur Log Analytics-Suche](log-analytics-search-reference.md) durch, um sich über alle Suchfelder und Facetten zu informieren, die in Log Analytics verfügbar sind.

