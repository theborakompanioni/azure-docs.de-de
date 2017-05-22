---
title: Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool | Microsoft Azure
description: Aktivieren Sie das automatische Skalieren in einem Cloudpool, um die Anzahl von Computeknoten im Pool dynamisch anzupassen.
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: c624cdfc-c5f2-4d13-a7d7-ae080833b779
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 05/05/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 85a2fac77cf5beae6debad044df169301d43f4ca
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="create-an-automatic-scaling-formula-for-scaling-compute-nodes-in-a-batch-pool"></a>Erstellen einer Formel für die automatische Skalierung von Computeknoten in einem Batch-Pool

Durch automatisches Skalieren kann der Azure Batch-Dienst Computeknoten in einem Pool auf der Grundlage der von Ihnen definierten Parameter dynamisch hinzufügen oder entfernen. Sie können sowohl Zeit als auch Geld sparen, indem Sie automatisch die von Ihrer Anwendung verwendete Anzahl von Computeknoten anpassen. Durch die automatische Skalierung können Sie Knoten hinzufügen, wenn sich die Taskanforderungen Ihres Auftrags erhöhen, bzw. Knoten entfernen, wenn der Bedarf sinkt.

Sie können die automatische Skalierung für einen Pool von Computeknoten aktivieren, indem Sie ihn einer von Ihnen definierten *Formel für die automatische Skalierung* zuordnen. Beispielsweise können Sie in Batch .NET die Methode [PoolOperations.EnableAutoScale][net_enableautoscale] verwenden. Anhand der Formel für die automatische Skalierung ermittelt der Batch-Dienst dann die Anzahl von Computeknoten, die zum Ausführen Ihrer Workload erforderlich sind. Batch reagiert auf Dienstmetrikdaten, die in regelmäßigen Abständen gesammelt werden. Anhand dieser Metrikdaten wird die Anzahl von Computeknoten im Pool basierend auf Ihrer Formel und einem konfigurierbaren Intervall von Batch angepasst.

Sie können die automatische Skalierung beim Erstellen eines Pools oder später für einen bereits vorhandenen Pool aktivieren. Sie können auch eine vorhandene Formel in einem Pool ändern, für den die „automatische Skalierung“ aktiviert ist. Batch gibt Ihnen die Möglichkeit, Formeln vor dem Zuweisen zu Pools auszuwerten und den Status automatischer Skalierungsausführungen zu überwachen.

In diesem Artikel werden die verschiedenen Entitäten erläutert, aus denen sich Ihre Formeln für die automatische Skalierung zusammensetzen. Dazu zählen Variablen, Operatoren, Operationen und Funktionen. Sie lernen, wie Sie verschiedene Metriken zu Compute-Ressourcen und Aufgaben in Batch abrufen. Sie können diese Metriken verwenden, um die Knotenanzahl des Pools basierend auf Ressourcenverbrauch und Aufgabenstatus automatisch anzupassen. Anschließend erfahren Sie, wie Sie mit der REST-API und der .NET-API von Batch eine Formel erstellen und die automatische Skalierung für einen Pool aktivieren.  Zum Abschluss werden nun verschiedene Beispielformeln gezeigt.

> [!IMPORTANT]
> Jedes Azure Batch-Konto ist auf eine bestimmte Anzahl von Kernen (und damit auf eine maximale Anzahl von Computeknoten) beschränkt, die für die Verarbeitung verwendet werden können. Der Batch-Dienst erstellt maximal so viele neue Knoten, wie durch den Kerngrenzwert vorgegeben wird. Die durch eine Formel für die automatischen Skalierung angegebene Zielanzahl von Computeknoten wird vom Batch-Dienst möglicherweise nicht erreicht. Unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) finden Sie Informationen zum Anzeigen und Erhöhen Ihrer Kontokontingente.
>
>

## <a name="automatic-scaling-formulas"></a>Formeln für die automatische Skalierung
Eine Formel für die automatische Skalierung ist ein von Ihnen definierter Zeichenfolgenwert, der mindestens eine Anweisung enthält. Die Formel für die automatische Skalierung wird dem Element [autoScaleFormula][rest_autoscaleformula] (Batch REST) oder der Eigenschaft [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula] (Batch .NET) eines Pools zugewiesen. Der Batch-Dienst verwendet Ihre Formel, um für das nächste Verarbeitungsintervall die Zielanzahl der Computeknoten im Pool zu ermitteln. Die Formelzeichenfolge darf eine Größe von 8 KB nicht überschreiten und kann bis zu 100 durch Semikolons getrennte Anweisungen sowie Zeilenumbrüche und Kommentare enthalten.

Sie können sich Formeln für die automatische Skalierung als eine Batch-„Sprache“ für die automatische Skalierung vorstellen. Formelanweisungen sind frei definierte Ausdrücke, die sowohl dienstdefinierte Variablen (vom Batch-Dienst definiert) als auch benutzerdefinierte Variablen (von Ihnen selbst definiert) enthalten können. Mithilfe von mitgelieferten Typen, Operatoren und Funktionen können sie für diese Werte eine Vielzahl von Operationen ausführen. Eine Anweisung kann beispielsweise wie folgt aussehen:

```
$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);
```

Formeln enthalten im Allgemeinen mehrere Anweisungen, die Operationen für Werte durchführen, die in vorherigen Anweisungen abgerufen wurden. So rufen wir beispielsweise zuerst einen Wert für `variable1` ab und übergeben ihn dann an eine Funktion, um `variable2` aufzufüllen:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Schließen Sie diese Anweisungen in Ihre Formel für die automatische Skalierung ein, um die Anzahl von Computeknoten zu erhalten, auf die der Pool skaliert werden soll (also die **Zielanzahl** von **dedizierten Knoten**). Diese Zahl kann höher, niedriger oder identisch mit der aktuellen Anzahl von Knoten im Pool sein. Batch wertet die Formel für die automatische Skalierung des Pools in einem bestimmten Intervall aus. ([Intervalle für die automatische Skalierung](#automatic-scaling-interval) werden weiter unten erläutert.) Batch passt die Zielanzahl von Knoten im Pool an die Zahl an, die durch die Formel für die automatische Skalierung zum Zeitpunkt der Auswertung angegeben wird.

### <a name="sample-autoscale-formula"></a>Beispielformel für die automatische Skalierung

Hier sehen Sie ein Beispiel für eine Formel für die automatische Skalierung, die für die meisten Szenarien angepasst werden kann. Die Variablen `startingNumberOfVMs` und `maxNumberofVMs` in der Beispielformel können an Ihre Anforderungen angepasst werden.

```
startingNumberOfVMs = 1;
maxNumberofVMs = 25;
pendingTaskSamplePercent = $PendingTasks.GetSamplePercent(180 * TimeInterval_Second);
pendingTaskSamples = pendingTaskSamplePercent < 70 ? startingNumberOfVMs : avg($PendingTasks.GetSample(180 * TimeInterval_Second));
$TargetDedicated=min(maxNumberofVMs, pendingTaskSamples);
```

Mit dieser Formel für die automatische Skalierung wird der Pool zunächst mit einem einzigen virtuellen Computer erstellt. Die Metrik „$PendingTasks“ definiert die Anzahl von Aufgaben, die ausgeführt werden oder sich in der Warteschlange befinden. Die Formel findet die durchschnittliche Anzahl ausstehender Aufgaben in den letzten 180 Sekunden und legt „TargetDedicated“ auf den entsprechenden Wert fest. Durch die Formel wird sichergestellt, dass „TargetDedicated“ nie über 25 VMs hinausgeht. Wenn neue Aufgaben gesendet werden, wächst der Pool automatisch an. Werden Aufgaben abgeschlossen, werden VMs nacheinander frei, und der Pool wird durch die Formel für die automatische Skalierung verkleinert.

## <a name="variables"></a>Variables
Sie können in Ihrer Formel für die automatische Skalierung sowohl **dienstdefinierte** als auch **benutzerdefinierte** Variablen verwenden. Die vom Dienst definierten Variablen sind in den Batch-Dienst integriert – einige mit Lese-/Schreibzugriff, andere schreibgeschützt. Benutzerdefinierte Variablen sind Variablen, die *Sie* definieren. In der im vorherigen Abschnitt gezeigten Beispielformel sind `$TargetDedicated` und `$PendingTasks` vom Dienst definierte Variablen. Die Variablen `startingNumberOfVMs` und `maxNumberofVMs` sind benutzerdefinierte Variablen.

> [!NOTE]
> Vom Dienst definierten Variablen wird immer ein Dollarzeichen ($) vorangestellt. Für benutzerdefinierte Variablen ist das Dollarzeichen optional.
>
>

Die folgenden Tabellen enthalten sowohl Variablen mit Lese-/Schreibzugriff als auch schreibgeschützte Variablen, die vom Batch-Dienst definiert werden.

Sie können diese vom Dienst definierten Variablen **abrufen** und **festlegen**, um die Anzahl von Computeknoten in einem Pool zu verwalten:

| Vom Dienst definierte Variablen mit Lese-/Schreibzugriff | Beschreibung |
| --- | --- |
| $TargetDedicated |Die **Zielanzahl** von **dedizierten Computeknoten** für den Pool entspricht der Anzahl von Computeknoten, auf die der Pool skaliert werden soll. Es handelt sich um einen „Zielwert“, da ein Pool die Zielanzahl von Knoten möglicherweise nicht erreicht. Wenn die Zielanzahl von Knoten beispielsweise durch eine nachfolgende Auswertung der automatischen Skalierung geändert wird, bevor der Pool das ursprüngliche Ziel erreicht hat, kann die Zielanzahl von Knoten möglicherweise nicht vom Pool erreicht werden. Es kann auch vorkommen, wenn ein Batch-Konto-Knoten oder Kern-Kontingent erreicht ist, bevor die vorgegebene Anzahl von Knoten erreicht wird. |
| $NodeDeallocationOption |Dieser Vorgang wird ausgeführt, wenn Computeknoten aus einem Pool entfernt werden. Mögliche Werte:<ul><li>**requeue**: Aufgaben sofort beenden und wieder in die Auftragswarteschlange einfügen, damit sie neu geplant werden können.<li>**terminate**: Aufgaben sofort beenden und aus der Auftragswarteschlange entfernen.<li>**taskcompletion**: Auf derzeit ausgeführte Aufgaben warten und den Knoten dann aus dem Pool entfernen.<li>**retaineddata**: Warten, bis alle lokal vorgehaltenen Aufgabendaten des Pools gelöscht wurden, bevor der Knoten aus dem Pool entfernt wird.</ul> |

Sie können die Werte dieser vom Dienst definierten Variablen mit **get** abrufen, um Anpassungen basierend auf den Metriken des Batch-Diensts vorzunehmen:

| Vom Dienst definierte schreibgeschützte Variablen | Beschreibung |
| --- | --- |
| $CPUPercent |Die durchschnittliche prozentuale CPU-Auslastung |
| $WallClockSeconds |Die Anzahl der verbrauchten Sekunden |
| $MemoryBytes |Die durchschnittliche Anzahl genutzter Megabyte |
| $DiskBytes |Die durchschnittliche Anzahl der auf den lokalen Datenträgern genutzten Gigabyte |
| $DiskReadBytes |Die Anzahl der gelesenen Byte |
| $DiskWriteBytes |Die Anzahl der geschriebenen Byte |
| $DiskReadOps |Die Anzahl der ausgeführten Datenträger-Lesevorgänge |
| $DiskWriteOps |Die Anzahl der ausgeführten Datenträger-Schreibvorgänge |
| $NetworkInBytes |Die Anzahl der eingehenden Byte |
| $NetworkOutBytes |Die Anzahl der ausgehenden Byte |
| $SampleNodeCount |Die Anzahl der Computeknoten |
| $ActiveTasks |Die Anzahl der Aufgaben, die sich in einem aktiven Zustand befinden |
| $RunningTasks |Die Anzahl der Aufgaben, die sich in einem Ausführungszustand befinden |
| $PendingTasks |Die Summe aus $ActiveTasks und $RunningTasks. |
| $SucceededTasks |Die Anzahl der Aufgaben, die erfolgreich abgeschlossen wurden |
| $FailedTasks |Die Anzahl der Aufgaben, bei denen Fehler aufgetreten sind |
| $CurrentDedicated |Die aktuelle Anzahl der zugewiesenen Computeknoten |
| $PreemptedNodeCount | Die Anzahl der Knoten im Pool, die sich im Zustand „Vorzeitig entfernt“ befinden. |

> [!TIP]
> Die obigen, vom Dienst definierten schreibgeschützten Variablen sind *Objekte* , die verschiedene Methoden für den Zugriff auf die zum jeweiligen Objekt gehörigen Daten bereitstellen. Weitere Informationen finden Sie unten unter [Erfassen von Stichprobendaten](#getsampledata).
>
>

## <a name="types"></a>Typen
Folgende **Typen** werden in Formeln unterstützt.

* double
* doubleVec
* doubleVecList
* string
* timestamp – „timestamp“ ist eine Verbundstruktur, die folgende Member enthält:

  * year
  * Monat (1-12)
  * Tag (1-31)
  * weekday (als Zahl, z. B. 1 für Montag)
  * hour (im 24-Stunden-Format, z. B. 13 für 13 Uhr)
  * Minute (00-59)
  * Sekunde (00-59)
* timeInterval

  * TimeInterval_Zero
  * TimeInterval_100ns
  * TimeInterval_Microsecond
  * TimeInterval_Millisecond
  * TimeInterval_Second
  * TimeInterval_Minute
  * TimeInterval_Hour
  * TimeInterval_Day
  * TimeInterval_Week
  * TimeInterval_Year

## <a name="operations"></a>Vorgänge
Für die oben aufgeführten Typen sind folgende **Vorgänge** zulässig.

| Vorgang | Unterstützte Operatoren | Ergebnistyp |
| --- | --- | --- |
| double *Operator* double |+, -, *, / |double |
| double *Operator* timeinterval |* |timeInterval |
| doubleVec *Operator* double |+, -, *, / |doubleVec |
| doubleVec *Operator* doubleVec |+, -, *, / |doubleVec |
| timeinterval *Operator* double |*, / |timeInterval |
| timeinterval *Operator* timeinterval |+, - |timeInterval |
| timeinterval *Operator* timestamp |+ |timestamp |
| timestamp *Operator* timeinterval |+ |timestamp |
| timestamp *Operator* timestamp |- |timeInterval |
| *Operator*double |-, ! |double |
| *Operator*timeInterval |- |timeInterval |
| double *Operator* double |<, <=, ==, >=, >, != |double |
| string *Operator* string |<, <=, ==, >=, >, != |double |
| timestamp *Operator* timestamp |<, <=, ==, >=, >, != |double |
| timeinterval *Operator* timeinterval |<, <=, ==, >=, >, != |double |
| double *Operator* double |&&, &#124;&#124; |double |

Beim Testen von „double“ mit einem ternären Operator (`double ? statement1 : statement2`) sind Werte ungleich Null **true** und Nullwerte **false**.

## <a name="functions"></a>Functions
Zum Definieren einer Formel für die automatische Skalierung stehen folgende vordefinierte **Funktionen** zur Verfügung.

| Funktion | Rückgabetyp | Beschreibung |
| --- | --- | --- |
| avg(doubleVecList) |double |Der Durchschnittswert aller Werte in der doubleVecList wird zurückgegeben. |
| len(doubleVecList) |double |Die Länge des Vektors, der aus der doubleVecList erstellt wurde, wird zurückgegeben. |
| lg(double) |double |Gibt für den double-Wert den Logarithmus zur Basis 2 zurück. |
| lg(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenbezogenen Logarithmus zur Basis 2 zurück. Ein vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen. |
| ln(double) |double |Gibt für den double-Wert den natürlichen Logarithmus zurück. |
| ln(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenbezogenen Logarithmus zur Basis 2 zurück. Ein vec(double) muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen. |
| log(double) |double |Gibt für den double-Wert den Logarithmus zur Basis 10 zurück. |
| log(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenbezogenen Logarithmus zur Basis 10 zurück. Ein vec(double) muss explizit für den einzelnen double-Parameter übergeben werden. Andernfalls wird von der double log (double)-Version ausgegangen. |
| max(doubleVecList) |double |Der maximale Wert in der doubleVecList wird zurückgegeben. |
| min(doubleVecList) |double |Der minimale Wert in der doubleVecList wird zurückgegeben. |
| norm(doubleVecList) |double |Die Zweiernorm des Vektors, der aus der doubleVecList erstellt wurde, wird zurückgegeben. |
| percentile(doubleVec v, double p) |double |Das Perzentil-Element des Vektors v wird zurückgegeben. |
| rand() |double |Ein Zufallswert zwischen 0,0 und 1,0 wird zurückgegeben. |
| range(doubleVecList) |double |Der Unterschied zwischen dem Minimal- und Maximalwert in doubleVecList wird zurückgegeben. |
| std(doubleVecList) |double |Die Stichproben-Standardabweichung der Werte in der doubleVecList wird zurückgegeben. |
| stop() | |Beendet die Auswertung des Ausdrucks für die automatische Skalierung. |
| sum(doubleVecList) |double |Die Summe aller Komponenten von doubleVecList wird zurückgegeben. |
| time(string dateTime="") |timestamp |Es werden entweder der Zeitstempel der aktuellen Zeit zurückgegeben, wenn keine Parameter übergeben werden, oder andernfalls der Zeitstempel der DateTime-Zeichenfolge, wenn diese übergeben wird. Unterstützte DateTime-Formate sind W3C-DTF und RFC 1123. |
| val(doubleVec v, double i) |double |Der Wert des Elements an Position i im Vektor v mit einem Anfangsindex von 0 wird zurückgegeben. |

Einige der in der obigen Tabelle beschriebenen Funktionen akzeptieren eine Liste als Argument. Bei der durch Trennzeichen getrennten Liste handelt es sich um eine beliebige Kombination aus *double* und *doubleVec*. Beispiel:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Der *doubleVecList*-Wert wird vor der Auswertung in einen einzelnen *doubleVec* konvertiert. Zum Beispiel hat bei `v = [1,2,3]` das Aufrufen von `avg(v)` den gleichen Effekt wie das Aufrufen von `avg(1,2,3)`. Das Aufrufen von `avg(v, 7)` entspricht dem Aufrufen von `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Erfassen von Stichprobendaten
Die Formeln für die automatische Skalierung greifen auf Metrikdaten (Stichproben) zurück, die vom Batch-Dienst bereitgestellt werden. Eine Formel vergrößert oder verkleinert den Pool basierend auf den Werten, die sie vom Dienst erhält. Die obigen, vom Dienst definierten Variablen sind Objekte, die verschiedene Methoden für den Zugriff auf die zum jeweiligen Objekt gehörigen Daten bereitstellen. Der folgende Ausdruck zeigt z. B. eine Anforderung zum Abrufen der letzten fünf Minuten der CPU-Auslastung:

```
$CPUPercent.GetSample(TimeInterval_Minute * 5)
```

| Methode | Beschreibung |
| --- | --- |
| GetSample() |Die `GetSample()`-Methode gibt einen Vektor aus Stichprobenwerten zurück.<br/><br/>Eine Stichprobe enthält Metrikdaten, die innerhalb von 30 Sekunden erfasst wurden. Dies bedeutet, dass alle 30 Sekunden eine Stichprobe genommen wird. Wie nachstehend erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Daher stehen möglicherweise nicht alle Stichproben für einen bestimmten Zeitraum für die Bewertung durch eine Formel zur Verfügung.<ul><li>`doubleVec GetSample(double count)`<br/>Gibt die Anzahl von Stichproben an, die aus den letzten erfassten Stichproben abgerufen werden soll.<br/><br/>`GetSample(1)` gibt die neueste verfügbare Stichprobe zurück. Für Metriken wie `$CPUPercent`sollte diese Methode allerdings nicht verwendet werden, da unmöglich feststellbar ist, *wann* die Stichprobe erfasst wurde. Sie kann aktuell oder aber, aufgrund von Systemproblemen, auch wesentlich älter sein. In solchen Fällen ist es besser, wie unten gezeigt ein Zeitintervall zu verwenden.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Gibt einen Zeitraum für die Erfassung von Stichprobendaten an. Optional gibt diese Methode auch den Prozentsatz der Stichproben an, die im angeforderten Zeitraum verfügbar sein müssen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)` gibt 20 Stichproben zurück, wenn alle Stichproben der letzten zehn Minuten im CPUPercent-Verlauf vorhanden sind. Wenn jedoch die letzte Minute des Verlaufs nicht verfügbar ist, werden nur 18 Stichproben zurückgegeben. In diesem Fall:<br/><br/>Für `$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)` tritt ein Fehler auf, da nur 90 Prozent der Stichproben verfügbar sind.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)` wird erfolgreich durchgeführt.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Gibt einen Zeitrahmen für die Datenerfassung mit einer Start- und einer Endzeit an.<br/><br/>Wie bereits erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Diese Verzögerung muss bei Verwendung der `GetSample` -Methode berücksichtigt werden. Weitere Informationen finden Sie bei `GetSamplePercent` weiter unten. |
| GetSamplePeriod() |Gibt den Zeitraum zurück, in dem die Stichproben aus einem alten Stichproben-Dataset gesammelt wurden. |
| Count() |Liefert die Gesamtzahl der Stichprobenwerte im Metrikverlauf zurück. |
| HistoryBeginTime() |Gibt den Zeitstempel des ältesten verfügbaren Stichprobenwerts für die Metrik zurück. |
| GetSamplePercent() |Gibt den Prozentsatz an Stichprobenwerten zurück, die für ein bestimmtes Intervall verfügbar sind. Beispiel:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Da die `GetSample`-Methode einen Fehler erzeugt, wenn der Prozentsatz der zurückgegebenen Stichproben kleiner als der angegebene `samplePercent`-Wert ist, können Sie vorab mithilfe der `GetSamplePercent`-Methode eine Prüfung vornehmen. Wenn nicht genügend Beispiele vorhanden sind, können Sie anschließend eine andere Aktion ausführen, , ohne die Auswertung der automatischen Skalierung zu unterbrechen. |

### <a name="samples-sample-percentage-and-the-getsample-method"></a>Stichproben, Prozentsatz für die Stichprobe und die *GetSample()* -Methode
Das Abrufen von Metrikdaten zu Aufgaben und Ressourcen sowie das anschließende Anpassen der Poolgröße basierend auf diesen Daten ist die Kernfunktion einer Formel für die automatische Skalierung. Daher ist es wichtig, sich damit vertraut zu machen, wie Formeln für die automatische Skalierung mit Metrikdaten bzw. „Stichproben“ interagieren.

**Beispiele**

Der Batch-Dienst nimmt regelmäßig *Stichproben* von Task- und Ressourcenmetriken und stellt sie Ihren Formeln für die automatische Skalierung zur Verfügung. Diese Stichproben werden alle 30 Sekunden vom Batch-Dienst aufgezeichnet. Allerdings kommt es durch Latenz meist zu einer Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichproben und dem Zeitpunkt, zu dem sie Ihren Formeln für die automatische Skalierung zur Verfügung gestellt (und von diesen gelesen) werden können. Darüber hinaus werden Stichproben gelegentlich aufgrund verschiedener Faktoren wie Netzwerk- oder anderer Infrastrukturprobleme für ein bestimmtes Intervall nicht erfasst. Dies führt zu „fehlenden“ Stichproben.

**Prozentsatz für die Stichprobe**

Beim Übergeben eines `samplePercent`-Werts an die `GetSample()`-Methode oder Aufrufen der `GetSamplePercent()`-Methode bezieht sich „Percent“ auf einen Vergleich zwischen der *möglichen* Gesamtzahl der vom Batch-Dienst erfassten Stichproben und der Anzahl von Stichproben, die tatsächlich für Ihre Formel für die automatische Skalierung *verfügbar* sind.

Lassen Sie uns als Beispiel eine Zeitspanne von 10 Minuten betrachten. Da Stichproben in einer Zeitspanne von 10 Minuten alle 30 Sekunden erfasst werden, werden maximal 20 Stichproben von Batch erfasst (zwei pro Minute). Doch aufgrund der erwähnten Latenz des Berichterstellungsmechanismus oder anderer Probleme in der Azure-Infrastruktur stehen Ihrer Formel für die automatische Skalierung möglicherweise nur 15 Stichproben zur Verfügung. Das bedeutet, dass in diesem zehnminütigen Zeitraum nur **75 Prozent** der Gesamtanzahl erfasster Stichproben tatsächlich für Ihre Formel verfügbar sind.

**GetSample() und Stichprobenbereiche**

Die Formeln für die automatische Skalierung werden Ihre Pools vergrößern und verkleinern und Knoten hinzufügen oder entfernen. Da Knoten Geld Kosten, möchten Sie sicherstellen, dass Ihre Formeln eine sinnvolle Analysemethode verwenden, die auf ausreichend Daten basiert. Aus diesem Grund wird empfohlen, dass Sie eine Analyse des Typs Trend in Ihren Formeln verwenden. Dieser Typ vergrößert oder verkleinert Ihre Pools basierend auf einem *Bereich* gesammelter Stichproben.

Verwenden Sie hierzu `GetSample(interval look-back start, interval look-back end)`, um einen **Vektor** von Stichproben zurückzugeben:

```
$runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);
```

Wenn die obige Zeile von Batch ausgewertet wird, gibt sie einen Bereich von Stichproben als Vektor von Werten zurück. Beispiel:

```
$runningTasksSample=[1,1,1,1,1,1,1,1,1,1];
```

Nachdem Sie den Vektor von Stichproben erfasst haben, können Sie Funktionen wie `min()`, `max()` und `avg()` verwenden, um aussagekräftige Werte aus dem erfassten Bereich abzuleiten.

Zur Erhöhung der Sicherheit können Sie einen *Fehler* bei der Formelauswertung erzwingen, wenn für einen bestimmten Zeitraum weniger als ein bestimmter Prozentsatz von Stichproben verfügbar ist. Mit dem Erzwingen eines Fehlers bei einer Formelauswertung weisen Sie Batch an, die weitere Auswertung der Formel zu beenden, sofern der angegebene Prozentsatz von Stichproben nicht zur Verfügung steht. An der Größe des Pools erfolgt dann keinerlei Änderung. Wenn Sie einen erforderlichen Prozentsatz von Stichproben angeben möchten, damit die Auswertung Erfolg hat, geben Sie diesen in `GetSample()` als dritten Parameter an. Hier wird ein Mindestprozentsatz von 75 % angegeben:

```
$runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);
```

Weil bei der Verfügbarkeit von Stichproben eine Verzögerung auftreten kann, müssen Sie stets einen Zeitbereich mit einer Startzeit wählen, die mehr als eine Minute zurückliegt. Es dauert ca. eine Minute, bis Stichproben das System durchlaufen haben, weshalb Stichproben im Bereich `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` möglicherweise nicht verfügbar sind. In diesem Fall können Sie den Prozentsatzparameter `GetSample()` angeben, um einen bestimmten Prozentsatz von Stichproben zu erzwingen.

> [!IMPORTANT]
> Es wird **dringend empfohlen**, sich **in den Formeln für die automatische Skalierung nicht *ausschließlich* auf `GetSample(1)` zu verlassen**. Der Grund ist, dass `GetSample(1)` im Wesentlichen den Batch-Dienst anweist, die letzte vorhandene Stichprobe unabhängig vom Zeitpunkt ihrer Erfassung bereitzustellen. Da es sich nur um ein Stichprobe handelt, die ggf. schon älter ist, ist diese möglicherweise nicht für den aktuellen Aufgaben- oder Ressourcenstatus repräsentativ. Stellen Sie bei Verwendung von `GetSample(1)` sicher, dass dieser Wert zu einer längeren Anweisung gehört und nicht der einzige Datenpunkt ist, auf dem Ihre Formel basiert.
>
>

## <a name="metrics"></a>Metriken
Für das Definieren einer Formel können Sie sowohl **Ressourcenmetriken** als auch **Taskmetriken** verwenden. Sie passen die vorgegebene Anzahl dedizierter Knoten im Pool basierend auf den Metrikdaten an, die Sie abrufen und auswerten. Im Abschnitt [Variablen](#variables) finden Sie weitere Informationen zu den einzelnen Metriken.

<table>
  <tr>
    <th>Metrik</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><b>Ressource</b></td>
    <td><p><b>Ressourcenmetriken</b> basieren sowohl auf der Auslastung von CPU und Arbeitsspeicher und der Bandbreite der Computeknoten als auch auf deren Anzahl.</p>
        <p> Folgende vom Dienst definierte Variablen eignen sich für Anpassungen auf der Grundlage der Knotenanzahl:</p>
    <p><ul>
      <li>$TargetDedicated</li>
            <li>$CurrentDedicated</li>
            <li>$SampleNodeCount</li>
    </ul></p>
    <p>Folgende vom Dienst definierte Variablen eignen sich für Anpassungen auf der Grundlage der Ressourcenverwendung von Knoten:</p>
    <p><ul>
      <li>$CPUPercent</li>
      <li>$WallClockSeconds</li>
      <li>$MemoryBytes</li>
      <li>$DiskBytes</li>
      <li>$DiskReadBytes</li>
      <li>$DiskWriteBytes</li>
      <li>$DiskReadOps</li>
      <li>$DiskWriteOps</li>
      <li>$NetworkInBytes</li>
      <li>$NetworkOutBytes</li></ul></p>
  </tr>
  <tr>
    <td><b>Task</b></td>
    <td><p><b>Aufgabenmetriken</b> basieren auf dem Aufgabenstatus (z.B. „Aktiv“, „Ausstehend“ oder „Abgeschlossen“). Folgende vom Dienst definierte Variablen eignen sich für Poolgrößenanpassungen auf der Grundlage von Aufgabenmetriken:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## <a name="write-an-autoscale-formula"></a>Schreiben einer Formel für die automatische Skalierung
Sie können eine Formel für die automatische Skalierung erstellen, indem Sie mithilfe der oben aufgeführten Komponenten Anweisungen formulieren und diese dann zu einer vollständigen Formel kombinieren. In diesem Abschnitt erstellen wir eine Beispielformel für die automatische Skalierung, mit der einige praxistaugliche Skalierungsentscheidungen getroffen werden können.

Zuerst definieren wir die Anforderungen für die neue Formel für die automatische Skalierung. Mit der Formel soll Folgendes erreicht werden:

1. Die vorgegebene Anzahl von Computeknoten in einem Pool soll **erhöht** werden, wenn die CPU-Auslastung hoch ist.
2. Die vorgegebene Anzahl von Computeknoten in einem Pool soll **reduziert** werden, wenn die CPU-Auslastung gering ist.
3. Die **maximale** Anzahl von Knoten muss immer auf 400 beschränkt sein.

Zur *Erhöhung* der Knotenanzahl bei hoher CPU-Auslastung definieren wir eine Anweisung, die eine benutzerdefinierte Variable (`$totalNodes`) mit einem Wert auffüllt, der 110 Prozent der aktuellen Zielanzahl von Knoten darstellt. Dies gilt aber nur, wenn die minimale durchschnittliche CPU-Auslastung in den vergangenen 10 Minuten über 70 Prozent lag. Andernfalls verwenden wir den aktuellen dedizierten Wert.

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
```

Zum *Reduzieren* der Knotenanzahl während einer Phase mit geringer CPU-Auslastung legen wir mit der nächsten Anweisung in unserer Formel dieselbe `$totalNodes`-Variable auf 90 Prozent der aktuellen Zielanzahl von Knoten fest, wenn die durchschnittliche CPU-Auslastung in den letzten 60 Minuten unter 20 Prozent lag. Verwenden Sie andernfalls den aktuellen Wert von `$totalNodes`, den wir oben in der Anweisung angegeben haben.

```
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
```

Beschränken wir nun die vorgegebene Anzahl dedizierter Computeknoten auf **maximal** 400:

```
$TargetDedicated = min(400, $totalNodes)
```

Die vollständige Formel lautet:

```
$totalNodes =
    (min($CPUPercent.GetSample(TimeInterval_Minute * 10)) > 0.7) ?
    ($CurrentDedicated * 1.1) : $CurrentDedicated;
$totalNodes =
    (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ?
    ($CurrentDedicated * 0.9) : $totalNodes;
$TargetDedicated = min(400, $totalNodes)
```

## <a name="create-an-autoscale-enabled-pool"></a>Erstellen eines Pools mit aktivierter automatischer Skalierung
Sie können eines der folgenden Verfahren verwenden, um einen neuen Pool mit aktivierter automatischer Skalierung zu erstellen:

**Batch .NET**

1. Erstellen Sie den Pool mit [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx).
2. Legen Sie die [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx)-Eigenschaft auf `true` fest.
3. Legen Sie die [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx)-Eigenschaft mit Ihrer Formel für die automatische Skalierung fest.
4. (Optional) Legen Sie die [CloudPool.AutoScaleEvaluationInterval](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoScaleevaluationinterval.aspx)-Eigenschaft fest (Standardeinstellung ist 15 Minuten).
5. Führen Sie für den Pool mit [CloudPool.Commit](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commit.aspx) oder [CommitAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.commitasync.aspx) einen Commit durch.

**Batch-REST-API**

* [Hinzufügen eines Pools zu einem Konto](https://msdn.microsoft.com/library/azure/dn820174.aspx): Geben Sie die Elemente `enableAutoScale` und `autoScaleFormula` in Ihrer REST-API-Anforderung an, um die automatische Skalierung für einen Pool zu konfigurieren, wenn Sie ihn erstellen.

Mit dem folgenden Codeausschnitt wird mithilfe der [Batch .NET][net_api]-Bibliothek ein Pool mit aktivierter automatischer Skalierung erstellt. Die Formel für die automatische Skalierung des Pools legt die vorgegebene Anzahl von Knoten auf 5 am Montag und auf 1 an allen anderen Wochentagen fest. Das [Intervall für die automatische Skalierung](#automatic-scaling-interval) wird auf 30 Minuten festgelegt. In diesem und anderen C#-Ausschnitten in diesem Artikel ist „myBatchClient“ eine ordnungsgemäß initialisierte Instanz von [BatchClient][net_batchclient].

```csharp
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

Zusätzlich zur Batch-REST-API und zum .NET SDK können Sie auch die anderen [Batch-SDKs](batch-apis-tools.md#azure-accounts-for-batch-development), [Batch PowerShell-Cmdlets](batch-powershell-cmdlets-get-started.md) und die [Batch-CLI](batch-cli-get-started.md) für die automatische Skalierung verwenden.

> [!IMPORTANT]
> Wenn Sie einen Pool mit aktivierter automatischer Skalierung erstellen, dürfen Sie den Parameter `targetDedicated` **nicht** angeben. Beachten Sie auch, dass Sie zur manuellen Anpassung der Größe eines Pools mit aktivierter automatischer Skalierung (etwa mit [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]) zunächst die automatische Skalierung **deaktivieren** müssen, um die Größe des Pools ändern zu können.
>
>

### <a name="automatic-scaling-interval"></a>Intervall für die automatische Skalierung
Standardmäßig passt der Batch-Dienst die Poolgröße gemäß seiner Formel für die automatische Skalierung alle **15 Minuten**an. Dieses Intervall kann jedoch mithilfe der folgenden Pooleigenschaften konfiguriert werden:

* [CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval] (Batch .NET)
* [autoScaleEvaluationInterval][rest_autoscaleinterval] (REST-API)

Das kürzeste Intervall ist fünf Minuten, das längste 168 Stunden. Wenn ein Intervall außerhalb dieses Bereichs angegeben wird, gibt der Batch-Dienst einen Fehler des Typs „Unzulässige Anforderung (400)“ zurück.

> [!NOTE]
> Die automatische Skalierung ist derzeit nicht als Reaktion im Zeitraum unter einer Minute auf Änderungen vorgesehen, sondern dient eher zum allmählichen Anpassen der Größe Ihres Pools während der Ausführung Ihres Workloads.
>
>

## <a name="enable-autoscaling-on-an-existing-pool"></a>Aktivieren der automatischen Skalierung für einen vorhandenen Pool
Wenn Sie bereits einen Pool mit einer festgelegten Anzahl von Computeknoten mit dem Parameter *targetDedicated* erstellt haben, können Sie die automatische Skalierung trotzdem aktivieren. Jedes Batch-SDK verfügt über einen Vorgang zum Aktivieren der automatischen Skalierung („enable autoscale“), z.B.:

* [BatchClient.PoolOperations.EnableAutoScale][net_enableautoscale] (Batch .NET)
* [Enable automatic scaling on a pool][rest_enableautoscale] (Aktivieren des automatischen Skalierens für einen Pool) (REST-API)

Wenn Sie die automatische Skalierung für einen vorhandenen Pool aktivieren, gilt Folgendes:

* Falls die automatische Skalierung für den Pool derzeit **deaktiviert** ist, wenn Sie die Anforderung „enable autoscale“ ausführen, *müssen* Sie dabei eine gültige autoscale-Formel angeben. Sie können *optional* ein Auswertungsintervall für die automatische Skalierung angeben. Wenn Sie kein Intervall angeben, wird der Standardwert „15 Minuten“ verwendet.
* Wenn die automatische Skalierung für den Pool derzeit **aktiviert** ist, können Sie eine autoscale-Formel, ein Auswertungsintervall oder beides angeben. Es ist nicht möglich, beide Eigenschaften wegzulassen.

  * Wenn Sie ein neues Auswertungsintervall für die automatische Skalierung angeben, wird der vorhandene Auswertungszeitplan beendet und ein neuer Zeitplan gestartet. Die Startzeit des neuen Zeitplans ist der Zeitpunkt, an dem die Anforderung vom Typ „enable autoscale“ ausgeführt wurde.
  * Wenn Sie die autoscale-Formel oder das Auswertungsintervall weglassen, nutzt der Batch-Dienst weiterhin den aktuellen Wert dieser Einstellung.

> [!NOTE]
> Wenn beim Erstellen des Pools für den *targetDedicated* -Parameter ein Wert angegeben wurde, wird dieser beim Auswerten der Formel für das automatische Skalieren ignoriert.
>
>

In diesem C#-Codeausschnitt wird die [Batch .NET][net_api]-Bibliothek verwendet, um die automatische Skalierung in einem vorhandenen Pool zu aktivieren:

```csharp
// Define the autoscaling formula. This formula sets the target number of nodes
// to 5 on Mondays, and 1 on every other day of the week
string myAutoScaleFormula = "$TargetDedicated = (time().weekday == 1 ? 5:1);";

// Set the autoscale formula on the existing pool
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myAutoScaleFormula);
```

### <a name="update-an-autoscale-formula"></a>Aktualisieren einer Formel für die automatische Skalierung
Sie verwenden die gleiche Anforderung vom Typ „enable autoscale“, um die Formel in einem vorhandenen Pool mit aktivierter automatischer Skalierung zu *aktualisieren* (beispielsweise mit [EnableAutoScale][net_enableautoscale] in Batch .NET). Es ist kein spezieller Vorgang vom Typ „update autoscale“ vorhanden. Wenn die automatische Skalierung für „myexistingpool“ beispielsweise bereits aktiviert ist, wird die autoscale-Formel beim Ausführen des folgenden Codes durch den Inhalt von `myNewFormula` ersetzt.

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleFormula: myNewFormula);
```

### <a name="update-the-autoscale-interval"></a>Aktualisieren des Intervalls für die automatische Skalierung
Zum Ändern des Auswertungsintervalls für die automatische Skalierung eines Pools mit aktivierter automatischer Skalierung wird die gleiche [EnableAutoScale][net_enableautoscale]-Methode verwendet wie beim Aktualisieren einer Formel für die automatische Skalierung. Gehen Sie beispielsweise wie folgt vor, um das Auswertungsintervall für die automatische Skalierung für einen Pool, der bereits entsprechend aktiviert wurde, auf 60 Minuten festzulegen:

```csharp
myBatchClient.PoolOperations.EnableAutoScale(
    "myexistingpool",
    autoscaleEvaluationInterval: TimeSpan.FromMinutes(60));
```

## <a name="evaluate-an-autoscale-formula"></a>Erstellen einer autoscale-Formel
Sie können eine Formel auswerten, bevor Sie sie auf einen Pool anwenden. Auf diese Weise können Sie einen „Testlauf“ für die Formel durchführen, um die Auswertung für die Anweisungen zu prüfen, bevor Sie die Formel in der Produktion einsetzen.

Damit Sie eine Formel für die automatische Skalierung auswerten können, müssen Sie zuerst die **automatische Skalierung** für den Pool mit einer **gültigen Formel** aktivieren. Wenn Sie eine Formel für einen Pool testen möchten, für den die automatische Skalierung noch nicht aktiviert ist, können Sie beim ersten Aktivieren der automatischen Skalierung die einzeilige Formel `$TargetDedicated = 0` verwenden. Verwenden Sie anschließend eines der folgenden Verfahren, um die zu testende Formel auszuwerten:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) oder [EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx)

    Für diese Batch .NET-Methoden sind die ID eines vorhandenen Pools und eine Zeichenfolge mit der auszuwertenden autoscale-Formel erforderlich. Die Ergebnisse der Auswertung sind in der zurückgegebenen [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx)-Instanz enthalten.
* [Auswerten einer Formel für die automatische Skalierung](https://msdn.microsoft.com/library/azure/dn820183.aspx)

    Geben Sie in dieser REST-API-Anforderung die Pool-ID im URI und die autoscale-Formel im *autoScaleFormula*-Element des Anforderungstexts an. Die bei der Anfrage generierte Antwort enthält Fehlerinformationen, die in Zusammenhang mit der Formel stehen können.

In diesem [Batch .NET][net_api]-Codeausschnitt evaluieren wir eine Formel, bevor wir sie auf den [CloudPool][net_cloudpool] anwenden. Wenn für den Pool noch keine automatische Skalierung aktiviert ist, führen wir dies zuerst aus.

```csharp
// First obtain a reference to an existing pool
CloudPool pool = batchClient.PoolOperations.GetPool("myExistingPool");

// If autoscaling isn't already enabled on the pool, enable it.
// You can't evaluate an autoscale formula on non-autoscale-enabled pool.
if (pool.AutoScaleEnabled == false)
{
    // We need a valid autoscale formula to enable autoscaling on the
    // pool. This formula is valid, but won't resize the pool:
    pool.EnableAutoScale(
        autoscaleFormula: $"$TargetDedicated = {pool.CurrentDedicated};",
        autoscaleEvaluationInterval: TimeSpan.FromMinutes(5));

    // Batch limits EnableAutoScale calls to once every 30 seconds.
    // Because we want to apply our new autoscale formula below if it
    // evaluates successfully, and we *just* enabled autoscaling on
    // this pool, we pause here to ensure we pass that threshold.
    Thread.Sleep(TimeSpan.FromSeconds(31));

    // Refresh the properties of the pool so that we've got the
    // latest value for AutoScaleEnabled
    pool.Refresh();
}

// We must ensure that autoscaling is enabled on the pool prior to
// evaluating a formula
if (pool.AutoScaleEnabled == true)
{
    // The formula to evaluate - adjusts target number of nodes based on
    // day of week and time of day
    string myFormula = @"
        $curTime = time();
        $workHours = $curTime.hour >= 8 && $curTime.hour < 18;
        $isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
        $isWorkingWeekdayHour = $workHours && $isWeekday;
        $TargetDedicated = $isWorkingWeekdayHour ? 20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not
    // actually apply the formula to the pool.
    AutoScaleRun eval =
        batchClient.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun.
        // This will display the values of each variable as evaluated by the
        // autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " +
            eval.Results.Replace("$", "\n    $"));

        // Apply the formula to the pool since it evaluated successfully
        batchClient.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " +
            eval.Error.Message);
    }
}
```

Eine erfolgreiche Auswertung der Formel in diesem Codeausschnitt sollte eine Ausgabe ähnlich der folgenden ergeben:

```
AutoScaleRun.Results:
    $TargetDedicated=10;
    $NodeDeallocationOption=requeue;
    $curTime=2016-10-13T19:18:47.805Z;
    $isWeekday=1;
    $isWorkingWeekdayHour=0;
    $workHours=0
```

## <a name="get-information-about-autoscale-runs"></a>Abrufen von Informationen zu Ausführungen der automatischen Skalierung
Um sicherzustellen, dass die Formel wie erwartet funktioniert, sind regelmäßige Überprüfungen der Ergebnisse von Ausführungen der automatischen Skalierung ratsam, die von Batch für den Pool vorgenommen werden. Rufen Sie hierzu einen Verweis auf den Pool auf (oder aktualisieren Sie ihn), und untersuchen Sie die Eigenschaften der letzten Ausführung der automatischen Skalierung.

In Batch .NET verfügt die [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx)-Eigenschaft über mehrere Eigenschaften, die Informationen zur letzten Ausführung der automatischen Skalierung für den Pool durch den Batch-Dienst liefern.

* [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
* [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)

In der REST-API gibt die Anforderung zum [Abrufen von Informationen zu einem Pool](https://msdn.microsoft.com/library/dn820165.aspx) Informationen zum Pool zurück, z.B. zur letzten Ausführung der automatischen Skalierung in [autoScaleRun](https://msdn.microsoft.com/library/dn820165.aspx#bk_autrun).

Im folgenden C#-Codeausschnitt wird die Batch .NET-Bibliothek verwendet, um Informationen zur letzten Ausführung der automatischen Skalierung für den Pool „myPool“ auszugeben:

```csharp
Cloud pool = myBatchClient.PoolOperations.GetPool("myPool");
Console.WriteLine("Last execution: " + pool.AutoScaleRun.Timestamp);
Console.WriteLine("Result:" + pool.AutoScaleRun.Results.Replace("$", "\n  $"));
Console.WriteLine("Error: " + pool.AutoScaleRun.Error);
```

Beispielausgabe des obigen Codeausschnitts:

```
Last execution: 10/14/2016 18:36:43
Result:
  $TargetDedicated=10;
  $NodeDeallocationOption=requeue;
  $curTime=2016-10-14T18:36:43.282Z;
  $isWeekday=1;
  $isWorkingWeekdayHour=0;
  $workHours=0
Error:
```

## <a name="example-autoscale-formulas"></a>Beispiele für autoscale-Formeln
Hier sind einige Formeln angegeben, die verschiedene Möglichkeiten zum Anpassen der Anzahl von Computeressourcen in einem Pool darstellen.

### <a name="example-1-time-based-adjustment"></a>Beispiel 1: Zeitbasierte Anpassung
Es kann beispielsweise sein, dass Sie die Größe des Pools basierend auf Wochentag und Uhrzeit anpassen und die Anzahl der Knoten im Pool entsprechend erhöhen oder reduzieren möchten:

Diese Formel ruft zunächst die aktuelle Uhrzeit ab. Wenn es sich um einen Werktag (1 bis 5) handelt und der Wert innerhalb der Geschäftszeiten (8:00 Uhr bis 18:00 Uhr) liegt, wird die Zielgröße des Pools auf 20 Knoten festgelegt. Andernfalls wird der Wert auf 10 Knoten festgelegt.

```
$curTime = time();
$workHours = $curTime.hour >= 8 && $curTime.hour < 18;
$isWeekday = $curTime.weekday >= 1 && $curTime.weekday <= 5;
$isWorkingWeekdayHour = $workHours && $isWeekday;
$TargetDedicated = $isWorkingWeekdayHour ? 20:10;
```

### <a name="example-2-task-based-adjustment"></a>Beispiel 2: Aufgabenbasierte Anpassung
In diesem Beispiel wird die Größe des Pools basierend auf der Anzahl der Aufgaben in der Warteschlange angepasst. Beachten Sie, dass in Formelzeichenfolgen sowohl Kommentare als auch Zeilenumbrüche verwendet werden können.

```csharp
// Get pending tasks for the past 15 minutes.
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point,
// otherwise we use the maximum of last sample point and the history average.
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise
// half of current dedicated.
$targetVMs = $tasks > 0? $tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it
// to 20. This value should be adjusted according to your use case.
$TargetDedicated = max(0, min($targetVMs, 20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-3-accounting-for-parallel-tasks"></a>Beispiel 3: Berücksichtigung paralleler Aufgaben
Auch in diesem Beispiel wird die Poolgröße an die Anzahl der Aufgaben angepasst. Diese Formel berücksichtigt auch den für den Pool festgelegten Wert [MaxTasksPerComputeNode][net_maxtasks]. Dies ist besonders hilfreich, wenn in Ihrem Pool die [parallele Taskausführung](batch-parallel-node-tasks.md) aktiviert wurde.

```csharp
// Determine whether 70 percent of the samples have been recorded in the past
// 15 minutes; if not, use last sample
$samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$tasks = $samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the
// MaxTasksPerComputeNode property on this pool is set to 4, adjust this number
// for your use case)
$cores = $TargetDedicated * 4;
$extraVMs = (($tasks - $cores) + 3) / 4;
$targetVMs = ($TargetDedicated + $extraVMs);
// Attempt to grow the number of compute nodes to match the number of active
// tasks, with a maximum of 3
$TargetDedicated = max(0,min($targetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### <a name="example-4-setting-an-initial-pool-size"></a>Beispiel 4: Festlegen einer anfänglichen Poolgröße
Dieses Beispiel zeigt einen C#-Codeausschnitt mit einer Formel für die automatische Skalierung, die die Größe des Pools für einen anfänglichen Zeitraum auf eine bestimmte Anzahl von Knoten festlegt. Dann passt sie die Größe des Pools nach dem ersten Zeitraum basierend auf der Anzahl der ausgeführten und aktive Aufgaben an.

Mit der Formel im folgenden Codeausschnitt wird Folgendes durchgeführt:

* Die anfängliche Poolgröße wird auf 4 Knoten festgelegt.
* Die Größe des Pools wird innerhalb der ersten 10 Minuten des Lebenszyklus des Pools nicht angepasst.
* Nach 10 Minuten wird die maximale Anzahl ausgeführter und aktiver Aufgaben in den letzten 60 Minuten abgerufen.
  * Falls beide Werte 0 sind (d. h., dass in den letzten 60 Minuten keine Aufgaben ausgeführt wurden oder aktiv waren), wird die Poolgröße auf 0 festgelegt.
  * Wenn einer der Werte größer als null ist, erfolgt keine Änderung.

```csharp
string now = DateTime.UtcNow.ToString("r");
string formula = string.Format(@"
    $TargetDedicated = {1};
    lifespan         = time() - time(""{0}"");
    span             = TimeInterval_Minute * 60;
    startup          = TimeInterval_Minute * 10;
    ratio            = 50;

    $TargetDedicated = (lifespan > startup ? (max($RunningTasks.GetSample(span, ratio), $ActiveTasks.GetSample(span, ratio)) == 0 ? 0 : $TargetDedicated) : {1});
    ", now, 4);
```

## <a name="next-steps"></a>Nächste Schritte
* [Maximieren der Azure Batch-Computeressourcenauslastung mit parallelen Knotenaufgaben](batch-parallel-node-tasks.md) enthält Informationen dazu, wie Sie auf den Computeknoten im Pool mehrere Tasks gleichzeitig ausführen können. Zusätzlich zur automatischen Skalierung können Sie mit diesem Feature die Auftragsdauer für einige Workloads verringern und so Geld sparen.
* Um die Effizienz weiter zu steigern, stellen Sie sicher, dass die Batch-Anwendung den Batch-Dienst in optimaler Weise abfragt. In [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md)erfahren Sie, wie Sie die Datenmenge beschränken, die übertragen wird, wenn Sie den Status von Tausenden von Computeknoten oder -tasks abfragen.

[net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_batchclient]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_cloudpool_autoscaleformula]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx
[net_cloudpool_autoscaleevalinterval]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleevaluationinterval.aspx
[net_enableautoscale]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.enableautoscale.aspx
[net_maxtasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx
[net_poolops_resizepool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.resizepool.aspx

[rest_api]: https://msdn.microsoft.com/library/azure/dn820158.aspx
[rest_autoscaleformula]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_autoscaleinterval]: https://msdn.microsoft.com/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx

