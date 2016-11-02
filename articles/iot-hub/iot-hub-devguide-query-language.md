<properties
 pageTitle="Entwicklerhandbuch – Abfragesprache | Microsoft Azure"
 description="Azure IoT Hub-Entwicklerhandbuch – Beschreibung der Abfragesprache, die zum Abrufen von Informationen über Zwillinge, Methoden und Aufträge vom IoT Hub verwendet wird"
 services="iot-hub"
 documentationCenter=".net"
 authors="fsautomata"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="elioda"/>


# <a name="reference---query-language-for-twins-and-jobs"></a>Referenz – Abfragesprache für Zwillinge und Aufträge

## <a name="overview"></a>Übersicht

IoT Hub bietet eine leistungsstarke SQL-ähnliche Sprache zum Abrufen von Informationen zu [Gerätezwillingen][lnk-twins] und [Aufträgen][lnk-jobs]. Dieser Artikel enthält Folgendes:

* Eine Einführung in die wichtigsten Features der Abfragesprache von IoT Hubs
* Eine ausführliche Beschreibung der Sprache

## <a name="getting-started-with-twin-queries"></a>Erste Schritte mit Zwillingsabfragen

[Gerätezwillinge][lnk-twins] können beliebige JSON-Objekte als Tags und Eigenschaften enthalten. In IoT Hub können Gerätezwillinge als einzelnes JSON-Dokument abgefragt werden, das alle Informationen zum Zwilling enthält.
Angenommen, Ihre IoT Hub-Zwillinge weisen die folgende Struktur auf:

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

> [AZURE.NOTE] [IoT Hub-SDKs][lnk-hub-sdks] unterstützen das Paging umfangreicher Ergebnisse.

IoT Hub ermöglicht beim Abrufen von Zwillingen ein Filtern mit beliebigen Bedingungen. Sie ist es z. B.

        SELECT * FROM devices
        WHERE tags.location.region = 'US'

ruft die Zwillinge ab, deren Tag **location.region** auf **US** festgelegt ist.
Boolesche Operatoren und arithmetische Vergleiche werden ebenfalls unterstützt. Das Beispiel:

        SELECT * FROM devices
        WHERE tags.location.region = 'US'
            AND properties.reported.telemetryConfig.sendFrequencyInSecs >= 60

ruft alle Zwillinge ab, die sich in den USA befinden und so konfiguriert sind, dass Telemetriedaten seltener als minütlich gesendet werden. Zur Vereinfachung können auch Arraykonstanten in Verbindung mit den Operatoren **IN** und **NIN** (nicht IN) verwendet werden. Sie ist es z. B.

        SELECT * FROM devices
        WHERE property.reported.connectivity IN ['wired', 'wifi']

ruft alle Zwillinge ab, die WLAN oder eine kabelgebundene Verbindung gemeldet haben. Die vollständige Referenz zu den Filtermöglichkeiten finden Sie im Abschnitt zur [WHERE-Klausel][lnk-query-where].

Gruppierungen und Aggregationen werden ebenfalls unterstützt. Sie ist es z. B.

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

Das Beispiel oben veranschaulicht eine Situation, in der drei Geräte eine erfolgreiche Konfiguration gemeldet haben, zwei die Konfiguration noch übernehmen und eines einen Fehler gemeldet hat.

### <a name="c#-example"></a>C#-Beispiel

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
Es muss beachtet werden, dass das query-Objekt mehrmals **Next\*** verfügbar macht – je nach der für die Abfrage benötigten Deserialisierung, d.h. Zwillings- oder Auftragsobjekte oder reiner JSON-Code für Projektionen.

### <a name="node-example"></a>Node-Beispiel

Die Abfragefunktion wird durch das [Node-Dienst-SDK][lnk-hub-sdks] im **Registry**-Objekt verfügbar gemacht.
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
Es muss beachtet werden, dass das query-Objekt mehrmals **Next\*** verfügbar macht – je nach der für die Abfrage benötigten Deserialisierung, d.h. Zwillings- oder Auftragsobjekte oder reiner JSON-Code für Projektionen.

### <a name="limitations"></a>Einschränkungen

Derzeit werden Projektionen nur bei Verwendung von Aggregationen unterstützt, d.h., für nicht aggregierte Abfragen kann nur `SELECT *` verwendet werden. Darüber hinaus werden Aggregation nur in Verbindung mit Gruppierungen unterstützt.

## <a name="getting-started-with-jobs-queries"></a>Erste Schritte mit Auftragsabfragen

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

Um alle Aufträge (vergangene und geplante) abzurufen, die ein einzelnes Gerät betreffen, können Sie z.B. die folgende Abfrage verwenden:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'

Beachten Sie, wie diese Abfrage den gerätespezifischen Status (und möglicherweise die direkte Antwortmethode) jedes zurückgegebenen Auftrags bereitstellt.
Es ist auch möglich, mit beliebigen booleschen Bedingungen alle Eigenschaften der Objekte in der Sammlung **devices.jobs** zu filtern.
Die folgende Abfrage:

        SELECT * FROM devices.jobs
        WHERE devices.jobs.deviceId = 'myDeviceId'
            AND devices.jobs.jobType = 'scheduleTwinUpdate'
            AND devices.jobs.status = 'completed'
            AND devices.jobs.createdTimeUtc > '2016-09-01'

ruft z.B. alle abgeschlossen Zwillingsaktualisierungsaufträge für das Gerät **myDeviceId** ab, die nach September 2016 erstellt wurden.

Die Ergebnisse pro Gerät können auch in einem einzelnen Auftrag abgerufen werden.

        SELECT * FROM devices.jobs
        WHERE devices.jobs.jobId = 'myJobId'

### <a name="limitations"></a>Einschränkungen
Derzeit wird für Abfragen von **devices.jobs** Folgendes nicht unterstützt:

* Projektionen, d.h. nur `SELECT *` ist möglich
* Bedingungen, die zusätzlich zu Auftragseigenschaften wie oben dargestellt auf einen Gerätezwilling verweisen
* Durchführen von Aggregationen, z.B. Zählen, Durchschnittsbildung, Gruppieren

## <a name="basics-of-an-iot-hub-query"></a>Grundlagen von IoT Hub-Abfragen

Jede IoT Hub-Abfrage besteht aus einer SELECT- und einer FROM-Klausel sowie optionalen WHERE- und GROUP BY-Klauseln. Jede Abfrage wird auf eine Sammlung von JSON-Dokumenten ausgeführt, z.B. Gerätezwillinge. Die FROM-Klausel zeigt die Dokumentsammlung an, die durchlaufen werden soll (**devices** oder **devices.jobs**). Anschließend wird der Filter in der WHERE-Klausel angewendet. Im Fall von Aggregationen werden die Ergebnisse dieses Schritts gemäß der GROUP BY-Klausel gruppiert, und es wird für jede Gruppe eine Zeile gemäß der SELECT-Klausel generiert.

        SELECT <select_list>
        FROM <from_specification>
        [WHERE <filter_condition>]
        [GROUP BY <group_specification>]

## <a name="from-clause"></a>Die FROM-Klausel

Die **FROM <Spezifikation>**-Klausel kann nur zwei Werte annehmen: **FROM devices** zum Abfragen von Gerätezwillingen oder **FROM devices.jobs** zum Abfragen der Details pro Gerät für den Auftrag.

## <a name="where-clause"></a>WHERE-Klausel

Die **WHERE <Filterbedingung>**-Klausel ist optional. Sie gibt die Bedingungen an, die von den in der FROM-Sammlung enthaltenen JSON-Dokumenten erfüllt werden müssen, um als Teil des Ergebnisses zurückgegeben zu werden. Jedes JSON-Dokument muss die angegebenen Bedingungen erfüllen, um in das Ergebnis einbezogen zu werden.

Die zulässigen Bedingungen werden im Abschnitt [Ausdrücke und Bedingungen][lnk-query-expressions] beschrieben.

## <a name="select-clause"></a>Die SELECT-Klausel

Die SELECT-Klausel (**SELECT <Liste>**) ist obligatorisch und gibt an, welche Werte von der Abfrage abgerufen werden. Sie gibt die JSON-Werte an, mit denen die neuen JSON-Objekte erstellt werden sollen. Für jedes Element der gefilterten (und optional gruppierten) Teilmenge der FROM-Sammlung generiert die Projektionsphase ein neues JSON-Objekt, das aus den in der SELECT-Klausel angegebenen Werten erstellt wird.

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
            count(<projection_element>) | count()
            | avg(<projection_element>) | avg()
            | sum(<projection_element>) | sum()
            | min(<projection_element>) | min()
            | max(<projection_element>) | max()

Dabei bezieht sich **attribute_name** auf jede Eigenschaft des JSON-Dokuments in der FROM-Sammlung. Im Abschnitt [Erste Schritte mit Zwillingsabfragen][lnk-query-getstarted] finden Sie einige Beispiele für SELECT-Klauseln.

Derzeit werden andere Auswahlklauseln als **SELECT\*** nur in Aggregatabfragen für Zwillinge unterstützt.

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

Die GROUP BY-Klausel wird derzeit nur für Abfragen von Zwillingen unterstützt.

## <a name="expressions-and-conditions"></a>Ausdrücke und Bedingungen

Auf hoher Ebene wird ein *Ausdruck*:

* in eine Instanz eines JSON-Typs (d.h. boolescher Wert, Zahl, Zeichenfolge, Array oder Objekt) ausgewertet, und
* durch das Ändern von Daten aus dem JSON-Dokument des Geräts und Konstanten mit integrierten Operatoren und Funktionen definiert.

*Bedingungen* sind Ausdrücke, die als boolescher Wert ausgewertet werden, d.h., jede Konstante außer dem booleschen Wert **true** wird als **false** betrachtet (einschließlich **null**, **undefiniert**, jeder Objekt- oder Arrayinstanz, jeder Zeichenfolge und selbstverständlich des booleschen Werts **false**).

Die Syntax für Ausdrücke lautet:

        <expression> ::=
            <constant> |
            attribute_name |
            unary_operator <expression> |
            <expression> binary_operator <expression> |
            <create_array_expression> |
            '(' <expression> ')'

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
| -------------- | -----------------|
| attribute_name | Eine beliebige Eigenschaft des JSON-Dokuments in der FROM-Sammlung. |
| unary_operator | Ein beliebiger unärer Operator gemäß dem Abschnitt „Operatoren“. |
| binary_operator | Ein beliebiger binärer Operator gemäß dem Abschnitt „Operatoren“. |
| decimal_literal | Ein Gleitkommawert, ausgedrückt in Dezimalschreibweise. |
| hexadecimal_literal | Eine Zahl, ausgedrückt durch die Zeichenfolge „0x“ gefolgt von einer Zeichenfolge von Hexadezimalzahlen. |
| string_literal | Zeichenfolgenliterale sind Unicode-Zeichenfolgen, die durch eine Sequenz aus null oder mehr Unicode-Zeichen oder Escapesequenzen dargestellt werden. Zeichenfolgenliterale werden in einfache Anführungszeichen (Apostrophen: ') oder doppelte Anführungszeichen (Anführungszeichen: ") eingeschlossen. Zulässige Escapezeichen: `\'`, `\"`, `\\`, `\uXXXX` für Unicode-Zeichen, die durch 4 Hexadezimalstellen definiert werden. |

### <a name="operators"></a>Operatoren

Die folgenden Operatoren werden unterstützt:

| Familie | Operatoren |
| -------------- | -----------------|
| Arithmetik | +,-,*,/,% |
| Logisch | AND, OR, NOT |
| Vergleich |  =, !=, <, >, <=, >=, <> |

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie Abfragen in Ihren Apps mit [IoT Hub-SDKs][lnk-hub-sdks] ausführen.

[lnk-query-where]: iot-hub-devguide-query-language.md#where-clause
[lnk-query-expressions]: iot-hub-devguide-query-language.md#expressions-and-conditions
[lnk-query-getstarted]: iot-hub-devguide-query-language.md#getting-started-with-twin-queries

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md


<!--HONumber=Oct16_HO2-->


