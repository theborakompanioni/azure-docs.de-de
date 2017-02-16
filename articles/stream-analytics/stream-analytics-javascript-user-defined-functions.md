---
title: "Azure Stream Analytics – Benutzerdefinierte JavaScript-Funktionen | Microsoft-Dokumentation"
description: "IoT-Sensortags und -Datenströme mit Stream Analytics und Echtzeit-Datenverarbeitung"
keywords: "IoT-Lösung, Erste Schritte mit IoT, Tools"
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/01/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: 822f0870396e1fa5fe2dfee3bae410aad3da1122
ms.openlocfilehash: 959fd48cb7d7f21385f6eea21f79ad26e8164bc0

---

# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Azure Stream Analytics – Benutzerdefinierte JavaScript-Funktionen
Azure Stream Analytics unterstützt benutzerdefinierte Funktionen (User-Defined Functions, UDFs), die in JavaScript geschrieben sind. Dank der umfassenden String-, RegExp-, Math-, Array- und Date-Methoden von JavaScript wird die Erstellung von komplexen Datentransformationen mit Stream Analytics-Aufträgen vereinfacht.

## <a name="overview"></a>Übersicht
JavaScript-UDFs unterstützen zustandslose skalare Computefunktionen, für die keine externe Verbindung benötigt wird. Der Rückgabewert einer Funktion kann nur ein skalarer (einzelner) Wert sein. Nach dem Hinzufügen zu einem Auftrag kann die Funktion wie eine integrierte skalare Funktion an einer beliebigen Stelle in der Abfrage verwendet werden.

Hier sind einige Beispielszenarien angegeben, für die JavaScript-UDFs nützlich sind:
* Analysieren und Bearbeiten von Zeichenfolgen mit Funktionen mit regulären Ausdrücken, z.B. „Regexp_Replace()“ und „Regexp_Extract()“
* Decodieren von Codierungsdaten, z.B. Konvertierung von binär in hexadezimal
* Mathematische Berechnungen mit mathematischen JavaScript-Funktionen
* Arrayvorgänge, z.B. Sortieren, Verknüpfen, Suchen und Füllen

Unten ist aufgeführt, was mit JavaScript-UDFs in Stream Analytics nicht möglich ist:
* Aufrufen von externen REST-Endpunkten, z.B. umgekehrte IP-Suche oder Abrufen von Referenzdaten von einer externen Quelle
* Benutzerdefinierte Serialisierung oder Deserialisierung von Ereignisformaten für Ein-/Ausgaben
* Benutzerdefinierte Aggregate

Auch wenn sie in der Funktionsdefinition nicht blockiert sind, ist es ratsam, auf die Verwendung von Funktionen wie „Date.GetDate()“ oder „Math.random()“ zu verzichten. Bei diesen Funktionen wird **nicht** bei jedem Aufruf das gleiche Ergebnis zurückgegeben, und im Azure Stream Analytics-Dienst wird kein Journal mit den Funktionsaufrufen und den zurückgegebenen Ergebnissen geführt. Wenn eine Funktion unterschiedliche Ergebnisse für die gleichen Ereignisse zurückgibt, ist daher beim Starten eines Auftrags durch Sie oder den Stream Analytics-Dienst die Wiederholbarkeit nicht sichergestellt.

## <a name="adding-a-javascript-udf-from-azure-portal"></a>Hinzufügen einer JavaScript-UDF über das Azure-Portal
Führen Sie die unten angegebenen Schritte aus, um eine einfache benutzerdefinierte JavaScript-Funktion unter einem vorhandenen Stream Analytics-Auftrag zu erstellen.

1.  Öffnen Sie das Azure-Portal.

2.  Suchen Sie nach Ihrem Stream Analytics-Auftrag, und klicken Sie unter **AUFTRAGSTOPOLOGIE** dann auf den Link für die Funktionen.
 
3.  Eine leere Liste für die vorhandenen Funktionen wird angezeigt. Klicken Sie auf das Symbol **Hinzufügen**, um eine neue UDF hinzuzufügen.

4.  Wählen Sie auf dem Blatt **Neue Funktion** die Option „JavaScript“ als Funktionstyp aus. Im Editor wird daraufhin eine Standardvorlage für Funktionen angezeigt.
 
5.  Geben Sie _hex2Int_ als UDF-Alias ein, und ändern Sie die Funktionsimplementierung wie unten angegeben.

    ```
    // Convert Hex value to integer.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.  Klicken Sie auf die Schaltfläche **Speichern**. Die Funktion wird in der Liste mit den Funktionen angezeigt. 

7.  Wenn Sie auf die neue Funktion **hex2Int** klicken, können Sie die Funktionsdefinition überprüfen. Beachten Sie, dass für alle Funktionen vor dem Funktionsalias das Präfix „UDF“ hinzugefügt wird. Sie müssen die Funktion in Ihrer Stream Analytics-Abfrage **mit dem Präfix** aufrufen (in diesem Fall **UDF.hex2Int**).
 
## <a name="calling-javascript-udf-in-a-query"></a>Aufrufen der JavaScript-UDF in einer Abfrage

1. Öffnen Sie den Abfrage-Editor, indem Sie unter **AUFTRAGSTOPOLOGIE** auf **Abfrage** klicken. 

2.  Bearbeiten Sie die Abfrage, und rufen Sie die eben hinzugefügte UDF wie folgt auf:

    ```
    SELECT 
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.  Klicken Sie mit der rechten Maustaste auf die Auftragseingabe, um die Beispieldatendatei hochzuladen. 
 
4.  Klicken Sie auf **Testen**, um die Abfrage zu testen.


## <a name="supported-javascript-objects"></a>Unterstützte JavaScript-Objekte
Für Azure Stream Analytics-JavaScript-UDFs werden integrierte Standardobjekte der JavaScript-Sprache unterstützt. Die Liste mit den Objekten finden Sie im Dokument zu [globalen Objekten](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics- und JavaScript-Typkonvertierung

Es gibt Unterschiede zwischen den Typen, die in der Stream Analytics-Abfragesprache und in JavaScript unterstützt werden. In der folgenden Tabelle sind die Konvertierungszuordnungen für diese beiden Sprachen aufgeführt.


---
Stream Analytics | JavaScript
--- | ---
bigint | Number (Beachten Sie, dass in JavaScript nur Integers bis exakt 2^53 dargestellt werden können.)
DateTime | Date (In JavaScript werden nur Millisekunden unterstützt.) 
double | NUMBER
nvarchar(MAX) | string
Datensatz | Objekt
Array | Array
NULL | Null


Konvertierungen von JavaScript in ASA sind ebenfalls aufgeführt.

---
JavaScript | Stream Analytics
--- | ---
NUMBER | „bigint“, wenn die Zahl gerundet ist und zwischen „long.MinValue“ und „long.MaxValue“ liegt, andernfalls „double“.
Datum | DateTime
String | nvarchar(MAX)
Objekt | Datensatz
Array | Array
Null, Undefined | NULL
Jeder andere Typ, z.B. Funktion, Fehler usw. | Nicht unterstützt – Runtimefehler

## <a name="troubleshooting"></a>Problembehandlung
JavaScript-Runtimefehler werden als schwerwiegend angesehen und im Aktivitätsprotokoll aufgezeichnet. Navigieren Sie zu Ihrem Auftrag, und klicken Sie auf **Aktivitätsprotokoll**, um das Protokoll aus dem Azure-Portal abzurufen.
 

## <a name="other-javascript-udf-usage-patterns"></a>Andere JavaScript-UDF-Verwendungsmuster

### <a name="writing-nested-json-to-output"></a>Schreiben von geschachteltem JSON-Code für die Ausgabe
Das Schreiben einer auszugebenden JSON-Zeichenfolge ist eine häufige Aufgabe, wenn ein nachfolgender Verarbeitungsschritt ausgeführt werden muss, bei dem die Ausgabe des Stream Analytics-Auftrags als Eingabe verwendet wird und das JSON-Format erforderlich ist. Im Beispiel unten wird die Funktion „JSON.stringify()“ aufgerufen, um alle Name-Wert-Paare der Eingabe zu verpacken und als einzelnen Zeichenfolgenwert in die Ausgabe zu schreiben. 

### <a name="javascript-udf-definition"></a>JavaScript-UDF-Definition:

```
function main(x) {
return JSON.stringify(x);
}
```

**Beispielabfrage:**
```
SELECT 
    DataString,
    DataValue,
    HexValue,
    UDF.json_stringify(input) As InputEvent
INTO
    output
FROM
    input PARTITION BY PARTITIONID
```

## <a name="get-help"></a>Hier erhalten Sie Hilfe
Um Hilfe zu erhalten, besuchen Sie unser [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference (in englischer Sprache)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referenz zur Azure Stream Analytics-Verwaltungs-REST-API](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--HONumber=Feb17_HO1-->


