---
title: Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool | Microsoft Docs
description: Aktivieren Sie das automatische Skalieren in einem Cloudpool, um die Anzahl von Computeknoten im Pool dynamisch anzupassen.
services: batch
documentationcenter: ''
author: mmacy
manager: timlt
editor: tysonn

ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: multiple
ms.date: 09/27/2016
ms.author: marsma

---
# Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool
Durch automatisches Skalieren kann der Azure Batch-Dienst Computeknoten in einem Pool auf der Grundlage der von Ihnen definierten Parameter dynamisch hinzufügen oder entfernen. Die automatische Anpassung der von Ihrer Anwendung beanspruchten Rechenleistung kann sowohl Zeit als auch Geld sparen: Fügen Sie Knoten hinzu, wenn sich die Aufgabenanforderungen Ihres Auftrags erhöhen, und entfernen Sie sie, wenn sich die Anforderungen wieder verringern.

Sie aktivieren die automatische Skalierung für einen Pool von Computeknoten, indem Sie dem Pool eine *Formel für die automatische Skalierung* zuordnen, die Sie z. B. mithilfe der [PoolOperations.EnableAutoScale][net_enableautoscale]-Methode in der [Batch-Bibliothek für .NET](batch-dotnet-get-started.md) definieren. Der Batch-Dienst verwendet dann diese Formel, um die Anzahl von Computeknoten zu bestimmen, die zum Ausführen des Workloads erforderlich sind. Der Batch-Dienst reagiert auf periodisch gesammelte Stichprobenwerten von Dienstmetriken und passt die Anzahl der Computeknoten im Pool mithilfe der zugeordneten Formel in einem konfigurierbaren Intervall an.

Sie können die automatische Skalierung beim Erstellen eines Pools oder später für einen bereits vorhandenen Pool aktivieren. Sie können auch eine vorhandene Formel in einem Pool ändern, für den die „automatische Skalierung“ aktiviert ist. Der Batch-Dienst bietet die Möglichkeit, die Formeln auszuwerten, bevor sie Pools zugewiesen werden, und den Status automatischer Skalierungen zu überwachen.

## Formeln für die automatische Skalierung
Bei einer Formel für die automatische Skalierung handelt es sich um einen von Ihnen definierten Zeichenfolgenwert mit mindestens einer Anweisung, der dem [autoScaleFormula][rest_autoscaleformula]-Element (Batch für REST) eines Pools oder der [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula]-Eigenschaft (Batch für .NET) eines Pools zugewiesen ist. Bei der Zuweisung zu einem Pool verwendet der Batch-Dienst Ihre Formel, um für das nächste Verarbeitungsintervall die Zielanzahl der Computeknoten in einem Pool zu bestimmen (mehr zu Intervallen weiter unten). Die Formelzeichenfolge darf eine Größe von 8 KB nicht überschreiten und kann bis zu 100 durch Semikolons getrennte Anweisungen sowie Zeilenumbrüche und Kommentare enthalten.

Sie können sich Formeln für die automatische Skalierung als eine Batch-„Sprache“ für die automatische Skalierung vorstellen. Formelanweisungen sind frei definierte Ausdrücke, die sowohl dienstdefinierte Variablen (vom Batch-Dienst definiert) als auch benutzerdefinierte Variablen (von Ihnen selbst definiert) enthalten können. Mithilfe von mitgelieferten Typen, Operatoren und Funktionen können sie für diese Werte eine Vielzahl von Operationen ausführen. Eine Anweisung kann beispielsweise wie folgt aussehen:

`$myNewVariable = function($ServiceDefinedVariable, $myCustomVariable);`

Formeln enthalten im Allgemeinen mehrere Anweisungen, die Operationen für Werte durchführen, die in vorherigen Anweisungen abgerufen wurden. So rufen wir beispielsweise zuerst einen Wert für `variable1` ab und übergeben ihn dann an eine Funktion, um `variable2` aufzufüllen:

```
$variable1 = function1($ServiceDefinedVariable);
$variable2 = function2($OtherServiceDefinedVariable, $variable1);
```

Mit diesen Anweisungen in Ihrer Formel bestimmen Sie die Anzahl von Computeknoten, auf die der Pool skaliert werden soll (also die **Zielanzahl** von **dedizierten Knoten**). Diese Zahl kann höher, niedriger oder identisch mit der aktuellen Anzahl von Knoten im Pool sein. Batch wertet die Formel für die automatische Skalierung des Pools in einem bestimmten Intervall aus. ([Intervalle für die automatische Skalierung](#automatic-scaling-interval) werden weiter unten erläutert). Anschließend wird die vorgegebene Anzahl von Knoten im Pool auf die Anzahl angepasst, die die Formel für die automatische Skalierung zum Zeitpunkt der Auswertung angibt.

Kurzbeispiel: Diese zweizeilige Formel für die automatische Skalierung gibt an, dass die Anzahl von Knoten gemäß der Anzahl von aktiven Aufgaben bis zu einem Höchstwert von 10 Computeknoten angepasst werden soll.

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

In den nächsten Abschnitten dieses Artikels werden die verschiedenen Elemente erläutert, mit denen Ihre Formeln für die automatische Skalierung gebildet werden. Dazu zählen Variablen, Operatoren, Operationen und Funktionen. Sie lernen, wie Sie verschiedene Metriken zu Compute-Ressourcen und Aufgaben in Batch abrufen. Sie können diese Metriken verwenden, um die Knotenanzahl des Pools basierend auf Ressourcenverbrauch und Aufgabenstatus automatisch anzupassen. Anschließend erfahren Sie, wie Sie mit der REST-API und der .NET-API von Batch eine Formel erstellen und die automatische Skalierung für einen Pool aktivieren. Zum Abschluss werden nun verschiedene Beispielformeln gezeigt.

> [!IMPORTANT]
> Jedes Azure Batch-Konto ist auf eine bestimmte Anzahl von Kernen (und damit auf eine maximale Anzahl von Computeknoten) beschränkt, die für die Verarbeitung verwendet werden können. Der Batch-Dienst erstellt maximal so viele Knoten, wie durch den Kerngrenzwert vorgegeben. Daher wird die durch die Formel angegebene Zielanzahl von Computeknoten möglicherweise nicht erreicht. Unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) finden Sie Informationen zum Anzeigen und Erhöhen Ihrer Kontokontingente.
> 
> 

## Variablen
Sie können in Ihrer Formel für die automatische Skalierung sowohl **dienstdefinierte** als auch **benutzerdefinierte** Variablen verwenden. Die vom Dienst definierten Variablen sind in den Batch-Dienst integriert – einige mit Lese-/Schreibzugriff, andere schreibgeschützt. Benutzerdefinierte Variablen sind Variablen, die *Sie* definieren. In der obigen zweizeiligen Beispielformel ist `$TargetDedicated` eine vom Dienst definierte Variable, `$averageActiveTaskCount` ist eine benutzerdefinierte Variable.

Die folgenden Tabellen enthalten sowohl Variablen mit Lese-/Schreibzugriff als auch schreibgeschützte Variablen, die vom Batch-Dienst definiert werden.

Sie können diese vom Dienst definierten Variablen **abrufen** und **festlegen**, um die Anzahl von Computeknoten in einem Pool zu verwalten:

| Vom Dienst definierte Variablen mit Lese-/Schreibzugriff | Beschreibung |
| --- | --- |
| $TargetDedicated |Die **Zielanzahl** **dedizierter Computeknoten** für den Pool. Dies ist die Anzahl von Computeknoten, auf die der Pool skaliert werden soll. Es handelt sich um einen „Zielwert“, da ein Pool die Zielanzahl von Knoten möglicherweise nicht erreicht. Dies kann auftreten, wenn die vorgegebene Anzahl von Knoten erneut durch eine Auswertung der automatischen Skalierung geändert wird, bevor der Pool das ursprüngliche Ziel erreicht hat. Es kann auch vorkommen, wenn ein Batch-Konto-Knoten oder Kern-Kontingent erreicht ist, bevor die vorgegebene Anzahl von Knoten erreicht wird. |
| $NodeDeallocationOption |Dieser Vorgang wird ausgeführt, wenn Computeknoten aus einem Pool entfernt werden. Mögliche Werte:<ul><li>**requeue**: Beendet Tasks sofort und stellt sie wieder in die Warteschlange, damit sie neu geplant werden.<li>**terminate**: Beendet Tasks sofort und entfernt sie aus der Auftragswarteschlange.<li>**taskcompletion**: Wartet auf die Beendigung aktuell ausgeführter Tasks und entfernt dann den Knoten aus dem Pool.<li>**retaineddata**: Wartet, bis alle lokalen, durch Tasks beibehaltenen Daten bereinigt wurden, und entfernt dann den Knoten aus dem Pool.</ul> |

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

> [!TIP]
> Die obigen, vom Dienst definierten schreibgeschützten Variablen sind *Objekte*, die verschiedene Methoden für den Zugriff auf die zum jeweiligen Objekt gehörigen Daten bereitstellen. Weitere Informationen finden Sie weiter unten unter [Erfassen von Stichprobendaten](#getsampledata).
> 
> 

## Typen
Folgende **Typen** werden in Formeln unterstützt.

* double
* doubleVec
* doubleVecList
* string
* timestamp – „timestamp“ ist eine Verbundstruktur, die folgende Member enthält:
  
  * year
  * Monat (1-12)
  * Tag (1-31)
  * weekday (als Zahl, z. B. 1 für Montag)
  * hour (im 24-Stunden-Format, z. B. 13 für 13 Uhr)
  * Minute (00-59)
  * Sekunde (00-59)
* timeInterval
  
  * TimeInterval\_Zero
  * TimeInterval\_100ns
  * TimeInterval\_Microsecond
  * TimeInterval\_Millisecond
  * TimeInterval\_Second
  * TimeInterval\_Minute
  * TimeInterval\_Hour
  * TimeInterval\_Day
  * TimeInterval\_Week
  * TimeInterval\_Year

## Vorgänge
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
| timestamp *Operator* timestamp |- |timeinterval |

Beim Testen von „double“ mit einem ternären Operator (`double ? statement1 : statement2`) sind Werte ungleich Null **true**, und Nullwerte sind **false**.

## Functions
Zum Definieren einer Formel für die automatische Skalierung stehen folgende vordefinierte **Funktionen** zur Verfügung.

| Funktion | Rückgabetyp | Beschreibung |
| --- | --- | --- |
| avg(doubleVecList) |double |Der Durchschnittswert aller Werte in der doubleVecList wird zurückgegeben. |
| len(doubleVecList) |double |Die Länge des Vektors, der aus der doubleVecList erstellt wurde, wird zurückgegeben. |
| lg(double) |double |Gibt für den double-Wert den Logarithmus zur Basis 2 zurück. |
| lg(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 2 zurück. „vec(double)“ muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen. |
| ln(double) |double |Gibt für den double-Wert den natürlichen Logarithmus zurück. |
| ln(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 2 zurück. „vec(double)“ muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen. |
| log(double) |double |Gibt für den double-Wert den Logarithmus zur Basis 10 zurück. |
| log(doubleVecList) |doubleVec |Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 10 zurück. Ein vec(double) muss explizit für einzelne double-Parameter übergeben werden. Andernfalls wird von der double log (double)-Version ausgegangen. |
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
Die Formeln für die automatische Skalierung greifen auf Metrikdaten (Stichproben) zurück, die vom Batch-Dienst bereitgestellt werden. Eine Formel vergrößert oder verkleinert den Pool basierend auf den Werten, die sie vom Dienst erhält. Die obigen, vom Dienst definierten Variablen sind Objekte, die verschiedene Methoden für den Zugriff auf die zum jeweiligen Objekt gehörigen Daten bereitstellen. Der folgende Ausdruck zeigt z. B. eine Anforderung zum Abrufen der letzten fünf Minuten der CPU-Auslastung:

`$CPUPercent.GetSample(TimeInterval_Minute * 5)`

| Methode | Beschreibung |
| --- | --- |
| GetSample() |Die `GetSample()`-Methode gibt einen Vektor aus Datenstichproben zurück.<br/><br/>Eine Stichprobe enthält Metrikdaten, die innerhalb von 30 Sekunden erfasst wurden. Dies bedeutet, dass alle 30 Sekunden eine Stichprobe genommen wird. Wie nachstehend erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Daher stehen möglicherweise nicht alle Stichproben für einen bestimmten Zeitraum für die Bewertung durch eine Formel zur Verfügung.<ul><li>`doubleVec GetSample(double count)`<br/>Gibt die Anzahl von Stichproben an, die aus den jüngsten erfassten Stichproben abgerufen werden sollen.<br/><br/>`GetSample(1)` gibt die neueste verfügbare Stichprobe zurück. Für Metriken wie `$CPUPercent` sollte diese Methode allerdings nicht verwendet werden, da unmöglich feststellbar ist, *wann* die Stichprobe erfasst wurde. Sie kann aktuell oder aber, aufgrund von Systemproblemen, auch wesentlich älter sein. In solchen Fällen ist es besser, wie unten gezeigt ein Zeitintervall zu verwenden.<li>`doubleVec GetSample((timestamp or timeinterval) startTime [, double samplePercent])`<br/>Gibt einen Zeitraum für die Erfassung von Stichprobendaten an. Optional gibt diese Methode auch den Prozentsatz der Stichproben an, die im angeforderten Zeitraum verfügbar sein müssen.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10)` gibt 20 Stichproben zurück, wenn alle Stichproben der letzten zehn Minuten im CPUPercent-Verlauf vorhanden sind. Wenn jedoch die letzte Minute des Verlaufs nicht verfügbar ist, werden nur 18 Stichproben zurückgegeben. In diesem Fall gilt:<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)` erzeugt einen Fehler, da nur 90 Prozent der Stichproben verfügbar sind.<br/><br/>`$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)` wird erfolgreich ausgeführt.<li>`doubleVec GetSample((timestamp or timeinterval) startTime, (timestamp or timeinterval) endTime [, double samplePercent])`<br/>Gibt einen Zeitraum für die Erfassung von Daten an, einschließlich Start- und Endzeit.<br/><br/>Wie bereits erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Diese Verzögerung muss bei Verwendung der `GetSample`-Methode berücksichtigt werden. Weitere Informationen finden Sie bei `GetSamplePercent` weiter unten. |
| GetSamplePeriod() |Gibt den Zeitraum zurück, in dem die Stichproben aus einem alten Stichproben-Dataset gesammelt wurden. |
| Count() |Liefert die Gesamtzahl der Stichprobenwerte im Metrikverlauf zurück. |
| HistoryBeginTime() |Gibt den Zeitstempel des ältesten verfügbaren Stichprobenwerts für die Metrik zurück. |
| GetSamplePercent() |Gibt den Prozentsatz an Stichprobenwerten zurück, die für ein bestimmtes Intervall verfügbar sind. Beispiel:<br/><br/>`doubleVec GetSamplePercent( (timestamp or timeinterval) startTime [, (timestamp or timeinterval) endTime] )`<br/><br/>Da die `GetSample`-Methode einen Fehler erzeugt, wenn der Prozentsatz der zurückgegebenen Stichproben kleiner als der angegebene `samplePercent`-Wert ist, können Sie vorab mithilfe der `GetSamplePercent`-Methode eine Prüfung vornehmen. Wenn nicht genügend Beispiele vorhanden sind, können Sie anschließend eine andere Aktion ausführen, , ohne die Auswertung der automatischen Skalierung zu unterbrechen. |

### Stichproben, Prozentsatz für die Stichprobe und die *GetSample()*-Methode
Das Abrufen von Metrikdaten zu Aufgaben und Ressourcen sowie das anschließende Anpassen der Poolgröße basierend auf diesen Daten ist die Kernfunktion einer Formel für die automatische Skalierung. Daher ist es wichtig, sich damit vertraut zu machen, wie Formeln für die automatische Skalierung mit Metrikdaten bzw. „Stichproben“ interagieren.

**Stichproben**

Der Batch-Dienst nimmt regelmäßig *Stichproben* von Task- und Ressourcenmetriken und stellt sie Ihren Formeln für die automatische Skalierung zur Verfügung. Diese Stichproben werden alle 30 Sekunden vom Batch-Dienst aufgezeichnet. Allerdings kommt es durch Latenz meist zu einer Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichproben und dem Zeitpunkt, zu dem sie Ihren Formeln für die automatische Skalierung zur Verfügung gestellt (und von diesen gelesen) werden können. Darüber hinaus werden Stichproben gelegentlich aufgrund verschiedener Faktoren wie Netzwerk- oder anderer Infrastrukturprobleme für ein bestimmtes Intervall nicht erfasst. Dies führt zu „fehlenden“ Stichproben.

**Prozentsatz für die Stichprobe**

Beim Übergeben eines `samplePercent`-Werts an die `GetSample()`-Methode oder Aufrufen der `GetSamplePercent()`-Methode bezieht sich „percent“ auf einen Vergleich zwischen der *möglichen* Gesamtanzahl der vom Batch-Dienst erfassten Stichproben und der Anzahl von Stichproben, die tatsächlich für Ihre Formel für die automatische Skalierung *verfügbar* sind.

Lassen Sie uns als Beispiel eine Zeitspanne von 10 Minuten betrachten. Da Stichproben in einer Zeitspanne von 10 Minuten alle 30 Sekunden erfasst werden, werden maximal 20 Stichproben von Batch erfasst (zwei pro Minute). Doch aufgrund der erwähnten Latenz des Berichterstellungsmechanismus oder anderer Probleme in der Azure-Infrastruktur stehen Ihrer Formel für die automatische Skalierung möglicherweise nur 15 Stichproben zur Verfügung. Das bedeutet, dass in diesem zehnminütigen Zeitraum nur **75 Prozent** der Gesamtanzahl erfasster Stichproben tatsächlich für Ihre Formel verfügbar sind.

**GetSample() und Stichprobenbereiche**

Die Formeln für die automatische Skalierung werden Ihre Pools vergrößern und verkleinern und Knoten hinzufügen oder entfernen. Da Knoten Geld Kosten, möchten Sie sicherstellen, dass Ihre Formeln eine sinnvolle Analysemethode verwenden, die auf ausreichend Daten basiert. Aus diesem Grund wird empfohlen, dass Sie eine Analyse des Typs Trend in Ihren Formeln verwenden. Dieser Typ vergrößert oder verkleinert Ihre Pools basierend auf einem *Bereich* gesammelter Stichproben.

Verwenden Sie hierzu `GetSample(interval look-back start, interval look-back end)`, um einen **Vektor** von Stichproben zurückzugeben:

`runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);`

Wenn die obige Zeile von Batch ausgewertet wird, gibt sie einen Bereich von Stichproben als Vektor von Werten zurück. Beispiel:

`runningTasksSample=[1,1,1,1,1,1,1,1,1,1];`

Nachdem Sie den Vektor von Stichproben erfasst haben, können Sie Funktionen wie `min()`, `max()` und `avg()` verwenden, um aussagekräftige Werte aus dem erfassten Bereich abzuleiten.

Zur Erhöhung der Sicherheit können Sie einen *Fehler* bei der Formelauswertung erzwingen, wenn für einen bestimmten Zeitraum weniger als ein bestimmter Prozentsatz von Stichproben verfügbar ist. Mit dem Erzwingen des Fehlschlags einer Formelauswertung weisen Sie Batch an, die weitere Auswertung der Formel zu beenden, sofern der angegebene Prozentsatz von Stichproben nicht zur Verfügung steht. An der Größe des Pools erfolgt dann keinerlei Änderung. Wenn Sie einen erforderlichen Prozentsatz von Stichproben angeben möchten, damit die Auswertung Erfolg hat, geben Sie diesen in `GetSample()` als dritten Parameter an. Hier wird ein Mindestprozentsatz von 75 % angegeben:

`runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);`

Aufgrund der zuvor erwähnten Verzögerung bei der Verfügbarkeit von Stichproben müssen Sie stets einen Zeitraum mit einer Startzeit wählen, die mehr als eine Minute zurückliegt. Der Grund: Es dauert ca. eine Minute, bis Stichproben das System durchlaufen haben, weshalb Stichproben im Bereich `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` häufig nicht verfügbar sind. In diesem Fall können Sie den Prozentsatzparameter `GetSample()` angeben, um einen bestimmten Prozentsatz von Stichproben zu erzwingen.

> [!IMPORTANT]
> Es wird **dringend empfohlen**, sich **in den Formeln für die automatische Skalierung nicht *ausschließlich* auf `GetSample(1)` zu verlassen**. Der Grund ist, dass `GetSample(1)` im Wesentlichen den Batch-Dienst anweist, die letzte vorhandene Stichprobe unabhängig vom Zeitpunkt ihrer Erfassung bereitzustellen. Da es sich nur um ein Stichprobe handelt, die ggf. schon älter ist, ist diese möglicherweise nicht für den aktuellen Aufgaben- oder Ressourcenstatus repräsentativ. Stellen Sie bei Verwendung von `GetSample(1)` sicher, dass dieser Wert zu einer längeren Anweisung gehört und nicht der einzige Datenpunkt ist, auf dem Ihre Formel basiert.
> 
> 

## Metriken
Für das Definieren einer Formel können Sie sowohl **Ressourcenmetriken** als auch **Taskmetriken** verwenden. Sie passen die vorgegebene Anzahl dedizierter Knoten im Pool basierend auf den Metrikdaten an, die Sie abrufen und auswerten. Im Abschnitt [Variablen](#variables) finden Sie weitere Informationen zu den einzelnen Metriken.

<table>
  <tr>
    <th>Metrik</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><b>Ressource</b></td>
    <td><p><b>Ressourcenmetriken</b> basieren sowohl auf der Auslastung von CPU und Arbeitsspeicher und der Bandbreite der Compute-Knoten, als auch auf deren Anzahl.</p>
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
    <td><b>Aufgabe</b></td>
    <td><p><b>Aufgabenmetriken</b> basieren auf dem Aufgabenstatus (wie etwa „Aktiv“, „Ausstehend“ oder „Abgeschlossen“). Folgende vom Dienst definierte Variablen eignen sich für Poolgrößenanpassungen auf der Grundlage von Aufgabenmetriken:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
      <li>$PendingTasks</li>
      <li>$SucceededTasks</li>
            <li>$FailedTasks</li></ul></p>
        </td>
  </tr>
</table>

## Erstellen einer Formel für das automatische Skalieren
Sie können eine Formel für die automatische Skalierung erstellen, indem Sie mithilfe der oben aufgeführten Komponenten Anweisungen formulieren und diese zu einer vollständigen Formel kombinieren. In diesem Beispiel wird eine Formel erstellt, indem wir zunächst die Anforderungen für die Formel wie folgt definieren:

1. Die vorgegebene Anzahl von Computeknoten in einem Pool soll erhöht werden, wenn die CPU-Auslastung hoch ist.
2. Die vorgegebene Anzahl von Computeknoten in einem Pool soll reduziert werden, wenn die CPU-Auslastung gering ist.
3. Die maximale Anzahl von Knoten muss immer auf 400 beschränkt sein.

Zur *Erhöhung* der Knotenanzahl bei hoher CPU-Auslastung definieren wir eine Anweisung, die eine benutzerdefinierte Variable ($TotalNodes) mit einem Wert auffüllt, der 110 Prozent der aktuellen Zielanzahl von Knoten darstellt, wenn die minimale durchschnittliche CPU-Auslastung in den vergangenen 10 Minuten über 70 Prozent lag:

`$TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;`

Die nächste Anweisung legt die gleiche Variable auf 90 Prozent der aktuellen Zielanzahl von Knoten fest, wenn die durchschnittliche CPU-Auslastung in den vergangenen 60 Minuten *unter* 20 Prozent lag. Damit wird die vorgegebene Anzahl von Knoten bei geringer CPU-Auslastung gesenkt. Beachten Sie, dass diese Anweisung ebenfalls auf die benutzerdefinierte Variable *$TotalNodes* aus der obigen Anweisung verweist.

`$TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;`

Beschränken wir nun die vorgegebene Anzahl dedizierter Computeknoten auf **maximal** 400:

`$TargetDedicated = min(400, $TotalNodes)`

Die vollständige Formel lautet:

```
$TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;
$TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute*60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;
$TargetDedicated = min(400, $TotalNodes)
```

> [!NOTE]
> Eine Formel für das automatische Skalieren besteht aus [Batch-REST][rest_api]-API-Variablen, -Typen, -Vorgängen und -Funktionen. Sie können diese auch dann in Formelzeichenfolgen verwenden, wenn Sie mit der [Batch .NET][net_api]-Bibliothek arbeiten.
> 
> 

## Erstellen eines Pools mit aktiviertem automatischen Skalieren
Zum Aktivieren der automatischen Skalierung beim Erstellen eines Pools verwenden Sie eines der folgenden Verfahren:

* [New-AzureBatchPool](https://msdn.microsoft.com/library/azure/mt125936.aspx): Dieses Azure PowerShell-Cmdlet legt die Formel für die automatische Skalierung mit dem AutoScaleFormula-Parameter fest.
* [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx): Nachdem diese .NET-Methode zum Erstellen eines Pools aufgerufen wurde, legen Sie die [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx)-Eigenschaft und die [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx)-Eigenschaft für den Pool fest, um die automatische Skalierung zu aktivieren.
* [Hinzufügen eines Pools zu einem Konto](https://msdn.microsoft.com/library/azure/dn820174.aspx): Das enableAutoScale-Element und das autoScaleFormula-Element werden in dieser REST-API-Anforderung verwendet, um beim Erstellen des Pools die automatische Skalierung für den Pool einzurichten.

> [!IMPORTANT]
> Wenn Sie mithilfe einer der genannten Methoden einen Pool mit aktivierter automatischer Skalierung erstellen, darf der *targetDedicated*-Parameter für den Pool **nicht** angegeben werden. Beachten Sie auch, dass Sie zur manuellen Anpassung der Größe eines Pools mit aktivierter automatischer Skalierung (etwa mit [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]) zunächst die automatische Skalierung **deaktivieren** müssen, um die Größe des Pools ändern zu können.
> 
> 

Der folgende Codeausschnitt zeigt die Erstellung eines Pools mit aktivierter automatischer Skalierung ([CloudPool][net_cloudpool]) mithilfe der [Batch .NET][net_api]-Bibliothek. Die Formel für die automatische Skalierung des Pools legt die vorgegebene Anzahl von Knoten auf 5 am Montag und auf 1 an allen anderen Wochentagen fest. Darüber hinaus wird das Intervall für die automatische Skalierung auf 30 Minuten festgelegt (siehe [Intervall für die automatische Skalierung](#automatic-scaling-interval) weiter unten). In diesem und anderen C#-Ausschnitten in diesem Artikel ist myBatchClient eine ordnungsgemäß initialisierte Instanz von [BatchClient][net_batchclient].

```
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

### Intervall für die automatische Skalierung
Standardmäßig passt der Batch-Dienst die Poolgröße gemäß seiner Formel für die automatische Skalierung alle **15 Minuten** an. Dieses Intervall kann jedoch mithilfe der folgenden Pooleigenschaften konfiguriert werden:

* REST-API: [autoScaleEvaluationInterval][rest_autoscaleinterval]
* .NET-API: [CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval]

Das kürzeste Intervall ist fünf Minuten, das längste 168 Stunden. Wenn ein Intervall außerhalb dieses Bereichs angegeben wird, gibt der Batch-Dienst einen Fehler des Typs „Unzulässige Anforderung (400)“ zurück.

> [!NOTE]
> Die automatische Skalierung ist derzeit nicht als Reaktion im Zeitraum unter einer Minute auf Änderungen vorgesehen, sondern dient eher zum allmählichen Anpassen der Größe Ihres Pools während der Ausführung Ihres Workloads.
> 
> 

## Aktivieren der automatischen Skalierung nach der Erstellung eines Pools
Wenn Sie bereits mithilfe des *targetDedicated*-Parameters einen Pool mit einer festgelegten Anzahl von Computeknoten eingerichtet haben, können Sie den vorhandenen Pool zu einem späteren Zeitpunkt so aktualisieren, dass die Skalierung automatisch erfolgt. Hierzu gehen Sie auf eine der folgenden Arten vor:

* [BatchClient.PoolOperations.EnableAutoScale][net_enableautoscale]\: Diese .NET-Methode erfordert die ID eines vorhandenen Pools und die auf den Pool anzuwendende Formel für die automatische Skalierung.
* [Aktivieren der automatischen Skalierung für einen Pool][rest_enableautoscale]\: Diese REST-API-Anforderung erfordert die ID des vorhandenen Pools im URI und die Formel für die automatische Skalierung im Anforderungstext.

> [!NOTE]
> Wenn beim Erstellen des Pools für den *targetDedicated*-Parameter ein Wert angegeben wurde, wird dieser beim Auswerten der Formel für das automatische Skalieren ignoriert.
> 
> 

Dieser Codeausschnitt zeigt, wie die automatische Skalierung mithilfe der [Batch .NET][net_api]-Bibliothek für einen vorhandenen Pool aktiviert wird. Beachten Sie, dass sowohl für das Aktivieren als auch für das Aktualisieren der Formel für einen vorhandenen Pool dasselbe Verfahren verwendet wird. Wenn das automatische Skalieren bereits aktiviert wurde, wird mithilfe des Verfahrens die Formel für den angegebenen Pool *aktualisiert*. Im Codeausschnitt wird davon ausgegangen, dass „mypool“ die ID eines vorhandenen Pools ([CloudPool][net_cloudpool]) ist.

         // Define the autoscaling formula. In this snippet, the  formula sets the target number of nodes to 5 on
         // Mondays, and 1 on every other day of the week
         string myAutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";

         // Update the existing pool's autoscaling formula by calling the BatchClient.PoolOperations.EnableAutoScale
         // method, passing in both the pool's ID and the new formula.
         myBatchClient.PoolOperations.EnableAutoScale("mypool", myAutoScaleFormula);

## Auswerten der Formel für das automatische Skalieren
Es ist stets empfehlenswert, eine Formel auszuwerten, bevor Sie sie in Ihrer Anwendung verwenden. Die Formel wird ausgewertet, indem Sie einen Testlauf der Formel für einen vorhandenen Pool durchführen. Verwenden Sie hierzu Folgendes:

* [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) oder [BatchClient.PoolOperations.EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx): Diese .NET-Methoden erfordern die ID eines vorhandenen Pools sowie die Zeichenfolge mit der Formel für die automatische Skalierung. Die Ergebnisse des Aufrufs befinden sich in einer Instanz der [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx)-Klasse.
* [Auswerten einer Formel für die automatische Skalierung](https://msdn.microsoft.com/library/azure/dn820183.aspx): In dieser REST-API-Anforderung wird die Pool-ID im URI angegeben. Die Formel für die automatische Skalierung wird im *autoScaleFormula*-Element des Anforderungstexts angegeben. Die bei der Anfrage generierte Antwort enthält Fehlerinformationen, die in Zusammenhang mit der Formel stehen können.

> [!NOTE]
> Damit Sie eine Formel für die automatische Skalierung auswerten können, müssen Sie zuerst die automatische Skalierung für den Pool mithilfe einer gültigen Formel aktivieren.
> 
> 

In diesem Codeausschnitt, in dem die [Batch .NET][net_api]-Bibliothek verwendet wird, werten wir eine Formel aus, bevor diese auf den Pool ([CloudPool][net_cloudpool]) angewendet wird.

```
// First obtain a reference to the existing pool
CloudPool pool = myBatchClient.PoolOperations.GetPool("mypool");

// We must ensure that autoscaling is enabled on the pool prior to evaluating a formula
if (pool.AutoScaleEnabled.HasValue && pool.AutoScaleEnabled.Value)
{
    // The formula to evaluate - adjusts target number of nodes based on day of week and time of day
    string myFormula = @"
        $CurTime=time();
        $WorkHours=$CurTime.hour>=8 && $CurTime.hour<18;
        $IsWeekday=$CurTime.weekday>=1 && $CurTime.weekday<=5;
        $IsWorkingWeekdayHour=$WorkHours && $IsWeekday;
        $TargetDedicated=$IsWorkingWeekdayHour?20:10;
    ";

    // Perform the autoscale formula evaluation. Note that this does not actually apply the formula to
    // the pool.
    AutoScaleEvaluation eval = client.PoolOperations.EvaluateAutoScale(pool.Id, myFormula);

    if (eval.AutoScaleRun.Error == null)
    {
        // Evaluation success - print the results of the AutoScaleRun. This will display the values of each
        // variable as evaluated by the autoscale formula.
        Console.WriteLine("AutoScaleRun.Results: " + eval.AutoScaleRun.Results);

        // Apply the formula to the pool since it evaluated successfully
        client.PoolOperations.EnableAutoScale(pool.Id, myFormula);
    }
    else
    {
        // Evaluation failed, output the message associated with the error
        Console.WriteLine("AutoScaleRun.Error.Message: " + eval.AutoScaleRun.Error.Message);
    }
}
```

Eine erfolgreiche Auswertung der Formel in diesem Codeausschnitt sollte eine Ausgabe ähnlich der folgenden ergeben:

`AutoScaleRun.Results: $TargetDedicated = 10;$NodeDeallocationOption = requeue;$CurTime = 2015 - 08 - 25T20: 08:42.271Z;$IsWeekday = 1;$IsWorkingWeekdayHour = 0;$WorkHours = 0`

## Abrufen von Informationen über Durchläufe des automatischen Skalierens
Überprüfen Sie regelmäßig die Ergebnisse der automatischen Skalierungsläufe, um sicherzustellen, dass die Formel wie erwartet funktioniert.

* [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx): Wenn Sie die .NET-Bibliothek verwenden, stellt diese Eigenschaft eines Pools eine Instanz der [AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.aspx)-Klasse bereit. Diese Klasse stellt die folgenden Eigenschaften für die aktuellste automatische Skalierung bereit:
  * [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)
  * [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
  * [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
* [Abrufen von Informationen zu einem Pool](https://msdn.microsoft.com/library/dn820165.aspx): Diese REST-API-Anforderung gibt Informationen zum Pool zurück, einschließlich der neuesten Ausführung der automatischen Skalierung.

## <a name="examples"></a>Beispielformeln
Die folgenden Beispiele zeigen mehrere Möglichkeiten auf, wie Compute-Ressourcen in einem Pool mithilfe von Formeln automatisch skaliert werden können.

### Beispiel 1: Zeitbasierte Anpassung
Möglicherweise möchten Sie die Größe des Pools basierend auf Wochentag und Uhrzeit anpassen und die Anzahl der Knoten im Pool entsprechend erhöhen oder reduzieren:

```
$CurTime=time();
$WorkHours=$CurTime.hour>=8 && $CurTime.hour<18;
$IsWeekday=$CurTime.weekday>=1 && $CurTime.weekday<=5;
$IsWorkingWeekdayHour=$WorkHours && $IsWeekday;
$TargetDedicated=$IsWorkingWeekdayHour?20:10;
```

Diese Formel ruft zunächst die aktuelle Uhrzeit ab. Wenn es sich um einen Werktag (1 bis 5) handelt und der Wert innerhalb der Geschäftszeiten (8:00 Uhr bis 18:00 Uhr) liegt, wird die Zielgröße des Pools auf 20 Knoten festgelegt. Andernfalls wird die Poolgröße auf 10 Knoten festgelegt.

### Beispiel 2: Aufgabenbasierte Anpassung
In diesem Beispiel wird die Größe des Pools basierend auf der Anzahl der Aufgaben in der Warteschlange angepasst. Beachten Sie, dass in Formelzeichenfolgen sowohl Kommentare als auch Zeilenumbrüche verwendet werden können.

```
// Get pending tasks for the past 15 minutes.
$Samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
// If we have fewer than 70 percent data points, we use the last sample point, otherwise we use the maximum of
// last sample point and the history average.
$Tasks = $Samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1), avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// If number of pending tasks is not 0, set targetVM to pending tasks, otherwise half of current dedicated.
$TargetVMs = $Tasks > 0? $Tasks:max(0, $TargetDedicated/2);
// The pool size is capped at 20, if target VM value is more than that, set it to 20. This value
// should be adjusted according to your use case.
$TargetDedicated = max(0,min($TargetVMs,20));
// Set node deallocation mode - keep nodes active only until tasks finish
$NodeDeallocationOption = taskcompletion;
```

### Beispiel 3: Berücksichtigung paralleler Aufgaben
Auch in diesem Beispiel wird die Poolgröße an die Anzahl der Aufgaben angepasst. Diese Formel berücksichtigt auch den für den Pool festgelegten Wert [MaxTasksPerComputeNode][net_maxtasks]. Dies ist besonders hilfreich, wenn in Ihrem Pool die [parallele Taskausführung](batch-parallel-node-tasks.md) aktiviert wurde.

```
// Determine whether 70 percent of the samples have been recorded in the past 15 minutes; if not, use last sample
$Samples = $ActiveTasks.GetSamplePercent(TimeInterval_Minute * 15);
$Tasks = $Samples < 70 ? max(0,$ActiveTasks.GetSample(1)) : max( $ActiveTasks.GetSample(1),avg($ActiveTasks.GetSample(TimeInterval_Minute * 15)));
// Set the number of nodes to add to one-fourth the number of active tasks (the MaxTasksPerComputeNode
// property on this pool is set to 4, adjust this number for your use case)
$Cores = $TargetDedicated * 4;
$ExtraVMs = (($Tasks - $Cores) + 3) / 4;
$TargetVMs = ($TargetDedicated+$ExtraVMs);
// Attempt to grow the number of compute nodes to match the number of active tasks, with a maximum of 3
$TargetDedicated = max(0,min($TargetVMs,3));
// Keep the nodes active until the tasks finish
$NodeDeallocationOption = taskcompletion;
```

### Beispiel 4: Festlegen einer anfänglichen Poolgröße
Dieses Beispiel zeigt einen C#-Codeausschnitt mit einer Formel für die automatische Skalierung, die die Größe des Pools für einen anfänglichen Zeitraum auf eine bestimmte Anzahl von Knoten festlegt. Dann passt sie die Größe des Pools nach dem ersten Zeitraum basierend auf der Anzahl der ausgeführten und aktive Aufgaben an.

```
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

Die Formel im oben stehenden Codeausschnitt bewirkt Folgendes:

* Die anfängliche Poolgröße wird auf 4 Knoten festgelegt.
* Die Größe des Pools wird innerhalb der ersten 10 Minuten des Lebenszyklus des Pools nicht angepasst.
* Nach 10 Minuten wird die maximale Anzahl ausgeführter und aktiver Aufgaben in den letzten 60 Minuten abgerufen.
  * Falls beide Werte 0 sind (d. h., dass in den letzten 60 Minuten keine Aufgaben ausgeführt wurden oder aktiv waren), wird die Poolgröße auf 0 festgelegt.
  * Wenn einer der Werte größer als null ist, erfolgt keine Änderung.

## Nächste Schritte
* [Maximieren der Azure Batch-Computeressourcenauslastung mit parallelen Knotenaufgaben](batch-parallel-node-tasks.md) enthält Informationen dazu, wie Sie auf den Computeknoten im Pool mehrere Tasks gleichzeitig ausführen können. Zusätzlich zur automatischen Skalierung können Sie mit diesem Feature die Auftragsdauer für einige Workloads verringern und so Geld sparen.
* Um die Effizienz weiter zu steigern, stellen Sie sicher, dass die Batch-Anwendung den Batch-Dienst in optimaler Weise abfragt. In [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md) erfahren Sie, wie Sie die Datenmenge beschränken, die übertragen wird, wenn Sie den Status von Tausenden von Computeknoten oder -tasks abfragen.

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
[rest_autoscaleinterval]: https://msdn.microsoft.com/de-DE/library/azure/dn820173.aspx
[rest_enableautoscale]: https://msdn.microsoft.com/library/azure/dn820173.aspx

<!---HONumber=AcomDC_0928_2016-->