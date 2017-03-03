---
title: Grundlegendes zur Azure IoT Hub-Abfragesprache | Microsoft Docs
description: "Entwicklerhandbuch – Beschreibung der SQL-ähnlichen IoT Hub-Abfragesprache, die zum Abrufen von Informationen zu Gerätezwillingen und Aufträgen vom IoT Hub verwendet wird."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 851a9ed3-b69e-422e-8a5d-1d79f91ddf15
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: 64f44c176633db4179f954d2f70cdf26d08b60b4
ms.openlocfilehash: 28ea238484d86b044899aa9f95861bbdbbf3a06c
ms.lasthandoff: 02/03/2017


---
# <a name="reference---iot-hub-query-language-for-device-twins-and-jobs"></a>Referenz – IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge
## <a name="overview"></a>Übersicht
IoT Hub bietet eine leistungsstarke SQL-ähnliche Sprache zum Abrufen von Informationen zu [Gerätezwillingen][lnk-twins] und [Aufträgen][lnk-jobs]. Dieser Artikel enthält Folgendes:

* Eine Einführung in die wichtigsten Features der IoT Hub-Abfragesprache
* Eine ausführliche Beschreibung der Sprache

## <a name="get-started-with-device-twin-queries"></a>Erste Schritte mit Gerätezwillingsabfragen
[Gerätezwillinge][lnk-twins] können beliebige JSON-Objekte als Tags und Eigenschaften enthalten. In IoT Hub können Sie Gerätezwillinge als einzelnes JSON-Dokument abfragen, das alle Informationen zum Gerätezwilling enthält.
Angenommen, Ihre IoT Hub-Gerätezwillinge weisen die folgende Struktur auf:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                "location": {                                                      
                    "region": "US",                                                  
                    "plant": "Redmond43"                                             
                }                                                                  
            },                                                                   
            "properties": {                                                      
                "desired": {                                                       
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300                                          
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                     
                    },                                                               
                    "$version": 4                                                    
                },                                                                 
                "reported": {                                                      
                    "connectivity": {                                                
                        "type": "cellular"                            
                    },                                                               
                    "telemetryConfig": {                                             
                        "configId": "db00ebf5-eeeb-42be-86a1-458cccb69e57",            
                        "sendFrequencyInSecs": 300,                                         
                        "status": "Success"                                            
                    },                                                               
                    "$metadata": {                                                   
                    ...                                                
                    },                                                               
                    "$version": 7                                                    
                }                                                                  
            }                                                                    
        }

IoT Hub macht die Gerätezwillinge als eine Dokumentensammlung namens **Geräte** verfügbar.
Die folgende Abfrage ruft also die gesamte Gruppe von Gerätezwillingen ab:

        SELECT * FROM devices

> [!NOTE]
> [Azure IoT SDKs][lnk-hub-sdks] unterstützen das Paging von umfangreichen Ergebnissen.
>
>

IoT Hub ermöglicht beim Abrufen von Gerätezwillingen das Filtern mit beliebigen Bedingungen. Beispielsweise

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

werden die Gerätezwillinge abgerufen, deren Tag **location.region** auf **US** festgelegt ist.
Boolesche Operatoren und arithmetische Vergleiche werden ebenfalls unterstützt. Beispielsweise

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

werden alle Gerätezwillinge abgerufen, die sich in den USA befinden und so konfiguriert sind, dass Telemetriedaten seltener als minütlich gesendet werden. Zur Vereinfachung können auch Arraykonstanten mit den Operatoren **IN** und **NIN** (nicht IN) verwendet werden. Beispielsweise

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

werden alle Gerätezwillinge abgerufen, die WLAN oder eine kabelgebundene Verbindung gemeldet haben. Es ist häufig erforderlich, alle Gerätezwillinge zu ermitteln, die eine bestimmte Eigenschaft enthalten. IoT Hub unterstützt zu diesem Zweck die Funktion `is_defined()`. Beispielsweise

        SELECT * FROM devices
        WHERE is_defined(property.reported.connectivity)

werden alle Gerätezwillinge abgerufen, mit denen die gemeldete `connectivity`-Eigenschaft definiert wird. Die vollständige Referenz zu den Filtermöglichkeiten finden Sie im Abschnitt zur [WHERE-Klausel][lnk-query-where].

Gruppierungen und Aggregationen werden ebenfalls unterstützt. Beispiel:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Gibt die Anzahl der Geräte in jedem Telemetriekonfigurationsstatus zurück.

        [
            {
                "numberOfDevices": 3,
                "status": "Success"
            },
            {
                "numberOfDevices": 2,
                "status": "Pending"
            },
            {
                "numberOfDevices": 1,
                "status": "Error"
            }
        ]

Das vorausgehende Beispiel veranschaulicht eine Situation, in der drei Geräte eine erfolgreiche Konfiguration gemeldet haben, zwei die Konfiguration noch übernehmen und eines einen Fehler gemeldet hat.

### <a name="c-example"></a>C#-Beispiel
Die Abfragefunktion wird durch das [C#-Dienst-SDK][lnk-hub-sdks] in der **RegistryManager**-Klasse verfügbar gemacht.
Es folgt ein Beispiel für eine einfache Abfrage:

        var query = registryManager.CreateQuery("SELECT * FROM devices", 100);
        while (query.HasMoreResults)
        {
            var page = await query.GetNextAsTwinAsync();
            foreach (var twin in page)
            {
                // do work on twin object
            }
        }

Beachten Sie, wie das **query**-Objekt mit einer Seitengröße (bis zu 1.000) instanziiert wird und dann mehrere Seiten durch mehrmaliges Aufrufen der **GetNextAsTwinAsync**-Methoden abgerufen werden können.
Beachten Sie, dass das query-Objekt mehrmals **Next\*** verfügbar macht – je nach der für die Abfrage benötigten Deserialisierung, z.B. Gerätezwillings- oder Auftragsobjekte oder reiner JSON-Code für Projektionen.

### <a name="nodejs-example"></a>Node.js-Beispiel
Die Abfragefunktion wird durch das [Azure IoT-Dienst-SDK für Node.js][lnk-hub-sdks] im **Registry-Objekt** verfügbar gemacht.
Es folgt ein Beispiel für eine einfache Abfrage:

        var query = registry.createQuery('SELECT * FROM devices', 100);
        var onResults = function(err, results) {
            if (err) {
                console.error('Failed to fetch the results: ' + err.message);
            } else {
                // Do something with the results
                results.forEach(function(twin) {
                    console.log(twin.deviceId);
                });

                if (query.hasMoreResults) {
                    query.nextAsTwin(onResults);
                }
            }
        };
        query.nextAsTwin(onResults);

Beachten Sie, wie das **query**-Objekt mit einer Seitengröße (bis zu 1.000) instanziiert wird und dann mehrere Seiten durch mehrmaliges Aufrufen der **nextAsTwin**-Methoden abgerufen werden können.
Beachten Sie, dass das query-Objekt mehrmals **Next\*** verfügbar macht – je nach der für die Abfrage benötigten Deserialisierung, z.B. Gerätezwillings- oder Auftragsobjekte oder reiner JSON-Code für Projektionen.

### <a name="limitations"></a>Einschränkungen
Derzeit werden Vergleiche nur zwischen primitiven Typen (keine Objekte) unterstützt. `... WHERE properties.desired.config = properties.reported.config` wird beispielsweise nur unterstützt, wenn diese Eigenschaften über primitive Werte verfügen.

## <a name="get-started-with-jobs-queries"></a>Erste Schritte mit Auftragsabfragen
[Aufträge][lnk-jobs] bieten eine Möglichkeit zum Ausführen von Vorgängen für Gerätegruppen. Jeder Gerätezwilling enthält die Informationen der auf ihn bezogenen Aufträge in einer Sammlung namens **jobs**.
Die logische Struktur entspricht der folgenden:

        {                                                                      
            "deviceId": "myDeviceId",                                            
            "etag": "AAAAAAAAAAc=",                                              
            "tags": {                                                            
                ...                                                              
            },                                                                   
            "properties": {                                                      
                ...                                                                 
            },
            "jobs": [
                {
                    "deviceId": "myDeviceId",
                    "jobId": "myJobId",    
                    "jobType": "scheduleTwinUpdate",            
                    "status": "completed",                    
                    "startTimeUtc": "2016-09-29T18:18:52.7418462",
                    "endTimeUtc": "2016-09-29T18:20:52.7418462",
                    "createdDateTimeUtc": "2016-09-29T18:18:56.7787107Z",
                    "lastUpdatedDateTimeUtc": "2016-09-29T18:18:56.8894408Z",
                    "outcome": {
                        "deviceMethodResponse": null   
                    }                                         
                },
                ...
            ]                                                             
        }

Diese Sammlung kann derzeit in der IoT Hub-Abfragesprache über **devices.jobs** abgefragt werden.

> [!IMPORTANT]
> Die jobs-Eigenschaft wird derzeit niemals zurückgegeben, wenn Gerätezwillinge abgefragt werden (d.h. Abfragen, die „FROM devices“ enthalten). Darauf kann nur direkt mit Abfragen und Verwendung von `FROM devices.jobs` zugegriffen werden.
>
>

Um alle Aufträge (vergangene und geplante) abzurufen, die ein einzelnes Gerät betreffen, können Sie z.B. die folgende Abfrage verwenden:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Beachten Sie, wie diese Abfrage den gerätespezifischen Status (und möglicherweise die direkte Antwortmethode) jedes zurückgegebenen Auftrags bereitstellt.
Es ist auch möglich, mit beliebigen booleschen Bedingungen alle Objekteigenschaften in der Sammlung **devices.jobs** zu filtern.
Die folgende Abfrage

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

ruft beispielsweise alle abgeschlossen Gerätezwilling-Aktualisierungsaufträge für das Gerät **myDeviceId** ab, die nach September 2016 erstellt wurden.

Die Ergebnisse pro Gerät können auch in einem einzelnen Auftrag abgerufen werden.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Einschränkungen
Derzeit wird für Abfragen von **devices.jobs** Folgendes nicht unterstützt:

* Projektionen, sodass nur `SELECT *` möglich ist.
* Bedingungen, die zusätzlich zu Auftragseigenschaften auf einen Gerätezwilling verweisen (siehe vorausgehender Abschnitt).
* Durchführung von Aggregationen, z.B. Zählen, Durchschnittsbildung, Gruppieren.

## <a name="get-started-with-device-to-cloud-message-routes-query-expressions"></a>Erste Schritte mit D2C-Nachrichtenrouten-Abfrageausdrücken

Mit [D2C-Routen][lnk-devguide-messaging-routes] können Sie IoT Hub dazu konfigurieren, D2C-Nachrichten basierend auf Ausdrücken, die für einzelne Nachrichten ausgewertet werden, an verschiedene Endpunkte zu senden.

Die [Routenbedingung][lnk-query-expressions] verwendet die gleiche IoT Hub-Abfragesprache wie Bedingungen in Zwillings- und Auftragsabfragen. Routenbedingungen werden anhand der Nachrichteneigenschaften unter Annahme folgender JSON-Darstellung ausgewertet:

        {
            "$messageId": "",
            "$enqueuedTime": "",
            "$to": "",
            "$expiryTimeUtc": "",
            "$correlationId": "",
            "$userId": "",
            "$ack": "",
            "$connectionDeviceId": "",
            "$connectionDeviceGenerationId": "",
            "$connectionAuthMethod": "",
            "$content-type": "",
            "$content-encoding": ""

            "userProperty1": "",
            "userProperty2": ""
        }

Nachrichtensystemeigenschaften ist das Symbol `'$'` vorangestellt.
Auf Benutzereigenschaften wird stets über deren Namen zugegriffen. Wenn der Name einer Benutzereigenschaft mit dem einer Systemeigenschaft übereinstimmt (wie z.B. `$to`), wird die Benutzereigenschaft mit dem Ausdruck `$to` abgerufen.
Sie können stets mithilfe von Klammern `{}` auf die Systemeigenschaft zugreifen. Sie können beispielsweise über den Ausdruck `{$to}` auf die Systemeigenschaft `to` zuweisen. Über Eigenschaftsnamen in Klammern wird stets die entsprechende Systemeigenschaft abgerufen.

Beachten Sie, dass bei Eigenschaftennamen nicht zwischen Groß- und Kleinschreibung unterschieden wird.

> [!NOTE]
> Alle Nachrichteneigenschaften sind Zeichenfolgen. Systemeigenschaften, wie im [Entwicklerhandbuch][lnk-devguide-messaging-format] beschrieben, sind derzeit nicht zur Verwendung in Abfragen verfügbar.
>

Wenn Sie z.B. eine `messageType`-Eigenschaft verwenden, möchten Sie vielleicht alle Telemetrie zu einem bestimmten Endpunkt weiterleiten und alle Warnungen zu einem anderen Endpunkt. Sie können den folgenden Ausdruck schreiben, um die Telemetrie weiterzuleiten:

        messageType = 'telemetry'

Schreiben Sie dann den folgenden Ausdruck zum Weiterleiten der Warnmeldungen:

        messageType = 'alert'

Boolesche Ausdrücke und Funktionen werden ebenfalls unterstützt. Anhand dieses Features können Sie Schweregrade unterscheiden, z.B.:

        messageType = 'alerts' AND as_number(severity) <= 2

Im Abschnitt [Ausdrücke und Bedingungen][lnk-query-expressions] finden Sie die vollständige Liste der unterstützten Operatoren und Funktionen.

## <a name="basics-of-an-iot-hub-query"></a>Grundlagen von IoT Hub-Abfragen
Jede IoT Hub-Abfrage besteht aus einer SELECT- und einer FROM-Klausel sowie optionalen WHERE- und GROUP BY-Klauseln. Jede Abfrage wird für eine Sammlung von JSON-Dokumenten ausgeführt, z.B. Gerätezwillinge. Die FROM-Klausel zeigt die Dokumentsammlung an, die durchlaufen werden soll (**devices** oder **devices.jobs**). Anschließend wird der Filter in der WHERE-Klausel angewendet. Mit Aggregationen werden die Ergebnisse dieses Schritts gemäß der GROUP BY-Klausel gruppiert, und es wird für jede Gruppe eine Zeile gemäß der SELECT-Klausel generiert.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>Die FROM-Klausel
Die **FROM <Spezifikation>**-Klausel kann nur zwei Werte annehmen: **FROM devices** zum Abfragen von Gerätezwillingen oder **FROM devices.jobs** zum Abfragen der Details pro Gerät für den Auftrag.

## <a name="where-clause"></a>WHERE-Klausel
Die **WHERE <Filterbedingung>**-Klausel ist optional. Sie gibt eine oder mehrere Bedingungen an, die von den in der FROM-Sammlung enthaltenen JSON-Dokumenten erfüllt werden müssen, um als Teil des Ergebnisses zurückgegeben zu werden. Jedes JSON-Dokument muss die angegebenen Bedingungen erfüllen, um in das Ergebnis einbezogen zu werden.

Die zulässigen Bedingungen werden im Abschnitt [Ausdrücke und Bedingungen][lnk-query-expressions] beschrieben.

## <a name="select-clause"></a>Die SELECT-Klausel
Die SELECT-Klausel (**SELECT <Liste>**) ist obligatorisch und gibt an, welche Werte von der Abfrage abgerufen werden. Sie gibt die JSON-Werte an, mit denen die neuen JSON-Objekte erstellt werden sollen.
Für jedes Element der gefilterten (und optional gruppierten) Teilmenge der FROM-Sammlung generiert die Projektionsphase ein neues JSON-Objekt, das aus den in der SELECT-Klausel angegebenen Werten erstellt wird.

Dies ist die Grammatik der SELECT-Klausel:

        SELECT [TOP <max number>] <projection list>

        <projection_list> ::=
            '*'
            | <projection_element> AS alias [, <projection_element> AS alias]+

        <projection_element> :==
            attribute_name
            | <projection_element> '.' attribute_name
            | <aggregate>

        <aggregate> :==
            count()
            | avg(<projection_element>)
            | sum(<projection_element>)
            | min(<projection_element>)
            | max(<projection_element>)

Dabei bezieht sich **attribute_name** auf jede Eigenschaft des JSON-Dokuments in der FROM-Sammlung. Im Abschnitt [Erste Schritte mit Gerätezwillingsabfragen][lnk-query-getstarted] finden Sie einige Beispiele für SELECT-Klauseln.

Derzeit werden andere Auswahlklauseln als **SELECT\*** nur in Aggregatabfragen für Gerätezwillinge unterstützt.

## <a name="group-by-clause"></a>GROUP BY-Klausel
Die **GROUP BY <Gruppierungsspezifikation>**-Klausel ist ein optionaler Schritt, der nach dem in der WHERE-Klausel angegebenen Filter und vor der in der SELECT-Klausel angegebenen Projektion ausgeführt werden kann. Sie gruppiert Dokumente anhand des Werts eines Attributs. Diese Gruppen werden verwendet, um aggregierte Werte gemäß der SELECT-Klausel zu generieren.

Ein Beispiel für eine Abfrage mit GROUP BY:

        SELECT properties.reported.telemetryConfig.status AS status,
            COUNT() AS numberOfDevices
        FROM devices
        GROUP BY properties.reported.telemetryConfig.status

Die formale Syntax für GROUP BY lautet:

        GROUP BY <group_by_element>
        <group_by_element> :==
            attribute_name
            | < group_by_element > '.' attribute_name

Dabei bezieht sich **attribute_name** auf jede Eigenschaft des JSON-Dokuments in der FROM-Sammlung.

Die GROUP BY-Klausel wird derzeit nur für Abfragen von Gerätezwillingen unterstützt.

## <a name="expressions-and-conditions"></a>Ausdrücke und Bedingungen
Auf hoher Ebene wird ein *Ausdruck*:

* in eine Instanz eines JSON-Typs (z.B. boolescher Wert, Zahl, Zeichenfolge, Array oder Objekt) ausgewertet und
* durch das Ändern von Daten aus dem JSON-Dokument des Geräts und Konstanten mit integrierten Operatoren und Funktionen definiert.

*Bedingungen* sind Ausdrücke, die als boolescher Wert ausgewertet werden. Jede Konstante außer dem booleschen Wert **true** wird als **false** betrachtet (einschließlich **NULL**, **undefiniert**, jeder Objekt- oder Arrayinstanz, jeder Zeichenfolge und selbstverständlich des booleschen Werts **false**).

Die Syntax für Ausdrücke lautet:

        <expression> ::=
            <constant> |
            attribute_name |
            <function_call> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

        <function_call> ::=
            <function_name> '(' expression ')'

        <constant> ::=
            <undefined_constant>
            | <null_constant>
            | <number_constant>
            | <string_constant>
            | <array_constant>

        <undefined_constant> ::= undefined
        <null_constant> ::= null
        <number_constant> ::= decimal_literal | hexadecimal_literal
        <string_constant> ::= string_literal
        <array_constant> ::= '[' <constant> [, <constant>]+ ']'

Hinweis:

| Symbol | Definition |
| --- | --- |
| attribute_name | Eine beliebige Eigenschaft des JSON-Dokuments in der **FROM**-Sammlung. |
| binary_operator | Ein beliebiger binärer, im Abschnitt [Operatoren](#operators) aufgelisteter Operator. |
| function_name| Eine beliebige im Abschnitt [Funktionen](#functions) aufgelistete Funktion. |
| decimal_literal |Ein Gleitkommawert, ausgedrückt in Dezimalschreibweise. |
| hexadecimal_literal |Eine Zahl, ausgedrückt durch die Zeichenfolge „0x“ gefolgt von einer Zeichenfolge von Hexadezimalzahlen. |
| string_literal |Zeichenfolgenliterale sind Unicode-Zeichenfolgen, die durch eine Sequenz aus null oder mehr Unicode-Zeichen oder Escapesequenzen dargestellt werden. Zeichenfolgenliterale werden in einfache Anführungszeichen (Apostrophen: ') oder doppelte Anführungszeichen (Anführungszeichen: ") eingeschlossen. Zulässige Escapezeichen: `\'`, `\"`, `\\`, `\uXXXX` für Unicode-Zeichen, die durch 4 Hexadezimalstellen definiert werden. |

### <a name="operators"></a>Operatoren
Die folgenden Operatoren werden unterstützt:

| Familie | Operatoren |
| --- | --- |
| Arithmetik |+,-,*,/,% |
| Logisch |AND, OR, NOT |
| Vergleich |=, !=, <, >, <=, >=, <> |

### <a name="functions"></a>Functions
Bei Abfragen von Zwillingen und Aufträgen wird nur folgende Funktion unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| IS_DEFINED(Eigenschaft) | Gibt einen booleschen Wert zurück, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde (inklusive `null`). |

In Routenbedingungen werden die folgenden mathematischen Funktionen unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| ABS(x) | Gibt den absoluten (positiven) Wert des angegebenen numerischen Ausdrucks zurück. |
| EXP(x) | Gibt den Exponentialwert des angegebenen numerischen Ausdrucks (e^x) zurück. |
| POWER(x,y) | Gibt den Wert des angegebenen Ausdrucks gemäß der angegebenen Potenz (x^y) zurück.|
| SQUARE(x) | Gibt das Quadrat des angegebenen numerischen Werts zurück. |
| CEILING(x) | Gibt den kleinsten ganzzahligen Wert zurück, der größer oder gleich dem angegebenen numerischen Ausdruck ist. |
| FLOOR(x) | Gibt den größten ganzzahligen Wert zurück, der kleiner oder gleich dem angegebenen numerischen Ausdruck ist. |
| SIGN(x) | Gibt das positive Vorzeichen (+1), null (0) oder das negative Vorzeichen (-1) des angegebenen numerischen Ausdrucks zurück.|
| SQRT(x) | Gibt die Quadratwurzel des angegebenen numerischen Werts zurück. |

In Routenbedingungen werden die folgenden Typüberprüfungs- und Umwandlungsfunktionen unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| AS_NUMBER | Konvertiert die Eingabezeichenfolge in eine Zahl. `noop`, wenn die Eingabe eine Zahl ist; `Undefined`, wenn die Zeichenfolge keine Zahl darstellt.|
| IS_ARRAY | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Array“ ist. |
| IS_BOOL | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „boolesch“ ist. |
| IS_DEFINED | Gibt einen booleschen Wert zurück, um anzugeben, ob der Eigenschaft ein Wert zugewiesen wurde. |
| IS_NULL | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „NULL“ ist. |
| IS_NUMBER | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zahl“ ist. |
| IS_OBJECT | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „JSON-Objekt“ ist. |
| IS_PRIMITIVE | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Grundtyp“ ist (Zeichenfolge, boolesch, numerisch oder `null`). |
| IS_STRING | Gibt einen booleschen Wert zurück, der angibt, ob der angegebene Ausdruck vom Typ „Zeichenfolge“ ist. |

In Routenbedingungen werden die folgenden Zeichenfolgenfunktionen unterstützt:

| Funktion | Beschreibung |
| -------- | ----------- |
| CONCAT(x, …) | Gibt eine Zeichenfolge zurück, die das Ergebnis der Verkettung von zwei oder mehr Zeichenfolgenwerten darstellt. |
| LENGTH(x) | Gibt die Anzahl der Zeichen im angegebenen Zeichenfolgenausdruck zurück.|
| LOWER(x) | Gibt eine Zeichenfolge zurück, nachdem Großbuchstaben in Kleinbuchstaben konvertiert wurden. |
| UPPER(x) | Gibt eine Zeichenfolge zurück, nachdem Kleinbuchstaben in Großbuchstaben konvertiert wurden. |
| SUBSTRING(Zeichenfolge, Start [, Länge]) | Gibt einen Teil eines Zeichenfolgenausdrucks zurück. Das angegebene Zeichen ist der Nullpunkt, von dem ab die Teilzeichenfolge in angegebener Länge bzw. bis zum Ende der Zeichenfolge zurückgegeben wird. |
| INDEX_OF(Zeichenfolge, Fragment) | Gibt die Anfangsposition des ersten Vorkommens des zweiten Zeichenfolgenausdrucks innerhalb des ersten angegebenen Zeichenfolgenausdrucks zurück, oder -1, wenn die Zeichenfolge nicht gefunden wird.|
| STARTS_WITH(x, y) | Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten beginnt. |
| ENDS_WITH(x, y) | Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck mit dem zweiten endet. |
| CONTAINS(x,y) | Gibt einen booleschen Wert zurück, um anzugeben, ob der erste Zeichenfolgenausdruck den zweiten enthält. |

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich darüber, wie Sie Abfragen in Ihren Apps mit [Azure IoT SDKs][lnk-hub-sdks] ausführen.

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#get-started-with-device-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-devguide-messaging-routes]: iot-hub-devguide-messaging.md#routing-rules
[lnk-devguide-messaging-format]: iot-hub-devguide-messaging.md#message-format


[lnk-hub-sdks]: iot-hub-devguide-sdks.md

