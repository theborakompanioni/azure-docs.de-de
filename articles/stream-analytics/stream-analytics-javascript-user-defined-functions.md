---
title: "Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen | Microsoft Docs"
description: "Durchführen erweiterter Abfragemechanismen mit benutzerdefinierten JavaScript-Funktionen"
keywords: JavaScript, benutzerdefinierte Funktionen, UDF
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
ms.date: 03/28/2017
ms.author: jeffstok
translationtype: Human Translation
ms.sourcegitcommit: a8334d146877ccc3988784c7a74a1c887dba68ab
ms.openlocfilehash: 57027b97cebf8accccd91f135a13825047fd211e
ms.lasthandoff: 02/09/2017

---

# <a name="azure-stream-analytics-javascript-user-defined-functions"></a>Azure Stream Analytics – benutzerdefinierte JavaScript-Funktionen
Azure Stream Analytics unterstützt benutzerdefinierte Funktionen, die in JavaScript geschrieben sind. Die umfassenden **String**-, **RegExp**-, **Math**-, **Array**- und **Date**-Methoden von JavaScript vereinfachen die Erstellung von komplexen Datentransformationen mit Stream Analytics-Aufträgen.

## <a name="javascript-user-defined-functions"></a>JavaScript, benutzerdefinierte Funktionen
Benutzerdefinierte JavaScript-Funktionen unterstützen zustandslose skalare Computefunktionen, für die keine externe Verbindung benötigt wird. Der Rückgabewert einer Funktion kann nur ein skalarer (einzelner) Wert sein. Nachdem Sie einem Auftrag eine benutzerdefinierte JavaScript-Funktion hinzugefügt haben, können Sie die Funktion wie eine integrierte Skalarfunktion an einer beliebigen Stelle in der Abfrage verwenden.

Hier finden Sie einige Szenarien, in denen benutzerdefinierte JavaScript-Funktionen hilfreich sein könnten:
* Analysieren und Bearbeiten von Zeichenfolgen mit Funktionen mit regulären Ausdrücken, z.B. **Regexp_Replace()** und **Regexp_Extract()**
* Decodieren und Codieren von Daten, z.B. Konvertierung von binär in hexadezimal
* Durchführen mathematischer Berechnungen mit **Math**-Funktionen aus JavaScript
* Durchführen von Arrayvorgängen, z.B. Sortieren, Verknüpfen, Suchen und Füllen

Hier ist aufgeführt, was mit benutzerdefinierten JavaScript-Funktionen in Stream Analytics nicht möglich ist:
* Aufrufen von externen REST-Endpunkten, z.B. für die Durchführung umgekehrter IP-Suchen oder zum Abrufen von Referenzdaten von einer externen Quelle
* Durchführen einer benutzerdefinierten Serialisierung oder Deserialisierung von Ereignisformaten für Ein-/Ausgaben
* Erstellen benutzerdefinierter Aggregate

Auch wenn die Verwendung von Funktionen wie **Date.GetDate()** oder **Math.random()** in der Funktionsdefinition nicht blockiert ist, ist es ratsam, auf sie zu verzichten. Bei diesen Funktionen wird **nicht** bei jedem Aufruf das gleiche Ergebnis zurückgegeben, und im Azure Stream Analytics-Dienst wird kein Journal mit den Funktionsaufrufen und den zurückgegebenen Ergebnissen geführt. Wenn eine Funktion unterschiedliche Ergebnisse für dieselben Ereignisse zurückgibt, ist daher beim Starten eines Auftrags durch Sie oder den Stream Analytics-Dienst die Wiederholbarkeit nicht sichergestellt.

## <a name="add-a-javascript-user-defined-function-in-the-azure-portal"></a>Hinzufügen benutzerdefinierter JavaScript-Funktionen im Azure-Portal
Führen Sie die unten angegebenen Schritte aus, um eine einfache benutzerdefinierte JavaScript-Funktion unter einem vorhandenen Stream Analytics-Auftrag zu erstellen:

1.    Suchen Sie den Stream Analytics-Auftrag im Azure-Portal.
2.  Wählen Sie unter **Auftragstopologie** Ihre Funktion aus. Eine leere Liste von Funktionen wird angezeigt.
3.    Wählen Sie zum Erstellen einer neuen benutzerdefinierten Funktion **Hinzufügen** aus.
4.    Wählen Sie auf dem Blatt **Neue Funktion** als **Funktionstyp** die Option **JavaScript** aus. Eine Standardfunktionsvorlage wird im Editor angezeigt.
5.    Geben Sie **hex2Int** als **UDF-Alias** ein, und ändern Sie die Funktionsimplementierung wie folgt:

    ```
    // Convert Hex value to integer.
    function main(hexValue) {
        return parseInt(hexValue, 16);
    }
    ```

6.    Wählen Sie **Speichern**aus. Ihre Funktion wird in der Liste der Funktionen angezeigt.
7.    Wählen Sie die neue **hex2Int**-Funktion aus, und überprüfen Sie die Funktionsdefinition. Bei allen Funktionen wird dem Funktionsalias ein **UDF**-Präfix hinzugefügt. Sie müssen das Präfix beim Aufruf der Funktion in der Stream Analytics-Abfrage *einschließen*. Rufen Sie in diesem Fall **UDF.hex2Int** aus.

## <a name="call-a-javascript-user-defined-function-in-a-query"></a>Aufrufen benutzerdefinierter JavaScript-Funktionen in einer Abfrage

1. Wählen Sie im Abfrage-Editor unter **Auftragstopologie** die Option **Abfrage** aus.
2.    Bearbeiten Sie die Abfrage, und rufen Sie anschließend die benutzerdefinierte Funktion wie folgt auf:

    ```
    SELECT
        time,
        UDF.hex2Int(offset) AS IntOffset
    INTO
        output
    FROM
        InputStream
    ```

3.    Klicken Sie zum Hochladen der Beispieldatendatei mit der rechten Maustaste auf die Auftragseingabe.
4.    Wählen Sie zum Testen der Abfrage **Testen** aus.


## <a name="supported-javascript-objects"></a>Unterstützte JavaScript-Objekte
Benutzerdefinierte JavaScript-Funktionen in Azure Stream Analytics unterstützen die integrierten JavaScript-Standardobjekte. Eine Liste dieser Objekte finden Sie unter [Globale Objekte](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects).

### <a name="stream-analytics-and-javascript-type-conversion"></a>Stream Analytics- und JavaScript-Typkonvertierung

Es gibt Unterschiede zwischen den Typen, die in der Stream Analytics-Abfragesprache und in JavaScript unterstützt werden. In der folgenden Tabelle sind die Konvertierungszuordnungen für diese beiden Sprachen aufgeführt:

Stream Analytics | JavaScript
--- | ---
bigint | Number (in JavaScript können nur ganze Zahlen bis genau 2^53 dargestellt werden)
DateTime | Date (JavaScript unterstützt nur Millisekunden)
double | NUMBER
nvarchar(MAX) | string
Datensatz | Objekt
Array | Array
NULL | Null


Konvertierungen von JavaScript zu Stream Analytics:


JavaScript | Stream Analytics
--- | ---
NUMBER | bigint (wenn die Zahl gerundet ist und zwischen long.MinValue und long.MaxValue liegt, andernfalls double)
Datum | DateTime
String | nvarchar(MAX)
Objekt | Datensatz
Array | Array
Null, Undefined | NULL
Beliebiger anderer Typ (z.B. eine Funktion oder ein Fehler) | Nicht unterstützt (führt zu einem Laufzeitfehler)

## <a name="troubleshooting"></a>Problembehandlung
JavaScript-Laufzeitfehler werden als schwerwiegend angesehen und im Aktivitätsprotokoll aufgezeichnet. Navigieren Sie im Azure-Portal zu Ihrem Auftrag, und wählen Sie **Aktivitätsprotokoll** aus, um das Protokoll abzurufen.


## <a name="other-javascript-user-defined-function-patterns"></a>Weitere benutzerdefinierte JavaScript-Funktionsmuster

### <a name="write-nested-json-to-output"></a>Schreiben von geschachteltem JSON-Code in die Ausgabe
Wenn ein nachfolgender Verarbeitungsschritt die Ausgabe des Stream Analytics-Auftrags als Eingabe verwendet und das JSON-Format benötigt, können Sie eine JSON-Zeichenfolge in die Ausgabe schreiben. Im nächsten Beispiel wird die Funktion **JSON.stringify()** aufgerufen, um alle Name-Wert-Paare der Eingabe zu packen und als einzelnen Zeichenfolgenwert in die Ausgabe zu schreiben.

**Definition von benutzerdefinierten JavaScript-Funktionen:**

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
Weitere Hilfe finden Sie in unserem [Azure Stream Analytics-Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Azure Stream Analytics](stream-analytics-introduction.md)
* [Erste Schritte mit Azure Stream Analytics](stream-analytics-get-started.md)
* [Skalieren von Azure Stream Analytics-Aufträgen](stream-analytics-scale-jobs.md)
* [Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referenz zur Stream Analytics-Abfragesprache)
* [Azure Stream Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx) (Referenz zur Azure Stream Analytics-Verwaltungs-REST-API)

