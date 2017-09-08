---
title: Benutzerhandbuch zur Anomalieerkennung in Azure (Vorschauversion) | Microsoft-Dokumentation
description: Verwenden Sie Stream Analytics und Machine Learning, um Anomalien zu erkennen.
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: 7ab489f6ae7da2640ba199b20e7727da60497918
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---

# <a name="using-the-anomalydetection-operator"></a>Verwenden des ANOMALYDETECTION-Operators

> [!IMPORTANT]
> Diese Funktion befindet sich in der Vorschauphase. Die Verwendung in einer Produktionsumgebung wird nicht empfohlen.

Der **ANOMALYDETECTION**-Operator kann zum Erkennen von Anomalien in Ereignisdatenströmen verwendet werden.
Beispielsweise kann eine langsame Abnahme des freien Arbeitsspeichers über einen langen Zeitraum Anzeichen eines Speicherlecks sein, oder die Anzahl der Webdienstanforderungen, die in einem Bereich stabil sind, kann sich möglicherweise dramatisch erhöhen oder verringern.

Er überprüft für die angegebene Dauer, ob die folgenden Typen von Anomalien auftreten:

- Bidirektionale Pegeländerungen
- Langsamer positiver Trend
- Langsamer negativer Trend

Welcher Zeitraum rückwirkend vom aktuellen Ereignis aus überprüft werden muss, wird mit der **LIMIT DURATION**-Klausel festgelegt. **ANOMALYDETECTION** kann optional ausschließlich auf Ereignisse beschränkt werden, die gemäß der **WHEN**-Klausel mit bestimmten Eigenschaften oder Bedingungen übereinstimmen.

Optional können Gruppen von Ereignissen auch basierend auf dem in der **PARTITION BY**-Klausel angegebenen Schlüssel separat verarbeitet werden. Training und Vorhersage treten in jeder Partition unabhängig auf.

## <a name="syntax"></a>Syntax

`ANOMALYDETECTION(\<scalar_expression\>) OVER ([PARTITION BY \<partition key\>] LIMIT DURATION(\<unit\>, \<length\>) [WHEN boolean_expression])` 


## <a name="example-usage"></a>Beispielverwendung

`SELECT id, val, ANOMALYDETECTION(val) OVER(PARTITION BY id LIMIT DURATION(hour, 1) WHEN id \> 100) FROM input`|


## <a name="arguments"></a>Argumente

- **scalar_expression**

  Der skalare Ausdruck, über den die Anomalieerkennung ausgeführt würde. Es ist ein Ausdruck vom Gleitkomma- oder bigint-Typ, der einen einzelnen (skalaren) Wert zurückgibt. Den Platzhalterausdruck **\*** ist nicht zulässig. **scalar_expression** darf keine anderen analytischen oder externen Funktionen enthalten.

- **OVER ( [ partition_by_clause ] limit_duration_clause [when_clause])**

- **partition_by_clause** 

  Die `PARTITION BY \<partition key\>`-Klausel verteilt Lernen und Training über separate Partitionen. Das heißt, ein separates Modell würde gemäß dem Wert von `\<partition key\>` verwendet, und nur Ereignisse mit diesem Wert würden für das Lernen und Trainieren in diesem Modell verwendet. Beispiel:

  `SELECT sensorId, reading, ANOMALYDETECTION(reading) OVER(PARTITION BY sensorId LIMIT DURATION(hour, 1)) FROM input`

  trainiert und bewertet einen Messwert nur im Vergleich zu anderen Messwerten des gleichen Sensors.

- **limit_duration clause** DURATION(\<Einheit\>, \<Länge\>)

  Gibt an, wie viel vom Verlauf ab dem aktuellen Ereignis in der **ANOMALYDETECTION**-Berechnung berücksichtigt wird. Eine ausführliche Beschreibung der unterstützten Einheiten und deren Abkürzungen finden Sie in DATEDIFF.

- **when_clause** 

  Gibt eine boolesche Bedingung für die in der **ANOMALYDETECTION**-Berechnung berücksichtigten Ereignisse an.

## <a name="return-types"></a>Rückgabetypen

Die Funktion gibt einen Datensatz mit allen drei Bewertungen als Ausgabe zurück. Die mit den verschiedenen Typen von Anomaliedetektoren verbundenen Eigenschaften heißen:

- BiLevelChangeScore
- SlowPosTrendScore
- SlowNegTrendScore

Um die einzelnen Werte aus dem Datensatz zu extrahieren, verwenden Sie die **GetRecordPropertyValue** Funktion. Beispiel:

`SELECT id, val FROM input WHERE (GetRecordPropertyValue(ANOMALYDETECTION(val) OVER(LIMIT DURATION(hour, 1)), 'BiLevelChangeScore')) \> 3.25` 


Eine Anomalie eines bestimmten Typs wird erkannt, wenn eine dieser Anomaliebewertungen einen Schwellenwert überschreitet. Der Schwellenwert kann jede Gleitkommazahl \>= 0 sein. Der Schwellenwert ist ein Kompromiss zwischen Empfindlichkeit und Genauigkeit. Beispielsweise wäre die Erkennung bei einem niedrigeren Schwellenwert empfindlicher gegenüber Änderungen und würde mehr Warnungen generieren, während sie bei einem höheren Schwellenwert weniger empfindlich und genauer sein könnte, aber dabei würden einige Anomalien maskiert. Der genaue zu verwendende Schwellenwert hängt vom Szenario ab. Es gibt keine Obergrenze, aber der empfohlene Bereich ist 3.25-5.

## <a name="algorithm"></a>Algorithmus

**ANOMALYDETECTION** nutzt die Semantik des gleitenden Fensters, was bedeutet, dass die Berechnung pro Ereignis ausgeführt wird, das die Funktion eingibt, und eine Bewertung für das Ergebnis produziert wird. Die Berechnung basiert auf Austauschbarkeits-Martingals, die überprüfen, ob sich die Verteilung der Ereigniswerte geändert hat. Wenn dies der Fall ist, wurde eine potenzielle Anomalie erkannt. Die zurückgegebene Bewertung ist ein Hinweis auf den Genauigkeitsgrad dieser Anomalie. Als interne Optimierung berechnet **ANOMALYDETECTION** die Anomaliebewertung eines Ereignisses basierend auf in *d* bis *2d* auftretenden Ereignissen, wobei *d* die angegebene Größe des Erkennungsfensters ist.

**ANOMALYDETECTION** erwartet, dass die Eingabezeitreihen einheitlich sein. Ein Ereignisdatenstrom kann durch Aggregieren über ein rollierendes oder Hopping-Fenster vereinheitlicht werden. In Szenarien, in denen die Lücke zwischen Ereignissen immer kleiner als das Aggregationsfenster ist, reicht ein rollierendes Fenster, um die Zeitreihe zu vereinheitlichen. Wenn die Lücke größer sein kann, können die Lücken durch die Wiederholung des letzten Werts mit einem Hopping-Fenster ausgefüllt werden. Im folgenden Beispiel können beide Szenarien behandelt werden. Derzeit kann der `FillInMissingValuesStep`-Schritt nicht übersprungen werden. Ohne diesen Schritt tritt ein Kompilierungsfehler auf.

## <a name="performance-guidance"></a>Leistungsleitfaden

- Verwenden Sie für Aufträge 6 SU. 
- Senden Sie Ereignisse in einem Abstand von mindestens 1 Sekunde.
- Eine nicht partitionierte, die **ANOMALYDETECTION**-Funktion verwendende Abfrage kann Ergebnisse mit einer durchschnittlichen Berechnungswartezeit von ca. 25 ms erzeugen.
- Die bei einer partitionierten Abfrage auftretende Wartezeit variiert leicht gemäß der Anzahl der Partitionen, da die Anzahl der Berechnungen höher ist. Allerdings ist die Wartezeit für eine vergleichbare Gesamtanzahl der Ereignisse auf allen Partitionen mit der einer nicht partitionierten Abfrage identisch.
- Während des Lesens von Nicht-Echtzeitdaten wird eine große Datenmenge schnell erfasst. Die Verarbeitung dieser Daten ist derzeit erheblich langsamer. Es stellte sich heraus, dass die Wartezeit in einem solchen Szenario linear mit der Anzahl der Datenpunkte im Fenster statt der Fenstergröße oder des Ereignisintervalls an sich anstieg. Um die Wartezeit in Nicht-Echtzeitfällen zu reduzieren, erwägen Sie eine geringere Fenstergröße. Alternativ können Sie erwägen, Ihren Auftrag ab der aktuellen Uhrzeit zu starten. Einige Beispiele für Wartezeiten in einer nicht partitionierten Abfrage: 
    - 60 Datenpunkte im Erkennungsfenster können zu einer Wartezeit von 10 Sekunden mit einem Durchsatz von 3 MBit/s führen. 
    - Bei 600 Datenpunkten kann die Wartezeit ungefähr 80 Sekunden mit einem Durchsatz von 0,4 MBit/s erreichen.

## <a name="example"></a>Beispiel

Mit der folgenden Abfrage kann eine Warnung ausgegeben werden, wenn eine Anomalie erkannt wird.
Wenn der Eingabedatenstrom nicht einheitlich ist, kann der Aggregationsschritt helfen, ihn in eine einheitliche Zeitreihe zu transformieren. Im Beispiel wird **AVG** verwendet, aber der bestimmte Aggregationstyp ist vom Benutzerszenario abhängig. Wenn darüber hinaus die Lücken einer Zeitreihe größer als das Aggregationsfenster sind, lösen keine Ereignisse in der Zeitreihe die Anomalieerkennung aus (in Übereinstimmung mit der Semantik des gleitenden Fensters). Daher wird die Annahme der Einheitlichkeit unterbrochen, wenn das nächste Ereignis eintrifft. In solchen Situationen benötigen wir eine Methode, um die Lücken in der Zeitreihe zu füllen. Ein möglicher Ansatz besteht darin, wie unten dargestellt das letzte Ereignis in jedem Hopping-Fenster zu nehmen.

Wie bereits erwähnt, überspringen Sie jetzt nicht den `FillInMissingValuesStep`-Schritt. Ohne diesen Schritt tritt ein Kompilierungsfehler auf.

    WITH AggregationStep AS 
    (
         SELECT
               System.Timestamp as tumblingWindowEnd,

               AVG(value) as avgValue

         FROM input
         GROUP BY TumblingWindow(second, 5)
    ),

    FillInMissingValuesStep AS
    (
          SELECT
                System.Timestamp AS hoppingWindowEnd,

                TopOne() OVER (ORDER BY tumblingWindowEnd DESC) AS lastEvent

         FROM AggregationStep
         GROUP BY HOPPINGWINDOW(second, 300, 5)

    ),

    AnomalyDetectionStep AS
    (

          SELECT
                hoppingWindowEnd,
                lastEvent.tumblingWindowEnd as lastTumblingWindowEnd,
                lastEvent.avgValue as lastEventAvgValue,
                system.timestamp as anomalyDetectionStepTimestamp,

                ANOMALYDETECTION(lastEvent.avgValue) OVER (LIMIT DURATION(hour, 1)) as
                scores

          FROM FillInMissingValuesStep
    )

    SELECT
          alert = 1,
          hoppingWindowEnd,
          lastTumblingWindowEnd,
          lastEventAvgValue,
          anomalyDetectionStepTimestamp,
          scores

    INTO output

    FROM AnomalyDetectionStep

    WHERE

        CAST(GetRecordPropertyValue(scores, 'BiLevelChangeScore') as float) \>= 3.25

        OR CAST(GetRecordPropertyValue(scores, 'SlowPosTrendScore') as float) \>=
        3.25

       OR CAST(GetRecordPropertyValue(scores, 'SlowNegTrendScore') as float) \>=
       3.25

## <a name="references"></a>Referenzen

* [Anomaly Detection – Using Machine Learning to Detect Abnormalities in Time Series Data](https://blogs.technet.microsoft.com/machinelearning/2014/11/05/anomaly-detection-using-machine-learning-to-detect-abnormalities-in-time-series-data/) (Anomalieerkennung – Verwenden von Machine Learning zum Erkennen von Auffälligkeiten in Zeitreihendaten)
* [Machine Learning Anomaly Detection-API](https://docs.microsoft.com/en-gb/azure/machine-learning/machine-learning-apps-anomaly-detection-api)
* [Time Series Anomaly Detection](https://msdn.microsoft.com/library/azure/mt775197.aspx) (Anomalieerkennung in Zeitreihen)

## <a name="get-support"></a>Support
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte

* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


