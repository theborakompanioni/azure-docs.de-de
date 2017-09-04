---
title: "Abfragebeispiele für gängige Verwendungsmuster in Stream Analytics | Microsoft Docs"
description: Allgemeine Abfragemuster in Azure Stream Analytics
keywords: Abfragebeispiele
services: stream-analytics
documentationcenter: 
author: samacha
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: samacha
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: a27bae1828bd469d4439e0ce43098edd73f54243
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a>Abfragebeispiele für gängige Stream Analytics-Verwendungsmuster
## <a name="introduction"></a>Einführung
Abfragen in Azure Stream Analytics werden in einer SQL-ähnlichen Abfragesprache ausgedrückt. Diese Abfragen sind im Handbuch [Referenz zur Stream Analytics-Abfragesprache](https://msdn.microsoft.com/library/azure/dn834998.aspx) dokumentiert. Dieser Artikel zeigt anhand von Praxisbeispielen Lösungen für mehrere weit verbreitete Abfragemuster. Das Dokument wird nach und nach mit weiteren Mustern aktualisiert.

## <a name="query-example-convert-data-types"></a>Abfragebeispiel: Konvertieren von Datentypen
**Beschreibung**: Definieren der Arten von Eigenschaften im Eingabestream.
Beispiel: Das Fahrzeuggewicht ist im Eingabestream als Zeichenfolge angegeben und muss zur Durchführung von **SUM** in **INT** konvertiert werden.

**Eingabe**:

| Make | Time | Weight |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |"1000" |
| Honda |2015-01-01T00:00:02.0000000Z |"2000" |

**Ausgabe**:

| Make | Weight |
| --- | --- |
| Honda |3000 |

**Lösung**:

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erläuterung**: Verwenden Sie im Feld **Gewichtung** eine **CAST**-Anweisung, um den jeweiligen Datentyp anzugeben. Die Liste der unterstützten Datentypen finden Sie unter [Datentypen (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).

## <a name="query-example-use-likenot-like-to-do-pattern-matching"></a>Abfragebeispiel: Musterabgleich mithilfe von „Like“/„Not like“
**Beschreibung**: Sicherstellen, dass ein Feldwert im Ereignis einem bestimmten Muster entspricht.
Beispiel: Überprüfen des Ergebnisses, ob Nummernschilder zurückgegeben werden, die mit „A“ beginnen und mit „9“ enden.

**Eingabe**:

| Make | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Ausgabe**:

| Make | LicensePlate | Time |
| --- | --- | --- |
| Toyota |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Nissan |ABC-369 |2015-01-01T00:00:03.0000000Z |

**Lösung**:

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

**Erläuterung**: Verwenden Sie die **LIKE**-Anweisung, um den Feldwert von **LicensePlate** zu überprüfen. Dieser sollte mit „A“ beginnen, von einer leeren Zeichenfolge oder einer Zeichenfolge mit einer beliebigen Anzahl von Zeichen gefolgt werden und schließlich mit „9“ enden. 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a>Abfragebeispiel: Logik für verschiedene Fälle/Werte (CASE-Anweisungen)
**Beschreibung**: Angeben einer abweichenden kriterienbasierten Berechnung für ein Feld.
Beispiel: Bereitstellen einer Zeichenfolge, die beschreibt, wie viele Fahrzeuge der gleichen Marke vorbeigefahren sind (mit einem Sonderfall für „1“).

**Eingabe**:

| Make | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Ausgabe**:

| CarsPassed | Time |
| --- | --- | --- |
| 1 Honda |2015-01-01T00:00:10.0000000Z |
| 2 Toyotas |2015-01-01T00:00:10.0000000Z |

**Lösung**:

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

**Erläuterung**: Mit der **CASE**-Klausel kann auf der Grundlage einiger Kriterien (in unserem Fall der Fahrzeuganzahl im Aggregatfenster) eine abweichende Berechnung angegeben werden.

## <a name="query-example-send-data-to-multiple-outputs"></a>Abfragebeispiel: Senden von Daten an mehrere Ausgaben
**Beschreibung**: Senden von Daten an mehrere Ausgabeziele über einen einzelnen Auftrag.
Beispiel: Analysieren von Daten für eine schwellenwertbasierte Warnung und Archivieren aller Ereignisse in Blob Storage.

**Eingabe**:

| Make | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Ausgabe 1**:

| Make | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Ausgabe 2**:

| Make | Time | Count |
| --- | --- | --- |
| Toyota |2015-01-01T00:00:10.0000000Z |3 |

**Lösung**:

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

**Erläuterung**: Mit der **INTO**-Klausel wird Stream Analytics mitgeteilt, in welche Ausgabe die Daten aus dieser Anweisung geschrieben werden sollen.
Bei der ersten Abfrage werden die eingegangenen Daten an eine Ausgabe namens **ArchiveOutput** übergeben.
Bei der zweiten Abfrage werden die Daten nach einer einfachen Aggregation und Filterung an ein nachgelagertes Warnsystem gesendet.

Beachten Sie, dass die Ergebnisse der allgemeinen Tabellenausdrücke (Common Table Expressions, CTEs) (z.B. **WITH**-Anweisungen) in mehreren Ausgabeanweisungen auch wiederverwendet werden können. Diese Option hat den Vorteil, dass weniger Leser für die Eingabequelle geöffnet werden müssen.
Beispiel: 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a>Abfragebeispiel: Zählen eindeutiger Werte
**Beschreibung**: Zählen der Anzahl eindeutiger Feldwerte, die im Stream innerhalb eines bestimmten Zeitfensters vorkommen.
Beispiel: Wie viele individuelle Fahrzeugmarken passieren in einem Zeitraum von zwei Sekunden die Mautstation?

**Eingabe**:

| Make | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Honda |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |
| Toyota |2015-01-01T00:00:03.0000000Z |

**Ausgabe:**

| Count | Time |
| --- | --- |
| 2 |2015-01-01T00:00:02.000Z |
| 1 |2015-01-01T00:00:04.000Z |

**Lösung:**

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


**Erläuterung:**
**COUNT (DISTINCT Make)** gibt die Anzahl der unterschiedlichen Werte der Spalte **Make** innerhalb eines Zeitfensters zurück.

## <a name="query-example-determine-if-a-value-has-changed"></a>Abfragebeispiel: Ermitteln, ob ein Wert geändert wurde
**Beschreibung**: Überprüfen eines früheren Werts zur Bestimmung, ob er sich von dem aktuellen Wert unterscheidet.
Beispiel: Weist das vorherige Fahrzeug auf der Mautstraße dieselbe Marke auf wie das aktuelle Fahrzeug?

**Eingabe**:

| Make | Time |
| --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Ausgabe**:

| Make | Time |
| --- | --- |
| Toyota |2015-01-01T00:00:02.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

**Erläuterung**: Verwenden Sie **LAG**, um im Eingabestream einen Blick auf das vorherige Ereignis zu werfen und den **Make**-Wert zu ermitteln. Vergleichen Sie ihn dann mit dem **Make**-Wert des aktuellen Ereignisses, und geben Sie das Ereignis aus, falls sich die Werte unterscheiden.

## <a name="query-example-find-the-first-event-in-a-window"></a>Abfragebeispiel: Ermitteln des ersten Ereignisses in einem Zeitfenster
**Beschreibung**: Ermitteln des jeweils ersten Fahrzeugs in einem Zehn-Minuten-Intervall.

**Eingabe**:

| LicensePlate | Make | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Ausgabe**:

| LicensePlate | Make | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

Ändern wir nun die Aufgabe, und ermitteln wir jeweils das erste Fahrzeug einer bestimmten Marke in einem Zehn-Minuten-Intervall.

| LicensePlate | Make | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Lösung**:

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-the-last-event-in-a-window"></a>Abfragebeispiel: Ermitteln des letzten Ereignisses in einem Zeitfenster
**Beschreibung**: Ermitteln des jeweils letzten Fahrzeugs in einem Zehn-Minuten-Intervall.

**Eingabe**:

| LicensePlate | Make | Time |
| --- | --- | --- |
| DXE 5291 |Honda |2015-07-27T00:00:05.0000000Z |
| YZK 5704 |Ford |2015-07-27T00:02:17.0000000Z |
| RMV 8282 |Honda |2015-07-27T00:05:01.0000000Z |
| YHN 6970 |Toyota |2015-07-27T00:06:00.0000000Z |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| QYF 9358 |Honda |2015-07-27T00:12:02.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Ausgabe**:

| LicensePlate | Make | Time |
| --- | --- | --- |
| VFE 1616 |Toyota |2015-07-27T00:09:31.0000000Z |
| MDR 6128 |BMW |2015-07-27T00:13:45.0000000Z |

**Lösung**:

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

**Erläuterung**: Es gibt zwei Schritte in der Abfrage. Im ersten Schritt wird in einem Zeitfenster von zehn Minuten nach dem aktuellen Zeitstempel gesucht. Im zweiten Schritt werden die Ergebnisse der ersten Abfrage mit dem ursprünglichen Stream zusammengeführt, um nach Ereignissen zu suchen, die dem letzten Zeitstempel des jeweiligen Zeitfensters entsprechen. 

## <a name="query-example-detect-the-absence-of-events"></a>Abfragebeispiel: Erkennen der Abwesenheit von Ereignissen
**Beschreibung**: Überprüfen, ob der Stream einen Wert enthält, der einem bestimmten Kriterium entspricht.
Beispiel: Wurde die mautpflichtige Straße innerhalb der letzten 90 Sekunden von zwei aufeinanderfolgenden Fahrzeugen der gleichen Marke befahren?

**Eingabe**:

| Make | LicensePlate | Time |
| --- | --- | --- |
| Honda |ABC-123 |2015-01-01T00:00:01.0000000Z |
| Honda |AAA-999 |2015-01-01T00:00:02.0000000Z |
| Toyota |DEF-987 |2015-01-01T00:00:03.0000000Z |
| Honda |GHI-345 |2015-01-01T00:00:04.0000000Z |

**Ausgabe**:

| Make | Time | CurrentCarLicensePlate | FirstCarLicensePlate | FirstCarTime |
| --- | --- | --- | --- | --- |
| Honda |2015-01-01T00:00:02.0000000Z |AAA-999 |ABC-123 |2015-01-01T00:00:01.0000000Z |

**Lösung**:

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

**Erläuterung**: Verwenden Sie **LAG**, um im Eingabestream einen Blick auf das vorherige Ereignis zu werfen und den **Make**-Wert zu ermitteln. Vergleichen Sie ihn mit dem **MAKE**-Wert des aktuellen Ereignisses, und geben Sie dann das Ereignis aus, falls die Werte identisch sind. Sie können Daten zum vorherigen Fahrzeug auch mithilfe von **LAG** abrufen.

## <a name="query-example-detect-the-duration-between-events"></a>Abfragebeispiel: Ermitteln der Dauer zwischen Ereignissen
**Beschreibung**: Ermitteln der Dauer eines bestimmten Ereignisses. Beispiel: Ermitteln der für eine Funktion aufgewendeten Zeit mit einem bestimmten Webclickstream.

**Eingabe**:  

| Benutzer | Feature | Ereignis | Time |
| --- | --- | --- | --- |
| user@location.com |RightMenu |Start |2015-01-01T00:00:01.0000000Z |
| user@location.com |RightMenu |End |2015-01-01T00:00:08.0000000Z |

**Ausgabe**:  

| Benutzer | Feature | Duration |
| --- | --- | --- |
| user@location.com |RightMenu |7 |

**Lösung**:

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

**Erläuterung**: Verwenden Sie die **LAST**-Funktion, um den letzten **TIME**-Wert mit dem Ereignistyp **Start** zu ermitteln. Die **LAST**-Funktion verwendet **PARTITION BY [user]**, um anzuzeigen, dass das Ergebnis einzeln pro Benutzer berechnet wird. Die Abfrage hat einen maximalen Schwellenwert von einer Stunde für die Zeitdifferenz zwischen **Start**- und **Stopp**-Ereignissen, ist aber nach Bedarf konfigurierbar **(LIMIT DURATION(hour, 1)**.

## <a name="query-example-detect-the-duration-of-a-condition"></a>Abfragebeispiel: Ermitteln der Dauer einer Bedingung
**Beschreibung**: Ermitteln, wie lange eine Bedingung angedauert hat.
Beispiel: Aufgrund eines Fehlers wurde für alle Fahrzeuge ein falsches Gewicht (über 20.000 Pfund) erfasst. Nun möchten wir ermitteln, wie lange dieser Fehler aufgetreten ist.

**Eingabe**:

| Make | Time | Weight |
| --- | --- | --- |
| Honda |2015-01-01T00:00:01.0000000Z |2000 |
| Toyota |2015-01-01T00:00:02.0000000Z |25000 |
| Honda |2015-01-01T00:00:03.0000000Z |26000 |
| Toyota |2015-01-01T00:00:04.0000000Z |25000 |
| Honda |2015-01-01T00:00:05.0000000Z |26000 |
| Toyota |2015-01-01T00:00:06.0000000Z |25000 |
| Honda |2015-01-01T00:00:07.0000000Z |26000 |
| Toyota |2015-01-01T00:00:08.0000000Z |2000 |

**Ausgabe**:

| StartFault | EndFault |
| --- | --- |
| 2015-01-01T00:00:02.000Z |2015-01-01T00:00:07.000Z |

**Lösung**:

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

**Erläuterung**: Verwenden Sie **LAG**, um den Eingabestream 24 Stunden lang einzusehen, und suchen Sie nach Instanzen, bei denen **StartFault** und **StopFault** von Gewichtungen unter 20.000 umgeben sind.

## <a name="query-example-fill-missing-values"></a>Beispiel für eine Abfrage: Ausfüllen der fehlenden Werte
**Beschreibung**: Erstellen Sie für den Ereignisdatenstrom mit fehlenden Werten einen Ereignisdatenstrom mit regelmäßigen Intervallen.
Generieren Sie z.B. alle 5 Sekunden ein Ereignis, das den zuletzt angezeigten Datenpunkt meldet.

**Eingabe**:

| t | value |
| --- | --- |
| "2014-01-01T06:01:00" |1 |
| "2014-01-01T06:01:05" |2 |
| "2014-01-01T06:01:10" |3 |
| "2014-01-01T06:01:15" |4 |
| "2014-01-01T06:01:30" |5 |
| "2014-01-01T06:01:35" |6 |

**Ausgabe (erste zehn Zeilen)**:

| windowend | lastevent.t | lastevent.value |
| --- | --- | --- |
| 2014-01-01T14:01:00.000Z |2014-01-01T14:01:00.000Z |1 |
| 2014-01-01T14:01:05.000Z |2014-01-01T14:01:05.000Z |2 |
| 2014-01-01T14:01:10.000Z |2014-01-01T14:01:10.000Z |3 |
| 2014-01-01T14:01:15.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:20.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:25.000Z |2014-01-01T14:01:15.000Z |4 |
| 2014-01-01T14:01:30.000Z |2014-01-01T14:01:30.000Z |5 |
| 2014-01-01T14:01:35.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:40.000Z |2014-01-01T14:01:35.000Z |6 |
| 2014-01-01T14:01:45.000Z |2014-01-01T14:01:35.000Z |6 |

**Lösung**:

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


**Erläuterung**: Diese Abfrage generiert alle fünf Sekunden Ereignisse und gibt das letzte zuvor empfangene Ereignis aus. Die Dauer eines [springenden Fensters](https://msdn.microsoft.com/library/dn835041.aspx "Springendes Fenster – Azure Stream Analytics") legt fest, wie weit die Abfrage zurückreicht, um das letzte Ereignis zu suchen (in diesem Beispiel 300 Sekunden).

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, nutzen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


