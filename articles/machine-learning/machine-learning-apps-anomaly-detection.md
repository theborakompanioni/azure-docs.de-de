<properties 
	pageTitle="Machine Learning-App: Anomaly Detection Service | Microsoft Azure" 
	description="Die Anomaly Detection-API ist ein mit Microsoft Azure Machine Learning erstelltes Beispiel, das Anomalien in Zeitreihendaten erkennt, wenn die numerischen Daten zeitlich gleich verteilt sind." 
	services="machine-learning" 
	documentationCenter="" 
	authors="alokkirpal" 
	manager="paulettm"
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.devlang="na" 
	ms.topic="reference" 
	ms.tgt_pltfrm="na" 
	ms.workload="multiple" 
	ms.date="06/29/2016" 
	ms.author="alokkirpal"/>


# Machine Learning Anomaly Detection Service#

##Übersicht

Die Anomaly Detection-API ist ein mit Azure Machine Learning erstelltes Beispiel, das Anomalien in Zeitreihendaten erkennt, wenn die numerischen Daten zeitlich gleich verteilt sind.

Diese APIs können die folgenden Arten von anomalen Mustern in Zeitreihendaten erkennen:

* **Positive und negative Trends**: Bei der Überwachung der Arbeitsspeichernutzung kann ein Aufwärtstrend in Bezug auf das Computing interessant sein, da dies ein Hinweis auf einen Speicherverlust sein kann.

* **Änderungen im dynamischen Wertebereich**: Bei der Überwachung der durch einen Clouddienst ausgelösten Ausnahmen können Änderungen im dynamischen Wertebereich z.B. auf eine Instabilität der Dienstintegrität hinweisen.

* **Spitzen und Abfälle**: Beim Überwachen der Anzahl von fehlerhaften Anmeldeversuchen bei einem Dienst oder der Anzahl von Auscheckvorgängen in einer E-Commerce-Website könnten Spitzen oder Abfälle z.B. auf anormales Verhalten hinweisen.

Diese Machine Learning-Erkennungselemente verfolgen diese Art von Wertänderungen in Abhängigkeit der Zeit, und fortlaufende Änderungen der Werte werden als Anomaliebewertungen gemeldet. Sie benötigen keine Ad-hoc-Schwellenwertanpassung, und ihre Bewertungen können verwendet werden, um falsch positive Raten zu steuern. Die Anomaly Detection-API ist beispielsweise in den folgenden Szenarios nützlich: Dienstüberwachung durch Nachverfolgen von KPIs in Abhängigkeit der Zeit, Nutzungsüberwachung anhand von Metriken wie der Anzahl von Suchvorgängen oder der Anzahl von Klicks, Leistungsüberwachung anhand von Indikatoren wie Arbeitsspeicher, CPU, Dateilesevorgänge usw. in Abhängigkeit der Zeit.

Das Anomaly Detection-Angebot verfügt über nützliche Tools für die ersten Schritte.
* Mit der [Webanwendung](http://anomalydetection-aml.azurewebsites.net/) können Sie die Ergebnisse von Anomaly Detection-APIs für Ihre Daten auswerten und visualisieren.
* Der [Beispielcode](http://adresultparser.codeplex.com/) veranschaulicht, wie Sie programmgesteuert auf die API zugreifen und die Ergebnisse in C# analysieren.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


##API-Definition

Der Dienst stellt eine REST-basierte API über HTTPS bereit, die auf unterschiedliche Weise genutzt werden kann, beispielsweise durch eine Web- oder mobile Anwendung, T, Python, Excel usw. Sie senden Zeitreihendaten über einen REST-API-Aufruf an diesen Dienst, und dann wird eine Kombination der oben beschriebenen drei Anomaliearten ausgeführt. Der Dienst wird auf der Azure Machine Learning-Plattform ausgeführt, die nahtlos für Ihre geschäftlichen Anforderungen skaliert werden kann und über SLAs verfügt.

###Headers
Stellen Sie sicher, dass Sie die richtigen Header in Ihre Anforderung einbeziehen, was so erfolgen muss:

	Authorization: Basic <creds>
	Accept: application/json
               
	Where <creds> = ConvertToBase64(“AccountKey:” + yourActualAccountKey);  

Sie finden den Schlüssel Ihres Kontos in [Azure Data Market](https://datamarket.azure.com/account/keys).

###Score-API

Die Score-API wird zum Ausführen der Anomaly Detection (Anomalieerkennung) für nicht saisonabhängige Zeitreihendaten verwendet. Die API führt mehrere Anomalieerkennungsmodule für die Daten aus und gibt jeweils die Anomaliebewertungen zurück. Die folgende Abbildung enthält ein Beispiel für Anomalien, die von der Score-API erkannt werden können. Diese Zeitreihe weist zwei unterschiedliche Pegeländerungen und drei Spitzen auf. Die roten Punkte zeigen die Zeit an, zu der die Pegeländerung erkannt wird, während die schwarzen Punkte die erkannten Spitzen anzeigen.

![Score-API][1]
	
**URL**

	https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/Score 

**Beispiel für Anforderungstext**

In der Anforderung unten senden wir eine Zeitreihe (hier abgeschnitten) mit Datenpunkten vom 01.03.2016 bis zum 10.03.2016 und Parametern der Spitzenerkennungsmodule an die API, um zu ermitteln, ob anormale Datenpunkte darunter sind.

	{
	  "data": [
	    [ "9/21/2014 11:05:00 AM", "3" ],
	    [ "9/21/2014 11:10:00 AM", "9.09" ],
	    [ "9/21/2014 11:15:00 AM", "0" ]
	  ],
	  "params": {
	    "tspikedetector.sensitivity": "3",
	    "zspikedetector.sensitivity": "3",
	    "trenddetector.sensitivity": "3.25",
	    "bileveldetector.sensitivity": "3.25",
	    "postprocess.tailRows": "2"
	  }
	}

Die Antwort lautet wie folgt:

	{
	  "odata.metadata":"https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
	  "ADOutput":"{
	    "ColumnNames":["Time","Data","TSpike","ZSpike","rpscore","rpalert","tscore","talert"],
		"ColumnTypes":["DateTime","Double","Double","Double","Double","Int32","Double","Int32"],
		"Values":[
		  ["9/21/2014 11:10:00 AM","9.09","0","0","-1.07030497733224","0","-0.884548154298423","0"],
		  ["9/21/2014 11:15:00 AM","0","0","0","-1.05186237440962","0","-1.173800281031","0"]
		]
	  }"
	}

###ScoreWithSeasonality-API

Die ScoreWithSeasonality-API wird zum Durchführen der Anomalieerkennung für Zeitreihen verwendet, die über saisonale Muster verfügen. Diese API ist nützlich, um Abweichungen in saisonalen Mustern zu erkennen.

Die folgende Abbildung enthält ein Beispiel für Anomalien, die in einer saisonalen Zeitreihe erkannt wurden. Die Zeitreihe weist eine Spitze (erster schwarzer Punkt), zwei Abfälle (zweiter schwarzer Punkt und ein Punkt am Ende) und eine Pegeländerung (roter Punkt) auf. Beachten Sie, dass sowohl der Abfall in der Mitte der Zeitreihe als auch die Pegeländerung erst erkennbar sind, nachdem die saisonalen Komponenten aus der Zeitreihe entfernt wurden.

![Seasonality-API][2]

**URL**

	https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/ScoreWithSeasonality 

**Beispiel für Anforderungstext**

In der Anforderung unten senden wir eine Zeitreihe (hier abgeschnitten) mit Datenpunkten vom 01.03.2016 bis zum 10.03.2016 und Parametern an die API, um zu ermitteln, ob anormale Datenpunkte darunter sind.

	{
	  "data": [
	    [ "9/21/2014 11:10:00 AM", "9" ],
	    [ "9/21/2014 11:15:00 AM", "12" ],
	    [ "9/21/2014 11:20:00 AM", "10" ]
	  ],
	  "params": {
	    "preprocess.aggregationInterval": "0",
	    "preprocess.aggregationFunc": "mean",
	    "preprocess.replaceMissing": "lkv",
	    "postprocess.tailRows": "1",
	    "zspikedetector.sensitivity": "3",
	    "tspikedetector.sensitivity": "3",
	    "upleveldetector.sensitivity": "3.25",
	    "bileveldetector.sensitivity": "3.25",
	    "trenddetector.sensitivity": "3.25",
	    "detectors.historywindow": "500",
	    "seasonality.enable": "true",
	    "seasonality.transform": "deseason",
	    "seasonality.numSeasonality": "1"
	  }
	}

Die Antwort lautet wie folgt:

	{
		"odata.metadata": "https://api.datamarket.azure.com/data.ashx/aml_labs/anomalydetection/v2/$metadata#AnomalyDetection.FrontEndService.Models.AnomalyDetectionResult",
		"ADOutput": "{
			"ColumnNames":["Time","OriginalData","ProcessedData","TSpike","ZSpike","PScore","PAlert","RPScore","RPAlert","TScore","TAlert"],
		  	"ColumnTypes":["DateTime","Double","Double","Double","Double","Double","Int32","Double","Int32","Double","Int32"],
		  	"Values":[
		    	["9/21/201411: 20: 00AM","10","10","0","0","-1.30229513613974","0","-1.30229513613974","0","-1.173800281031","0"]
		  	]
		}"
	}

###Erkennungsmodule

Die Anomaly Detection-API unterstützt Erkennungsmodule in drei allgemeinen Kategorien. Details zu bestimmten Eingabeparametern und Ausgaben für jedes Erkennungsmodul finden Sie in der folgenden Tabelle.

|Kategorie des Erkennungsmoduls|Erkennungsmodul|Beschreibung|Eingabeparameter|Ausgaben
|---|---|---|---|---|
|Spitzenerkennungsmodule|TSpike-Erkennungsmodul|Erkennen von Spitzen und Abfällen anhand der festgelegten Empfindlichkeit|*tspikedetector.sensitivity:* Verwendet einen Ganzzahlwert zwischen 1 und 10. Der Standardwert ist 3. Je höher der Wert, desto geringer die Empfindlichkeit.|TSpike: Binärwerte – „1“, wenn eine Spitze oder ein Abfall erkannt wird, andernfalls „0“.|
|ZSpike-Erkennungsmodul|Erkennen von Spitzen und Abfällen anhand der festgelegten Empfindlichkeit|*zspikedetector.sensitivity:* Verwendet einen Ganzzahlwert zwischen 1 und 10. Der Standardwert ist 3. Je höher der Wert, desto geringer die Empfindlichkeit.|ZSpike: Binärwerte – „1“, wenn eine Spitze oder ein Abfall erkannt wird, andernfalls „0“.|
|Erkennungsmodul für langsame Trends|Erkennungsmodul für langsame Trends|Erkennen von langsamen positiven Trends anhand der festgelegten Empfindlichkeit|*trenddetector.sensitivity:* Schwellenwert der Erkennungsmodulbewertung (Standard: 3,25, 3,25 - 5 ist ein geeigneter Bereich. Je höher der Wert, desto geringer die Empfindlichkeit.)|tscore: Gleitzahl, die für die Anomaliebewertung des Trends steht.|
|Erkennungsmodule für Pegeländerungen|Erkennungsmodul für unidirektionale Pegeländerungen|Erkennen einer Pegeländerung in Aufwärtsrichtung anhand der festgelegten Empfindlichkeit|Identisch mit Erkennungsmodul für langsame Trends|pscore: Gleitzahl, die für die Anomaliebewertung von Pegeländerungen in Aufwärtsrichtung steht.|
|Erkennungsmodul für bidirektionale Pegeländerungen|Erkennen von Pegeländerungen in Aufwärts- und Abwärtsrichtung anhand der festgelegten Empfindlichkeit|Identisch mit Erkennungsmodul für langsame Trends|rpscore: Gleitzahl, die für die Anomaliebewertung von Pegeländerungen in Aufwärts- und Abwärtsrichtung steht.

###Parameter

Weitere ausführliche Informationen zu diesen Eingabeparametern sind in der folgenden Tabelle aufgeführt:

|Eingabeparameter|Beschreibung|Standardeinstellung|Typ|Gültiger Bereich|Vorgeschlagener Bereich|
|---|---|---|---|---|---|
|preprocess.aggregationInterval|Aggregationsintervall in Sekunden zum Aggregieren von Eingabezeitreihen|0 (keine Aggregation)|integer|0: Aggregation überspringen, andernfalls > 0|5 Minuten bis 1 Tag, von Zeitreihe abhängig
|preprocess.aggregationFunc|Funktion zum Aggregieren von Daten im angegebenen AggregationInterval|mean|enumerated|mean, sum, length|–|
|preprocess.replaceMissing|Werte zum Zuordnen fehlender Daten|lkv (last known value)|enumerated|zero, lkv, mean|N/V|
|detectors.historyWindow|Verwendeter Verlauf (in Anzahl von Datenpunkten) für die Anomalieberechnung|500|integer|10 - 2.000|Von Zeitreihe abhängig|
|upleveldetector.sensitivity|Empfindlichkeit für Erkennungsmodul für Pegeländerungen in Aufwärtsrichtung |3\.25|double|Keine|3,25 - 5 (niedrigerer Wert = höhere Empfindlichkeit)|
|bileveldetector.sensitivity|Empfindlichkeit für Erkennungsmodul für bidirektionale Pegeländerungen |3\.25|double|Keine|3,25 - 5 (niedrigerer Wert = höhere Empfindlichkeit)|
|trenddetector.sensitivity|Empfindlichkeit für Erkennungsmodul für positive Trends |3\.25|double|Keine|3,25 - 5 (niedrigerer Wert = höhere Empfindlichkeit)|
|tspikedetector.sensitivity |Empfindlichkeit für TSpike-Erkennungsmodul|3|integer|1 - 10|3 - 5 (niedrigerer Wert = höhere Empfindlichkeit)|
|zspikedetector.sensitivity |Empfindlichkeit für ZSpike-Erkennungsmodul|3|integer|1 - 10 |3 - 5 (niedrigerer Wert = höhere Empfindlichkeit)|
|seasonality.enable|Gibt an, ob eine saisonabhängige Analyse durchgeführt wird.|true|boolean|true, false|Von Zeitreihe abhängig|
|seasonality.numSeasonality |Maximale Anzahl von zu erkennenden periodischen Zyklen|1|integer|1, 2|1 - 2|
|seasonality.transform |Gibt an, ob saisonale (und) Trendkomponenten entfernt werden sollen, bevor die Anomalieerkennung angewendet wird.|deseason|enumerated|none, deseason, deseasontrend|–|
|postprocess.tailRows |Anzahl von aktuellen Datenpunkten, die in den Ausgabeergebnissen beibehalten werden sollen|0|integer|0 (alle Datenpunkte beibehalten), oder geben Sie die Anzahl von Punkten an, die in den Ergebnissen beibehalten werden sollen|–|

###Ausgabe
Die API führt alle Erkennungsmodule für Ihre Zeitreihendaten aus und gibt für jeden Zeitpunkt Anomaliebewertungen und binäre Spitzenindikatoren zurück. In der Tabelle unten sind die Ausgaben der API aufgeführt.

|Ausgaben|Beschreibung|
|---|---|
|Time|Zeitstempel aus Rohdaten oder aggregierte (und/oder) zugeordnete Daten, wenn Aggregation (und/oder) die Zuordnung fehlender Daten angewendet wird|
|OriginalData|Werte aus Rohdaten, oder aggregierte (und/oder) zugeordnete Daten, wenn Aggregation (und/oder) die Zuordnung fehlender Daten angewendet wird|
|ProcessedData|Eine der beiden folgenden Möglichkeiten: <ul><li>Saisonabhängig angepasste Zeitreihen, wenn eine signifikante Saisonabhängigkeit erkannt und die Option „deseason“ gewählt wurde</li><li>Saisonabhängig angepasste und trendbereinigte Zeitreihen, wenn eine signifikante Saisonabhängigkeit erkannt und die Option „deseasontrend“ gewählt wurde</li><li>Andernfalls identisch mit OriginalData</li>|
|TSpike|Binärer Indikator, um anzugeben, ob eine Spitze vom TSpike-Erkennungsmodul erkannt wird|
|ZSpike|Binärer Indikator, um anzugeben, ob eine Spitze vom ZSpike-Erkennungsmodul erkannt wird|
|Pscore|Gleitzahl, die für die Anomaliebewertung von Pegeländerungen in Aufwärtsrichtung steht|
|Palert|1/0-Wert, der basierend auf der Eingabeempfindlichkeit angibt, dass eine Pegeländerung in Aufwärtsrichtung vorhanden ist|
|RPScore|Gleitzahl, die für die Anomaliebewertung von bidirektionalen Pegeländerungen steht|
|RPAlert|1/0-Wert, der basierend auf der Eingabeempfindlichkeit angibt, dass eine bidirektionale Pegeländerung vorhanden ist|
|TScore|Gleitzahl, die für die Anomaliebewertung positiver Trends steht|
|TAlert|1/0-Wert, der basierend auf der Eingabeempfindlichkeit angibt, dass eine positive Trendanomalie vorhanden ist|


Diese Ausgabe kann mit einem [einfachen Parser](https://adresultparser.codeplex.com/) analysiert werden. Es wird Beispielcode bereitgestellt, der veranschaulicht, wie eine Verbindung mit der API hergestellt und die Ausgabe analysiert wird. Die erkannten Anomalien können in einem Dashboard visualisiert und/oder an menschliche Experten übergeben werden, um Korrekturmaßnahmen zu ergreifen oder sie in Ticketsysteme zu integrieren.


[1]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-score.png
[2]: ./media/machine-learning-apps-anomaly-detection/anomaly-detection-seasonal.png

 

 

<!---HONumber=AcomDC_0629_2016-->