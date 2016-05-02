<properties
	pageTitle="Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool | Microsoft Azure"
	description="Aktivieren Sie das automatische Skalieren in einem Cloudpool, um die Anzahl von Computeknoten im Pool dynamisch anzupassen."
	services="batch"
	documentationCenter=""
	authors="mmacy"
	manager="timlt"
	editor="tysonn"/>

<tags
	ms.service="batch"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="vm-windows"
	ms.workload="multiple"
	ms.date="04/18/2016"
	ms.author="marsma"/>

# Automatisches Skalieren von Computeknoten in einem Azure Batch-Pool

Durch automatische Skalierung kann der Azure Batch-Dienst Computeknoten in einem Pool basierend auf von Ihnen definierten Parametern dynamisch hinzufügen oder entfernen. Dadurch können Sie die Menge der Computeressourcen, die von der Anwendung genutzt werden, automatisch anpassen und so möglicherweise Zeit und Geld sparen.

Sie aktivieren die automatische Skalierung für einen Pool von Computeknoten, indem Sie dem Pool eine *Formel für die automatische Skalierung* zuordnen, die Sie z. B. mithilfe der [PoolOperations.EnableAutoScale][net_enableautoscale]-Methode in der [Batch-Bibliothek für .NET](batch-dotnet-get-started.md) definieren. Der Batch-Dienst verwendet dann diese Formel, um die Anzahl von Computeknoten zu bestimmen, die zum Ausführen des Workloads erforderlich sind. Der Batch-Dienst reagiert auf periodisch gesammelte Stichprobenwerten von Dienstmetriken und passt die Anzahl der Computeknoten im Pool mithilfe der zugeordneten Formel in einem konfigurierbaren Intervall an.

Sie können die automatische Skalierung beim Erstellen eines Pools oder später für einen bereits vorhandenen Pool aktivieren. Sie können auch eine vorhandene Formel in einem Pool ändern, für den die „automatische Skalierung“ aktiviert ist. Der Batch-Dienst bietet die Möglichkeit, die Formeln auszuwerten, bevor sie Pools zugewiesen werden, und den Status automatischer Skalierungen zu überwachen.

## Formeln für die automatische Skalierung

Bei einer Formel für die automatische Skalierung handelt es sich um einen von Ihnen definierten Zeichenfolgenwert mit mindestens einer Anweisung, der dem [autoScaleFormula][rest_autoscaleformula]-Element (Batch für REST) eines Pools oder der [CloudPool.AutoScaleFormula][net_cloudpool_autoscaleformula]-Eigenschaft (Batch für .NET) eines Pools zugewiesen ist. Bei der Zuweisung zu einem Pool verwendet der Batch-Dienst Ihre Formel, um für das nächste Verarbeitungsintervall die Zielanzahl der Computeknoten in einem Pool zu bestimmen (mehr zu Intervallen weiter unten). Die Formelzeichenfolge darf eine Größe von 8 KB nicht überschreiten und kann bis zu 100 durch Semikolons getrennte Anweisungen sowie Zeilenumbrüche und Kommentare enthalten.

Sie können sich Formeln für die automatische Skalierung als eine Batch-„Sprache“ für die automatische Skalierung vorstellen. Formel-Anweisungen sind frei gebildete Ausdrücke, die vom System definierte und benutzerdefinierte Variablen sowie Konstanten enthalten können. Mithilfe von mitgelieferten Typen, Operatoren und Funktionen können sie für diese Werte eine Vielzahl von Operationen ausführen. Eine Anweisung kann beispielsweise wie folgt aussehen:

`VAR = Expression(system-defined variables, user-defined variables);`

Formeln enthalten im Allgemeinen mehrere Anweisungen, die Operationen auf Werte anwenden, die in vorherigen Anweisungen abgerufen wurden:

```
VAR₀ = Expression₀(system-defined variables);
VAR₁ = Expression₁(system-defined variables, VAR₀);
```

Mit diesen Anweisungen in Ihrer Formel bestimmen Sie die Anzahl von Computeknoten, auf die der Pool skaliert werden soll, d. h. die **Zielanzahl** an **dedizierten Knoten**. Diese Zahl kann höher, niedriger oder identisch mit der aktuellen Anzahl von Knoten im Pool sein. Batch wertet die Formel für die automatische Skalierung des Pools in einem bestimmten Intervall aus. ([Intervalle für die automatische Skalierung](#automatic-scaling-interval) werden weiter unten erläutert). Anschließend wird die vorgegebene Anzahl von Knoten im Pool auf die Anzahl angepasst, die die Formel für die automatische Skalierung zum Zeitpunkt der Auswertung angibt.

Kurzbeispiel: Diese zweizeilige Formel für die automatische Skalierung gibt an, dass die Anzahl von Knoten gemäß der Anzahl von aktiven Aufgaben bis zu einem Höchstwert von 10 Computeknoten angepasst werden soll.

```
$averageActiveTaskCount = avg($ActiveTasks.GetSample(TimeInterval_Minute * 15));
$TargetDedicated = min(10, $averageActiveTaskCount);
```

In den nächsten Abschnitten dieses Artikels werden die verschiedenen Elemente erläutert, mit denen Ihre Formeln für die automatische Skalierung gebildet werden. Dazu zählen Variablen, Operatoren, Operationen und Funktionen. Sie lernen, wie Sie verschiedene Metriken zu Compute-Ressourcen und Aufgaben in Batch abrufen. Sie können diese Metriken verwenden, um die Knotenanzahl des Pools basierend auf Ressourcenverbrauch und Aufgabenstatus automatisch anzupassen. Anschließend erfahren Sie, wie Sie mit der REST-API und der .NET-API von Batch eine Formel erstellen und die automatische Skalierung für einen Pool aktivieren. Zum Abschluss werden nun verschiedene Beispielformeln gezeigt.

> [AZURE.IMPORTANT] Jedes Azure Batch-Konto ist auf eine bestimmte Anzahl von Computeknoten beschränkt, die für die Verarbeitung verwendet werden können. Der Batch-Dienst wird Knoten nur bis zu dieser Anzahl erstellen. Daher wird die durch die Formel angegebene Zielanzahl möglicherweise nicht erreicht. Unter [Kontingente und Limits für den Azure Batch-Dienst](batch-quota-limit.md) finden Sie Informationen zum Anzeigen und Erhöhen Ihrer Kontokontingente.

## <a name="variables"></a>Variablen

Sie können in einer Formel für die automatische Skalierung sowohl vom System definierte als auch benutzerdefinierte Variablen verwenden. In der obigen zweizeiligen Beispielformel ist `$TargetDedicated` eine vom System definierte Variable, `$averageActiveTaskCount` ist benutzerdefiniert. Die folgenden Tabellen enthalten sowohl Variablen mit Lese-/Schreibzugriff als auch schreibgeschützte Variablen, die vom Batch-Dienst definiert werden.

Mithilfe von *get* und *set* können Sie diese **vom System definierten Variablen** abrufen und festlegen, um die Anzahl von Computeknoten in einem Pool zu verwalten:

<table>
  <tr>
    <th>Variablen (Lese-/Schreibzugriff)</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td>$TargetDedicated</td>
    <td>Die <b>Zielanzahl</b> <b>dedizierter Computeknoten</b> für den Pool. Dies ist die Anzahl von Computeknoten, auf die der Pool skaliert werden soll. Es handelt sich um einen „Zielwert“, da ein Pool die Zielanzahl von Knoten möglicherweise nicht erreicht. Dies kann auftreten, wenn die vorgegebene Anzahl von Knoten erneut durch eine Auswertung der automatischen Skalierung geändert wird, bevor der Pool das ursprüngliche Ziel erreicht hat. Es kann auch vorkommen, wenn ein Batch-Konto-Knoten oder Kern-Kontingent erreicht ist, bevor die vorgegebene Anzahl von Knoten erreicht wird.</td>
  </tr>
  <tr>
    <td>$NodeDeallocationOption</td>
    <td>Dieser Vorgang wird ausgeführt, wenn Computeknoten aus einem Pool entfernt werden. Mögliche Werte:
      <br/>
      <ul>
        <li><p><b>requeue</b>: Aufgaben sofort beenden und wieder in die Auftragswarteschlange einfügen, damit sie neu geplant werden können.</p></li>
        <li><p><b>terminate</b>: Aufgaben sofort beenden und aus der Auftragswarteschlange entfernen.</p></li>
        <li><p><b>taskcompletion</b>: auf derzeit ausgeführte Aufgaben warten und den Knoten dann aus dem Pool entfernen.</p></li>
        <li><p><b>retaineddata</b>: warten, bis alle lokal vorgehaltenen Aufgabendaten des Pools gelöscht wurden, bevor der Knoten aus dem Pool entfernt wird.</p></li>
      </ul></td>
   </tr>
</table>

Sie können die Werte dieser **vom System definierten Variablen** *abrufen*, um basierend auf den Metriken des Batch-Diensts Anpassungen vorzunehmen:

<table>
  <tr>
    <th>Variablen (schreibgeschützt)</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td>$CPUPercent</td>
    <td>Die durchschnittliche prozentuale CPU-Auslastung</td>
  </tr>
  <tr>
    <td>$WallClockSeconds</td>
    <td>Die Anzahl der verbrauchten Sekunden</td>
  </tr>
  <tr>
    <td>$MemoryBytes</td>
    <td>Die durchschnittliche Anzahl genutzter Megabyte</td>
  <tr>
    <td>$DiskBytes</td>
    <td>Die durchschnittliche Anzahl der auf den lokalen Datenträgern genutzten Gigabyte</td>
  </tr>
  <tr>
    <td>$DiskReadBytes</td>
    <td>Die Anzahl der gelesenen Byte</td>
  </tr>
  <tr>
    <td>$DiskWriteBytes</td>
    <td>Die Anzahl der geschriebenen Byte</td>
  </tr>
  <tr>
    <td>$DiskReadOps</td>
    <td>Die Anzahl der ausgeführten Datenträger-Lesevorgänge</td>
  </tr>
  <tr>
    <td>$DiskWriteOps</td>
    <td>Die Anzahl der ausgeführten Datenträger-Schreibvorgänge</td>
  </tr>
  <tr>
    <td>$NetworkInBytes</td>
    <td>Die Anzahl der eingehenden Byte</td>
  </tr>
  <tr>
    <td>$NetworkOutBytes</td>
    <td>Die Anzahl der ausgehenden Byte</td>
  </tr>
  <tr>
    <td>$SampleNodeCount</td>
    <td>Die Anzahl der Computeknoten</td>
  </tr>
  <tr>
    <td>$ActiveTasks</td>
    <td>Die Anzahl der Aufgaben, die sich in einem aktiven Zustand befinden</td>
  </tr>
  <tr>
    <td>$RunningTasks</td>
    <td>Die Anzahl der Aufgaben, die sich in einem Ausführungszustand befinden</td>
  </tr>
  <tr>
    <td>$SucceededTasks</td>
    <td>Die Anzahl der Aufgaben, die erfolgreich abgeschlossen wurden</td>
  </tr>
  <tr>
    <td>$FailedTasks</td>
    <td>Die Anzahl der Aufgaben, bei denen Fehler aufgetreten sind</td>
  </tr>
  <tr>
    <td>$CurrentDedicated</td>
    <td>Die aktuelle Anzahl der zugewiesenen Computeknoten</td>
  </tr>
</table>

> [AZURE.TIP] Die obigen vom System definierten schreibgeschützten Variablen sind *Objekte*, die verschiedene Methoden für den Zugriff auf ihre dazugehörigen Daten bereitstellen. Weitere Informationen finden Sie weiter unten unter [Erfassen von Stichprobendaten](#getsampledata).

## Typen

Folgende **Typen** werden in Formeln unterstützt.

- double
- doubleVec
- doubleVecList
- string
- timestamp – „timestamp“ ist eine Verbundstruktur, die folgende Member enthält:
	- year
	- Monat (1-12)
	- Tag (1-31)
	- weekday (als Zahl, z. B. 1 für Montag)
	- hour (im 24-Stunden-Format, z. B. 13 für 13 Uhr)
	- Minute (00-59)
	- Sekunde (00-59)
- timeInterval
	- TimeInterval\_Zero
	- TimeInterval\_100ns
	- TimeInterval\_Microsecond
	- TimeInterval\_Millisecond
	- TimeInterval\_Second
	- TimeInterval\_Minute
	- TimeInterval\_Hour
	- TimeInterval\_Day
	- TimeInterval\_Week
	- TimeInterval\_Year

## Vorgänge

Folgende **Vorgänge** sind für die oben aufgeführten Typen zulässig.

| Vorgang | Unterstützte Operatoren | Ergebnistyp |
| ------------------------------------- | --------------------- | ------------- |
| double *Operator* double | +, -, *, / | double | | double *Operator* timeinterval | * | timeinterval | | doubleVec *Operator* double | +, -, *, / | doubleVec | | doubleVec *Operator* doubleVec | +, -, *, / | doubleVec | | timeinterval *Operator* double | *, / | timeinterval | | timeinterval *Operator* timeinterval | +, - | timeinterval | | timeinterval *Operator* timestamp | + | timestamp | | timestamp *Operator* timeinterval | + | timestamp | | timestamp *Operator* timestamp | - | timeinterval | | *Operator*double | -, ! | double | | *Operator*timeinterval | - | timeinterval | | double *Operator* double | <, <=, ==, >=, >, != | double | | string *Operator* string | <, <=, ==, >=, >, != | double | | timestamp *Operator* timestamp | <, <=, ==, >=, >, != | double | | timeinterval *Operator* timeinterval | <, <=, ==, >=, >, != | double | | double *Operator* double | &&, || | double |

Beim Testen von „double“ mit einem ternären Operator (`double ? statement1 : statement2`) sind Werte ungleich null **true**, und null ist **false**.

## Funktionen

Folgende vordefinierte **Funktionen** stehen Ihnen zum Definieren einer Formel für die automatische Skalierung zur Verfügung.

| Funktion | Rückgabetyp | Beschreibung
| --------------------------------- | ------------- | --------- |
| avg(doubleVecList) | double | Der Durchschnittswert aller Werte in der doubleVecList wird zurückgegeben.
| len(doubleVecList) | double | Die Länge des Vektors, der aus der doubleVecList erstellt wurde, wird zurückgegeben.
| lg(double) | double | Gibt für den double-Wert den Logarithmus zur Basis 2 zurück.
| lg(doubleVecList) | doubleVec | Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 2 zurück. „vec(double)“ muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen.
| ln(double) | double | Gibt für den double-Wert den natürlichen Logarithmus zurück.
| ln(doubleVecList) | doubleVec | Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 2 zurück. „vec(double)“ muss explizit für den Parameter übergeben werden. Andernfalls wird von der double lg(double)-Version ausgegangen.
| log(double) | double | Gibt für den double-Wert den Logarithmus zur Basis 10 zurück.
| log(doubleVecList) | doubleVec | Gibt für die doubleVecList den komponentenweisen Logarithmus zur Basis 10 zurück. Ein vec(double) muss explizit für einzelne double-Parameter übergeben werden. Andernfalls wird von der double log (double)-Version ausgegangen.
| max(doubleVecList) | double | Der maximale Wert in der doubleVecList wird zurückgegeben.
| min(doubleVecList) | double | Der minimale Wert in der doubleVecList wird zurückgegeben.
| norm(doubleVecList) | double | Die Zweiernorm des Vektors, der aus der doubleVecList erstellt wurde, wird zurückgegeben.
| percentile(doubleVec v, double p) | double | Das Perzentil-Element des Vektors v wird zurückgegeben.
| rand() | double | Ein Zufallswert zwischen 0,0 und 1,0 wird zurückgegeben.
| range(doubleVecList) | double | Der Unterschied zwischen dem Minimal- und Maximalwert in doubleVecList wird zurückgegeben.
| std(doubleVecList) | double | Die Stichproben-Standardabweichung der Werte in der doubleVecList wird zurückgegeben.
| stop() | | Beendet die Auswertung des Ausdrucks für die automatische Skalierung.
| sum(doubleVecList) | double | Die Summe aller Komponenten von doubleVecList wird zurückgegeben.
| time(string dateTime="") | timestamp | Es werden entweder der Zeitstempel der aktuellen Zeit zurückgegeben, wenn keine Parameter übergeben werden, oder andernfalls der Zeitstempel der DateTime-Zeichenfolge, wenn diese übergeben wird. Unterstützte DateTime-Formate sind W3C-DTF und RFC 1123.
| val(doubleVec v, double i) | double | Der Wert des Elements an Position i im Vektor v mit einem Anfangsindex von 0 wird zurückgegeben.

Einige der in der obigen Tabelle beschriebenen Funktionen akzeptieren eine Liste als Argument. Die durch Kommas getrennte Liste ist eine beliebige Kombination aus *double* und *doubleVec*. Beispiel:

`doubleVecList := ( (double | doubleVec)+(, (double | doubleVec) )* )?`

Der *doubleVecList*-Wert wird vor der Auswertung in einen einzelnen *doubleVec* konvertiert. Zum Beispiel hat bei `v = [1,2,3]` das Aufrufen von `avg(v)` denselben Effekt wie das Aufrufen von `avg(1,2,3)`. Das Aufrufen von `avg(v, 7)` entspricht dem Aufrufen von `avg(1,2,3,7)`.

## <a name="getsampledata"></a>Erfassen von Stichprobendaten

Die Formeln für die automatische Skalierung greifen auf Metrikdaten (Stichproben) zurück, die vom Batch-Dienst bereitgestellt werden. Eine Formel vergrößert oder verkleinert den Pool basierend auf den Werten, die sie vom Dienst erhält. Die obigen, vom System definierten Variablen sind Objekte, die verschiedene Methoden für den Zugriff auf die zum jeweiligen Objekt gehörigen Daten bereitstellen. Der folgende Ausdruck zeigt z. B. eine Anforderung zum Abrufen der letzten fünf Minuten der CPU-Auslastung:

`$CPUPercent.GetSample(TimeInterval_Minute * 5)`

<table>
  <tr>
    <th>Methode</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td>GetSample()</td>
    <td><p>Die <b>GetSample()</b>-Methode gibt einen Vektor aus Stichprobenwerten zurück.
	<p>Eine Stichprobe entspricht in 30&#160;Sekunden erfassten Metrikdaten. Dies bedeutet, dass alle 30 Sekunden eine Stichprobe genommen wird. Wie nachstehend erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Daher stehen möglicherweise nicht alle Stichproben für einen bestimmten Zeitraum für die Bewertung durch eine Formel zur Verfügung.
        <ul>
          <li><p><b>doubleVec GetSample(double count)</b>: gibt die Anzahl der Stichproben an, die aus den letzten erfassten Stichproben abgerufen werden soll.</p>
				  <p>„GetSample(1)“ gibt die letzte verfügbare Stichprobe zurück. Für Metriken wie „$CPUPercent“ sollte diese Methode allerdings nicht verwendet werden, da unmöglich feststellbar ist, <em>wann</em> die Stichprobe erfasst wurde.  Sie kann aktuell oder aber, aufgrund von Systemproblemen, auch wesentlich älter sein. Es ist besser, wie unten dargestellt, ein Intervall zu verwenden.</p></li>
          <li><p><b>doubleVec GetSample((timestamp | timeinterval) startTime [, double samplePercent])</b> – legt einen Zeitrahmen für das Sammeln von Stichprobendaten fest. Optional können Sie auch den Prozentsatz der Stichproben angeben, die im angeforderten Zeitraum verfügbar sein müssen.</p>
          <p><em>$CPUPercent.GetSample(TimeInterval\_Minute*10)</em> sollte 20&#160;Stichproben zurückgeben, wenn alle Stichproben der letzten zehn Minuten im Verlauf von „CPUPercent“ vorhanden sind. Wenn jedoch die letzte Minute des Verlaufs nicht verfügbar ist, werden nur 18&#160;Stichproben zurückgegeben. In diesem Fall:<br/>
		  &#160;&#160;&#160;&#160;<em>$CPUPercent.GetSample(TimeInterval_Minute * 10, 95)</em> würde misslingen, da nur 90&#160;% der Stichproben verfügbar sind.<br/>
		  &#160;&#160;&#160;&#160;<em>$CPUPercent.GetSample(TimeInterval_Minute * 10, 80)</em> hätte Erfolg.</p></li>
          <li><p><b>doubleVec GetSample((timestamp | timeinterval) startTime, (timestamp | timeinterval) endTime [, double samplePercent])</b> – legt einen Zeitrahmen für die Datenerfassung mit einer Start- und Endzeit fest.</p></li></ul>
		  <p>Wie bereits erwähnt, gibt es eine Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichprobe und ihrer Verfügbarkeit für eine Formel. Diese Verzögerung muss bei Verwendung der <em>GetSample</em>-Methode berücksichtigt werden. Weitere Informationen finden Sie im nachstehenden Abschnitt <em>GetSamplePercent</em>.</td>
  </tr>
  <tr>
    <td>GetSamplePeriod()</td>
    <td>Gibt den Zeitraum zurück, in dem die Stichproben aus einem alten Stichproben-Dataset gesammelt wurden.</td>
  </tr>
	<tr>
		<td>Count()</td>
		<td>Liefert die Gesamtzahl der Stichprobenwerte im Metrikverlauf zurück.</td>
	</tr>
  <tr>
    <td>HistoryBeginTime()</td>
    <td>Gibt den Zeitstempel des ältesten verfügbaren Stichprobenwerts für die Metrik zurück.</td>
  </tr>
  <tr>
    <td>GetSamplePercent()</td>
    <td><p>Gibt den Prozentsatz an Stichprobenwerten zurück, die für ein bestimmtes Intervall verfügbar sind. Beispiel:</p>
    <p><b>doubleVec GetSamplePercent( (timestamp | timeinterval) startTime [, (timestamp | timeinterval) endTime] )</b>
	<p>Da die GetSample-Methode fehlschlägt, wenn der Prozentsatz der zurückgegebenen Stichproben kleiner als der angeforderte Prozentwert ist, können Sie vorab mithilfe der GetSamplePercent-Methode eine Prüfung vornehmen. Wenn nicht genügend Beispiele vorhanden sind, können Sie anschließend eine andere Aktion ausführen, , ohne die Auswertung der automatischen Skalierung zu unterbrechen.</p></td>
  </tr>
</table>

### Stichproben, Prozentsatz für die Stichprobe und die *GetSample()*-Methode

Das Abrufen von Metrikdaten zu Aufgaben und Ressourcen sowie das anschließende Anpassen der Poolgröße basierend auf diesen Daten ist die Kernfunktion einer Formel für die automatische Skalierung. Daher ist es wichtig, sich damit vertraut zu machen, wie Formeln für die automatische Skalierung mit Metrikdaten bzw. „Stichproben“ interagieren.

**Stichproben**

Der Batch-Dienst nimmt regelmäßig *Stichproben* von Aufgaben- und Ressourcenmetriken und stellt diese Ihren Formeln für die automatische Skalierung zur Verfügung. Diese Stichproben werden alle 30 Sekunden vom Batch-Dienst aufgezeichnet. Allerdings kommt es durch Latenz meist zu einer Verzögerung zwischen dem Zeitpunkt der Erfassung der Stichproben und dem Zeitpunkt, zu dem sie Ihren Formeln für die automatische Skalierung zur Verfügung gestellt (und von diesen gelesen) werden können. Darüber hinaus werden Stichproben gelegentlich aufgrund verschiedener Faktoren wie Netzwerk- oder anderer Infrastrukturprobleme für ein bestimmtes Intervall nicht erfasst. Dies führt zu „fehlenden“ Stichproben.

**Prozentsatz für die Stichprobe**

Beim Übergeben eines `samplePercent`-Werts an die `GetSample()`-Methode oder Aufrufen der `GetSamplePercent()`-Methode bezieht sich „percent“ auf einen Vergleich zwischen der *möglichen* Gesamtanzahl der vom Batch-Dienst erfassten Stichproben und der Anzahl von Stichproben, die tatsächlich für Ihre Formel für die automatische Skalierung *verfügbar* sind.

Lassen Sie uns als Beispiel eine Zeitspanne von 10 Minuten betrachten. Da Stichproben in einer Zeitspanne von 10 Minuten alle 30 Sekunden erfasst werden, werden maximal 20 Stichproben von Batch erfasst (zwei pro Minute). Doch aufgrund der erwähnten Latenz des Berichterstellungsmechanismus oder anderer Probleme in der Azure-Infrastruktur stehen Ihrer Formel für die automatische Skalierung möglicherweise nur 15 Stichproben zur Verfügung. Dies bedeutet, dass in diesem zehnminütigen Zeitraum nur **75 %** der Gesamtanzahl erfasster Stichproben tatsächlich für Ihre Formel verfügbar sind.

**GetSample() und Stichprobenbereiche**

Die Formeln für die automatische Skalierung werden Ihre Pools vergrößern und verkleinern und Knoten hinzufügen oder entfernen. Da Knoten Geld Kosten, möchten Sie sicherstellen, dass Ihre Formeln eine sinnvolle Analysemethode verwenden, die auf ausreichend Daten basiert. Aus diesem Grund wird empfohlen, dass Sie eine Analyse des Typs Trend in Ihren Formeln verwenden. Dieser Typ vergrößert oder verkleinert Ihre Pools basierend auf einem *Bereich* gesammelter Stichproben.

Verwenden Sie hierzu `GetSample(interval look-back start, interval look-back end)`, um einen **Vektor** von Stichproben zurückzugeben:

`runningTasksSample = $RunningTasks.GetSample(1 * TimeInterval_Minute, 6 * TimeInterval_Minute);`

Wenn die obige Zeile von Batch ausgewertet wird, gibt sie einen Bereich von Stichproben als Vektor von Werten zurück. Beispiel:

`runningTasksSample=[1,1,1,1,1,1,1,1,1,1];`

Nachdem Sie den Vektor von Stichproben erfasst haben, können Sie Funktionen wie `min()`, `max()` und `avg()` verwenden, um aussagekräftige Werte aus dem erfassten Bereich abzuleiten.

Für mehr Sicherheit können Sie ein *Fehlschlagen* einer Formelauswertung erzwingen, wenn für einen bestimmten Zeitraum weniger als ein bestimmter Prozentsatz von Stichproben verfügbar ist. Mit dem Erzwingen des Fehlschlags einer Formelauswertung weisen Sie Batch an, die weitere Auswertung der Formel zu beenden, sofern der angegebene Prozentsatz von Stichproben nicht zur Verfügung steht. An der Größe des Pools erfolgt dann keinerlei Änderung. Wenn Sie einen erforderlichen Prozentsatz von Stichproben angeben möchten, damit die Auswertung Erfolg hat, geben Sie diesen in `GetSample()` als dritten Parameter an. Hier wird ein Mindestprozentsatz von 75 % angegeben:

`runningTasksSample = $RunningTasks.GetSample(60 * TimeInterval_Second, 120 * TimeInterval_Second, 75);`

Aufgrund der zuvor erwähnten Verzögerung bei der Verfügbarkeit von Stichproben müssen Sie stets einen Zeitraum mit einer Startzeit wählen, die mehr als eine Minute zurückliegt. Der Grund ist, dass es ca. eine Minute dauert, bis Stichproben das System durchlaufen haben, weshalb Stichproben im Bereich `(0 * TimeInterval_Second, 60 * TimeInterval_Second)` häufig nicht verfügbar sind. In diesem Fall können Sie den Prozentsatzparameter von `GetSample()` angeben, um einen bestimmten Prozentsatz von Stichproben zu erzwingen.

> [AZURE.IMPORTANT] Es wird **dringend empfohlen**, sich **nicht *nur* auf `GetSample(1)` in Ihren Formeln für die automatische Skalierung zu verlassen**. Der Grund ist, dass `GetSample(1)` im Wesentlichen den Batch-Dienst anweist, die letzte vorhandene Stichprobe unabhängig vom Zeitpunkt ihrer Erfassung bereitzustellen. Da es sich nur um ein Stichprobe handelt, die ggf. schon älter ist, ist diese möglicherweise nicht für den aktuellen Aufgaben- oder Ressourcenstatus repräsentativ. Wenn Sie `GetSample(1)` verwenden, stellen Sie sicher, dass dieser Wert zu einer längeren Anweisung gehört und nicht der einzige Datenpunkt ist, auf dem Ihre Formel basiert.

## Metriken

Für das Definieren einer Formel können Sie sowohl **Ressourcenmetriken** als auch **Aufgabenmetriken** verwenden. Sie passen die vorgegebene Anzahl dedizierter Knoten im Pool basierend auf den Metrikdaten an, die Sie abrufen und auswerten. Im Abschnitt [Variablen](#variables) finden Sie weitere Informationen zu den einzelnen Metriken.

<table>
  <tr>
    <th>Metrik</th>
    <th>Beschreibung</th>
  </tr>
  <tr>
    <td><b>Ressource</b></td>
    <td><p><b>Ressourcenmetriken</b> basieren sowohl auf der Auslastung von CPU und Arbeitsspeicher und der Bandbreite der Compute-Knoten, als auch auf deren Anzahl.</p>
		<p> Folgende vom System definierten Variablen eignen sich für Anpassungen basierend auf der Knotenanzahl:</p>
    <p><ul>
      <li>$TargetDedicated</li>
			<li>$CurrentDedicated</li>
			<li>$SampleNodeCount</li>
    </ul></p>
    <p>Diese vom System definierten Variablen eignen sich für Anpassungen basierend auf der Ressourcenverwendung von Knoten:</p>
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
    <td><p><b>Aufgabenmetriken</b> basieren auf dem Aufgabenstatus (wie etwa „Aktiv“, „Ausstehend“ oder „Abgeschlossen“). Folgende vom System definierten Variablen eignen sich für das Anpassen der Poolgröße basierend auf Aufgabenmetriken:</p>
    <p><ul>
      <li>$ActiveTasks</li>
      <li>$RunningTasks</li>
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

Für die *Erhöhung* der Anzahl von Knoten bei hoher CPU-Auslastung definieren wir eine Anweisung, die eine benutzerdefinierte Variable ($TotalNodes) mit einem Wert auffüllt, der 110 % der aktuellen Zielanzahl von Knoten darstellt, wenn die minimale durchschnittliche CPU-Auslastung in den vergangenen 10 Minuten über 70 % lag:

`$TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;`

Die nächste Anweisung legt dieselbe Variable auf 90 % der aktuellen Zielanzahl von Knoten fest, wenn die durchschnittliche CPU-Auslastung in den vergangenen 60 Minuten *unter* 20 % lag. Damit wird die vorgegebene Anzahl von Knoten bei geringer CPU-Auslastung gesenkt. Beachten Sie, dass diese Anweisung ebenfalls auf die benutzerdefinierte Variable *$TotalNodes* aus der obigen Anweisung verweist.

`$TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute * 60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;`

Beschränken wir nun die vorgegebene Anzahl dedizierter Computeknoten auf **maximal** 400:

`$TargetDedicated = min(400, $TotalNodes)`

Die vollständige Formel lautet:

```
$TotalNodes = (min($CPUPercent.GetSample(TimeInterval_Minute*10)) > 0.7) ? ($CurrentDedicated * 1.1) : $CurrentDedicated;
$TotalNodes = (avg($CPUPercent.GetSample(TimeInterval_Minute*60)) < 0.2) ? ($CurrentDedicated * 0.9) : $TotalNodes;
$TargetDedicated = min(400, $TotalNodes)
```

> [AZURE.NOTE] Eine Formel für das automatische Skalieren besteht aus [Batch-REST][rest_api]-API-Variablen, -Typen, -Vorgängen und -Funktionen. Sie können diese auch dann in Formelzeichenfolgen verwenden, wenn Sie mit der [Batch-Bibliothek für .NET][net_api] arbeiten.

## Erstellen eines Pools mit aktiviertem automatischen Skalieren

Zum Aktivieren der automatischen Skalierung beim Erstellen eines Pools verwenden Sie eines der folgenden Verfahren:

- [New-AzureBatchPool](https://msdn.microsoft.com/library/azure/mt125936.aspx): Dieses Azure PowerShell-Cmdlet legt die Formel für die automatische Skalierung mit dem AutoScaleFormula-Parameter fest.
- [BatchClient.PoolOperations.CreatePool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx): Nachdem diese .NET-Methode zum Erstellen eines Pools aufgerufen wurde, legen Sie die [CloudPool.AutoScaleEnabled](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleenabled.aspx)-Eigenschaft und die [CloudPool.AutoScaleFormula](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscaleformula.aspx)-Eigenschaft für den Pool fest, um die automatische Skalierung zu aktivieren.
- [Hinzufügen eines Pools zu einem Konto](https://msdn.microsoft.com/library/azure/dn820174.aspx): Das enableAutoScale-Element und das autoScaleFormula-Element werden in dieser REST-API-Anforderung verwendet, um beim Erstellen des Pools die automatische Skalierung für den Pool einzurichten.

> [AZURE.IMPORTANT] Wenn Sie mithilfe einer der genannten Methoden einen für die automatische Skalierung aktivierten Pool erstellen, darf der *targetDedicated*-Parameter für den Pool **nicht** angegeben werden. Beachten Sie auch, dass Sie für eine manuelle Anpassung der Größe eines für die automatische Skalierung aktivierten Pools (z. B. mit [BatchClient.PoolOperations.ResizePool][net_poolops_resizepool]) zunächst die automatische Skalierung **deaktivieren** müssen, bevor Sie die Größe des Pools ändern können.

Der folgende Codeausschnitt zeigt die Erstellung eines Pools mit aktivierter automatischer Skalierung ([CloudPool][net_cloudpool]) mithilfe der [Batch-Bibliothek für .NET][net_api]. Die Formel für die automatische Skalierung des Pools legt die vorgegebene Anzahl von Knoten auf 5 am Montag und auf 1 an allen anderen Wochentagen fest. Darüber hinaus wird das Intervall für die automatische Skalierung auf 30 Minuten festgelegt (siehe [Intervall für die automatische Skalierung](#automatic-scaling-interval) weiter unten). In diesem und anderen C#-Ausschnitten in diesem Artikel ist „myBatchClient“ eine ordnungsgemäß initialisierte Instanz von [BatchClient][net_batchclient].

```
CloudPool pool = myBatchClient.PoolOperations.CreatePool("mypool", "3", "small");
pool.AutoScaleEnabled = true;
pool.AutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";
pool.AutoScaleEvaluationInterval = TimeSpan.FromMinutes(30);
pool.Commit();
```

### Intervall für die automatische Skalierung

Standardmäßig passt der Batch-Dienst die Poolgröße gemäß seiner Formel für die automatische Skalierung alle **15 Minuten** an. Dieses Intervall kann jedoch mithilfe der folgenden Pooleigenschaften konfiguriert werden:

- REST-API – [autoScaleEvaluationInterval][rest_autoscaleinterval]
- .NET-API – [CloudPool.AutoScaleEvaluationInterval][net_cloudpool_autoscaleevalinterval]

Das kürzeste Intervall ist fünf Minuten, das längste 168 Stunden. Wenn ein Intervall außerhalb dieses Bereichs angegeben wird, gibt der Batch-Dienst einen Fehler des Typs „Unzulässige Anforderung (400)“ zurück.

> [AZURE.NOTE] Die automatische Skalierung ist derzeit nicht als Reaktion im Zeitraum unter einer Minute auf Änderungen vorgesehen, sondern dient eher zum allmählichen Anpassen der Größe Ihres Pools während der Ausführung Ihres Workloads.

## Aktivieren der automatischen Skalierung nach der Erstellung eines Pools

Wenn Sie bereits anhand des *targetDedicated*-Parameters einen Pool mit einer festgelegten Anzahl von Computeknoten eingerichtet haben, können Sie den vorhandenen Pool zu einem späteren Zeitpunkt so aktualisieren, dass automatisch skaliert wird. Hierzu gehen Sie auf eine der folgenden Arten vor:

- [BatchClient.PoolOperations.EnableAutoScale][net_enableautoscale]\: Diese .NET-Methode erfordert die ID eines vorhandenen Pools und die auf den Pool anzuwendende Formel für die automatische Skalierung.
- [Aktivieren der automatischen Skalierung für einen Pool][rest_enableautoscale]\: Diese REST-API-Anforderung erfordert die ID des vorhandenen Pools im URI und die Formel für die automatische Skalierung im Anforderungstext.

> [AZURE.NOTE] Wenn beim Erstellen des Pools für den *targetDedicated*-Parameter ein Wert angegeben wurde, wird dieser beim Auswerten der Formel für das automatische Skalieren ignoriert.

Dieser Codeausschnitt zeigt, wie die automatische Skalierung mithilfe der [Batch-Bibliothek für .NET][net_api] für einen vorhandenen Pool aktiviert wird. Beachten Sie, dass sowohl für das Aktivieren als auch für das Aktualisieren der Formel für einen vorhandenen Pool dasselbe Verfahren verwendet wird. Wenn das automatische Skalieren bereits aktiviert wurde, wird mithilfe des Verfahrens die Formel für den angegebenen Pool *aktualisiert*. Im Codeausschnitt wird angenommen, dass „mypool“ die ID eines vorhandenen Pools ([CloudPool][net_cloudpool]) ist.

		 // Define the autoscaling formula. In this snippet, the  formula sets the target number of nodes to 5 on
		 // Mondays, and 1 on every other day of the week
		 string myAutoScaleFormula = "$TargetDedicated = (time().weekday==1?5:1);";

		 // Update the existing pool's autoscaling formula by calling the BatchClient.PoolOperations.EnableAutoScale
		 // method, passing in both the pool's ID and the new formula.
		 myBatchClient.PoolOperations.EnableAutoScale("mypool", myAutoScaleFormula);

## Auswerten der Formel für das automatische Skalieren

Es ist stets empfehlenswert, eine Formel auszuwerten, bevor Sie sie in Ihrer Anwendung verwenden. Die Formel wird ausgewertet, indem Sie einen Testlauf der Formel für einen vorhandenen Pool durchführen. Verwenden Sie hierzu Folgendes:

- [BatchClient.PoolOperations.EvaluateAutoScale](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscale.aspx) oder [BatchClient.PoolOperations.EvaluateAutoScaleAsync](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.evaluateautoscaleasync.aspx): Diese .NET-Methoden erfordern die ID eines vorhandenen Pools sowie die Zeichenfolge mit der Formel für die automatische Skalierung. Die Ergebnisse des Aufrufs befinden sich in einer Instanz der [AutoScaleEvaluation](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscaleevaluation.aspx)-Klasse.
- [Auswerten einer Formel für die automatische Skalierung](https://msdn.microsoft.com/library/azure/dn820183.aspx): In dieser REST-API-Anforderung ist die Pool-ID im URI angegeben. Die Formel für die automatische Skalierung wird im *autoScaleFormula*-Element des Anforderungstexts angeben. Die bei der Anfrage generierte Antwort enthält Fehlerinformationen, die in Zusammenhang mit der Formel stehen können.

> [AZURE.NOTE] Damit Sie eine Formel für die automatische Skalierung auswerten können, müssen Sie zuerst die automatische Skalierung für den Pool mithilfe einer gültigen Formel aktivieren.

In diesem Codeausschnitt, der die [Batch-Bibliothek für .NET][net_api] verwendet, werten wir eine Formel aus, bevor diese auf den Pool ([CloudPool][net_cloudpool]) angewendet wird.

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

- [CloudPool.AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.autoscalerun.aspx): Wenn Sie die .NET-Bibliothek verwenden, stellt diese Eigenschaft eines Pools eine Instanz der [AutoScaleRun](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.aspx)-Klasse bereit. Diese Klasse stellt die folgenden Eigenschaften für die aktuellste automatische Skalierung bereit:
  - [AutoScaleRun.Error](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.error.aspx)
  - [AutoScaleRun.Results](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.results.aspx)
  - [AutoScaleRun.Timestamp](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.autoscalerun.timestamp.aspx)
- [Abrufen von Informationen zu einem Pool](https://msdn.microsoft.com/library/dn820165.aspx): Diese REST-API-Anforderung gibt Informationen zum Pool zurück, einschließlich des neuesten Durchlaufs der automatischen Skalierung.

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

Diese Formel ruft zunächst die aktuelle Uhrzeit ab. Wenn es sich um einen Werktag (1 bis 5) handelt und der Wert innerhalb der Geschäftszeiten (8:00 Uhr bis 18:00 Uhr) liegt, wird die Zielgröße des Pools auf 20 Knoten festgelegt. Andernfalls wird die Poolgröße auf 10 Knoten festgelegt.

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

Auch in diesem Beispiel wird die Poolgröße an die Anzahl der Aufgaben angepasst. Diese Formel berücksichtigt auch den Wert [MaxTasksPerComputeNode][net_maxtasks], der für den Pool festgelegt wurde. Dies ist besonders nützlich in Situationen, in denen eine [parallele Ausführung von Aufgaben](batch-parallel-node-tasks.md) in Ihrem Pool aktiviert wurde.

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

- Die anfängliche Poolgröße wird auf 4 Knoten festgelegt.
- Die Größe des Pools wird innerhalb der ersten 10 Minuten des Lebenszyklus des Pools nicht angepasst.
- Nach 10 Minuten wird die maximale Anzahl ausgeführter und aktiver Aufgaben in den letzten 60 Minuten abgerufen.
  - Falls beide Werte 0 sind (d. h., dass in den letzten 60 Minuten keine Aufgaben ausgeführt wurden oder aktiv waren), wird die Poolgröße auf 0 festgelegt.
  - Wenn einer der Werte größer als null ist, erfolgt keine Änderung.

## Nächste Schritte

* [Maximieren der Azure Batch-Computeressourcenauslastung mit parallelen Knotenaufgaben](batch-parallel-node-tasks.md) enthält Informationen dazu, wie Sie mehrere Aufgaben gleichzeitig auf den Computeknoten im Pool ausführen können. Zusätzlich zur automatischen Skalierung können Sie mit diesem Feature die Auftragsdauer für einige Workloads verringern und so Geld sparen.

* Um die Effizienz weiter zu steigern, stellen Sie sicher, dass die Batch-Anwendung den Batch-Dienst in optimaler Weise abfragt. In [Effizientes Abfragen des Azure Batch-Diensts](batch-efficient-list-queries.md) erfahren Sie, wie Sie die Datenmenge beschränken, die übertragen wird, wenn Sie den Status von Tausenden von Computeknoten oder -aufgaben abfragen.

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

<!---HONumber=AcomDC_0420_2016-->