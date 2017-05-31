---
title: "Grundlegendes zu Azure IoT Hub-Gerätezwillingen | Microsoft-Dokumentation"
description: "Entwicklerhandbuch: Verwenden Sie Gerätezwillinge zum Synchronisieren von Status und Daten zwischen IoT Hub und Ihren Geräten."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/09/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 18d4994f303a11e9ce2d07bc1124aaedf570fc82
ms.openlocfilehash: 5bda9ce182c93d23bf29fd211ccdeb6facacbb7e
ms.contentlocale: de-de
ms.lasthandoff: 05/09/2017


---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Verstehen und Verwenden von Gerätezwillingen in IoT Hub
## <a name="overview"></a>Übersicht
*Gerätezwillinge* sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. IoT Hub speichert einen Gerätezwilling für jedes Gerät, das Sie mit IoT Hub verbinden. Dieser Artikel beschreibt Folgendes:

* Die Struktur des Gerätezwillings (*Tags*, *gewünschte Eigenschaften* und *gemeldete Eigenschaften*)
* Die Vorgänge, die Geräte-Apps und Back-Ends auf Gerätezwillingen ausführen können

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen. Anweisungen zum Konvertieren vorhandener Geräte-Apps für die Verwendung von MQTT finden Sie im Artikel [MQTT-Unterstützung][lnk-devguide-mqtt].
> 
> 

### <a name="when-to-use"></a>Einsatzgebiete
Verwenden Sie Gerätezwillinge für Folgendes:

* Speichern gerätespezifischer Metadaten in der Cloud – beispielsweise den Aufstellungsort eines Automaten.
* Melden aktueller Zustandsinformationen – beispielsweise verfügbare Funktionen und Bedingungen aus Ihrer Geräte-App. Beispiel: Ein Gerät ist per Mobilfunk oder WLAN mit Ihrem IoT Hub verbunden.
* Synchronisieren des Zustands von Workflows mit langer Ausführungsdauer zwischen Geräte-App und Back-End-App. Beispielsweise wenn das Lösungs-Back-End die neu zu installierende Firmwareversion angibt und die Geräte-App die verschiedenen Phasen des Aktualisierungsvorgangs meldet.
* Abfragen von Metadaten, Konfiguration oder Status des Geräts

Weitere Informationen zur Verwendung von gemeldeten Eigenschaften, D2C-Nachrichten und Dateiuploads finden Sie bei Bedarf im [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance].
Weitere Informationen zu gewünschten Eigenschaften, direkten Methoden und C2D-Nachrichten finden Sie im [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance].

## <a name="device-twins"></a>Gerätezwillinge
Auf Gerätezwillingen werden gerätespezifische Informationen gespeichert, auf die Folgendes zutrifft:

* Geräte und Back-Ends können mit ihnen Gerätezustände und -konfigurationen synchronisieren.
* Das Lösungs-Back-End kann mit ihnen lang andauernde Vorgänge abfragen und als Ziel verwenden.

Der Lebenszyklus eines Gerätezwillings ist mit der entsprechenden [Geräteidentität][lnk-identity] verknüpft. Gerätezwillinge werden implizit erstellt und gelöscht, wenn in IoT Hub eine Geräteidentität neu erstellt oder gelöscht wird.

Ein Gerätezwilling ist ein JSON-Dokument, das Folgendes enthält:

* **Tags:** Ein Abschnitt des JSON-Dokuments, in dem das Lösungs-Back-End Lese- und Schreibvorgänge ausführen kann. Tags sind für Geräte-Apps nicht sichtbar.
* **Gewünschte Eigenschaften:** Werden in Verbindung mit gemeldeten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gewünschte Eigenschaften können nur vom Lösungs-Back-End festgelegt und von der Geräte-App gelesen werden. Die Geräte-App kann auch in Echtzeit über Änderungen an den gewünschten Eigenschaften benachrichtigt werden.
* **Gemeldete Eigenschaften:** Werden in Verbindung mit gewünschten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gemeldete Eigenschaften können nur von der Geräte-App festgelegt und vom Lösungs-Back-End gelesen und abgefragt werden.

Darüber hinaus enthält der Stamm des JSON-Dokuments für einen Gerätezwilling die schreibgeschützten Eigenschaften der zugehörigen Geräteidentität aus der [Identitätsregistrierung][lnk-identity].

![][img-twin]

Das folgende Beispiel zeigt das JSON-Dokument für einen Gerätezwilling:

        {
            "deviceId": "devA",
            "generationId": "123",
            "status": "enabled",
            "statusReason": "provisioned",
            "connectionState": "connected",
            "connectionStateUpdatedTime": "2015-02-28T16:24:48.789Z",
            "lastActivityTime": "2015-02-30T16:24:48.789Z",

            "tags": {
                "$etag": "123",
                "deploymentLocation": {
                    "building": "43",
                    "floor": "1"
                }
            },
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata" : {...},
                    "$version": 1
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

Im Stammobjekt befinden sich die Systemeigenschaften und Containerobjekte für `tags` sowie die `reported`- und `desired`-Eigenschaften. Der Container `properties` enthält einige schreibgeschützte Elemente (`$metadata`, `$etag` und `$version`), die in den Abschnitten zu den [Metadaten des Gerätezwillings][lnk-twin-metadata] und zur [optimistischen Parallelität][lnk-concurrency] beschrieben sind.

### <a name="reported-property-example"></a>Beispiel für eine gemeldete Eigenschaft
Im vorherigen Beispiel enthält der Gerätezwilling eine `batteryLevel`-Eigenschaft, die von der Geräte-App gemeldet wird. Mit dieser Eigenschaft können Geräte anhand des letzten gemeldeten Akkustands abgefragt und Aktionen auf ihnen ausgeführt werden. Ein weiteres Beispiel wäre das Melden von Gerätefunktionen oder Verbindungsoptionen durch das Gerät.

> [!NOTE]
> Gemeldete Eigenschaften tragen zur Vereinfachung von Szenarien bei, in denen das Lösungs-Back-End den letzten bekannten Wert einer Eigenschaft abfragen soll. Verwenden Sie [Gerät-zu-Cloud-Nachrichten][lnk-d2c], wenn das Lösungs-Back-End Gerätetelemetriedaten in Form von Folgen von Ereignissen mit Zeitstempeln abfragen muss (beispielsweise als Zeitreihe).

### <a name="desired-property-example"></a>Beispiel für eine gewünschte Eigenschaft
Im vorherigen Beispiel werden die gewünschten und gemeldeten Eigenschaften des `telemetryConfig`-Gerätezwillings vom Lösungs-Back-End und von der Geräte-App verwendet, um die Telemetriekonfiguration für dieses Gerät zu synchronisieren. Beispiel:

1. Das Lösungs-Back-End legt die gewünschte Eigenschaft mit dem gewünschten Konfigurationswert fest. Hier sehen Sie den Teil des Dokuments mit der festgelegten gewünschten Eigenschaft:
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. Die Geräte-App wird sofort über die Änderung benachrichtigt, wenn sie verbunden ist oder später eine Verbindung herstellt. Anschließend meldet die Geräte-App die aktualisierte Konfiguration (oder einen Fehler über die `status`-Eigenschaft). Dies ist der Teil mit den gemeldeten Eigenschaften:
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. Das Lösungs-Back-End kann die Ergebnisse des Konfigurationsvorgangs auf vielen Geräten durch [Abfragen][lnk-query] des Gerätezwillings nachverfolgen.

> [!NOTE]
> Die obigen Codeausschnitte sind Beispiele mit optimierter Lesbarkeit und stellen eine Möglichkeit zum Codieren einer Konfiguration und des Status eines Geräts dar. IoT Hub setzt kein bestimmtes Schema für die gewünschten und gemeldeten Eigenschaften in den Gerätezwillingen voraus.
> 
> 

Mithilfe von Zwillingen können Sie Vorgänge mit langer Ausführungsdauer (beispielsweise Firmwareupdates) synchronisieren. Weitere Informationen zur Verwendung von Eigenschaften zum geräteübergreifenden Synchronisieren und Verfolgen von Vorgängen mit langer Ausführungsdauer finden Sie unter [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-properties].

## <a name="back-end-operations"></a>Back-End-Vorgänge
Das Lösungs-Back-End greift mithilfe folgender atomarer Vorgänge, die über HTTP verfügbar gemacht werden, auf den Gerätezwilling zu:

1. **Abrufen des Gerätezwillings mittels ID**. Dieser Vorgang gibt das Dokument für den Gerätezwilling zurück – einschließlich Tags und gewünschter Eigenschaften, gemeldeter Eigenschaften und Systemeigenschaften.
2. **Partielles Aktualisieren des Gerätezwillings**. Dieser Vorgang ermöglicht es dem Lösungs-Back-End, die Tags oder gewünschten Eigenschaften in einem Gerätezwilling teilweise zu aktualisieren. Bei der partiellen Aktualisierung handelt es sich um ein JSON-Dokument, das eine beliebige Eigenschaft hinzufügt oder aktualisiert. Auf `null` festgelegte Eigenschaften werden entfernt. Im folgenden Beispiel wird eine neue gewünschte Eigenschaft mit dem Wert `{"newProperty": "newValue"}` erstellt, der vorhandene Wert von `existingProperty` wird mit `"otherNewValue"` überschrieben, und `otherOldProperty` wird entfernt. Ansonsten werden an vorhandenen Eigenschaften oder Tags keine weiteren Änderungen vorgenommen:
   
        {
            "properties": {
                "desired": {
                    "newProperty": {
                        "nestedProperty": "newValue"
                    },
                    "existingProperty": "otherNewValue",
                    "otherOldProperty": null
                }
            }
        }
3. **Ersetzen gewünschter Eigenschaften**. Dieser Vorgang ermöglicht dem Lösungs-Back-End, alle vorhandenen gewünschten Eigenschaften vollständig zu überschreiben und ein neues JSON-Dokument für `properties/desired` bereitzustellen.
4. **Ersetzen von Tags**. Dieser Vorgang ermöglicht es dem Lösungs-Back-End, alle vorhandenen Tags vollständig zu überschreiben und ein neues JSON-Dokument für `tags` bereitzustellen.
5. **Zwillingsbenachrichtigungen empfangen**. Mit diesem Vorgang kann das Lösungs-Back-End benachrichtigt werden, wenn der Zwilling geändert wird. Zu diesem Zweck muss Ihre IoT-Lösung eine Route erstellen die Datenquelle auf *twinChangeEvents* festlegen. Standardmäßig werden keine Zwillingsbenachrichtigungen gesendet, und es existieren noch keine solchen Routen. Wenn die Änderungsrate zu hoch ist, oder andere Gründe wie z.B. interne Fehler vorliegen, sendet der IoT Hub möglicherweise nur eine Benachrichtigung, die alle Änderungen enthält. Wenn Ihre Anwendung zuverlässige Prüfungen und Protokolle aller Zwischenzustände benötigt, empfehlen wir Ihnen, D2C-Nachrichten zu verwenden. Die Zwillingsbenachrichtung umfasst Eigenschaften und einen Textkörper.

    - Eigenschaften

    | Name | Wert |
    | --- | --- |
    $content-type | Anwendung/json |
    $iothub-enqueuedtime |  Uhrzeit, zu der die Benachrichtigung gesendet wurde |
    $iothub-message-source | twinChangeEvents |
    $content-encoding | UTF-8 |
    deviceId | ID des Geräts |
    hubName | Name des IoT Hub |
    operationTimestamp | ISO8601-Zeitstempel des Vorgangs |
    iothub-message-schema | deviceLifecycleNotification |
    opType | "replaceTwin" oder "updateTwin" |

    Nachrichtensystemeigenschaften ist das Symbol `'$'` vorangestellt.

    - body
        
    Dieser Abschnitt enthält alle Zwillingsänderungen in einem JSON-Format. Er verwendet das gleiche Format wie ein Patch, jedoch mit dem Unterschied, dass alle Zwillingsabschnitte enthalten sein können (Tags, properties.reported, properties.desired) und dass die $metadata-Elemente enthalten sind. Beispiel:
    ```
    {
        "properties": {
            "desired": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            },
            "reported": {
                "$metadata": {
                    "$lastUpdated": "2016-02-30T16:24:48.789Z"
                },
                "$version": 1
            }
        }
    }
    ``` 

Alle oben genannten Vorgänge unterstützen [optimistische Parallelität][lnk-concurrency] und erfordern die Berechtigung **ServiceConnect**, wie im Artikel [Sicherheit][lnk-security] definiert.

Neben diesen Vorgängen kann das Lösungs-Back-End auch folgende Aktionen ausführen:

* Abfragen der Gerätezwillinge mithilfe der SQL-ähnlichen [IoT Hub-Abfragesprache][lnk-query]
* Ausführen von Vorgängen für große Mengen von Gerätezwillingen mithilfe von [Aufträgen][lnk-jobs]

## <a name="device-operations"></a>Gerätevorgänge
Die Geräte-App greift mithilfe folgender atomarer Vorgänge auf den Gerätezwilling zu:

1. **Abrufen des Gerätezwillings**. Dieser Vorgang gibt das Dokument für den Gerätezwilling für das derzeit verbundene Gerät zurück – einschließlich Tags und gewünschter Eigenschaften, gemeldeter Eigenschaften und Systemeigenschaften.
2. **Teilweises Aktualisieren gemeldeter Eigenschaften**. Dieser Vorgang ermöglicht die teilweise Aktualisierung der gemeldeten Eigenschaften des derzeit verbundenen Geräts. Dabei wird das gleiche JSON-Updateformat wie bei der partiellen Aktualisierung der gewünschten Eigenschaften durch das Lösungs-Back-End verwendet.
3. **Beobachten gewünschter Eigenschaften**. Das derzeit verbundene Gerät kann benachrichtigt werden, sobald die gewünschten Eigenschaften aktualisiert werden. Das Gerät erhält die gleiche Form der Aktualisierung (partielle oder vollständige Ersetzung), die durch das Lösungs-Back-End ausgeführt wird.

Alle oben genannten Vorgänge erfordern die Berechtigung **DeviceConnect**, wie im Artikel [Sicherheit][lnk-security] definiert.

Die [Azure IoT-Geräte-SDKs][lnk-sdks] vereinfachen die Verwendung der oben genannten Vorgänge mit vielen Sprachen und Plattformen. Weitere Informationen zu den Details der IoT Hub-Grundtypen für die Synchronisierung gewünschter Eigenschaften finden Sie im [Ablauf zur Wiederherstellung der Geräteverbindung][lnk-reconnection].

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen.
> 
> 

## <a name="reference-topics"></a>Referenzthemen:
Die folgenden Referenzthemen enthalten weitere Informationen zum Steuern des Zugriffs auf Ihren IoT Hub.

## <a name="tags-and-properties-format"></a>Format von Tags und Eigenschaften
Tags, gewünschte und gemeldete Eigenschaften sind JSON-Objekte mit den folgenden Einschränkungen:

* Alle Schlüssel in JSON-Objekten sind UTF-8 UNICODE-Zeichenfolgen mit 64 Bytes, bei denen die Groß- und Kleinschreibung berücksichtigt werden muss. UNICODE-Steuerzeichen (Segmente C0 und C1) sowie `'.'`, `' '` und `'$'` gehören nicht zu den zulässigen Zeichen.
* Alle Werte in JSON-Objekten können die folgenden JSON-Typen aufweisen: boolescher Wert, Zahl, Zeichenfolge, Objekt. Arrays sind nicht zulässig.
* Alle JSON-Objekte in Tags, gewünschten und gemeldeten Eigenschaften können eine maximale Tiefe von 5 haben. Das folgende Objekt ist z.B. gültig:

        {
            ...
            "tags": {
                "one": {
                    "two": {
                        "three": {
                            "four": {
                                "five": {
                                    "property": "value"
                                }
                            }
                        }
                    }
                }
            },
            ...
        }

* Alle Zeichenfolgenwerte können höchstens 512Byte lang sein.

## <a name="device-twin-size"></a>Größe des Gerätezwillings
IoT Hub erzwingt eine Größenbegrenzung von 8 KB für die Werte von `tags`, `properties/desired` und `properties/reported`, ausgenommen schreibgeschützte Elemente.
Die Größe wird durch Zusammenzählen aller Zeichen mit Ausnahme von UNICODE-Steuerzeichen (Segmente C0 und C1) und Leerzeichen `' '` außerhalb einer Zeichenfolgenkonstante berechnet.
IoT Hub gibt für alle Vorgänge, die die Größe dieser Dokumente über den Grenzwert hinaus erhöhen würden, einen Fehler zurück.

## <a name="device-twin-metadata"></a>Metadaten des Gerätezwillings
IoT Hub verwaltet den Zeitstempel der letzten Aktualisierung für jedes JSON-Objekt in den gewünschten und gemeldeten Eigenschaften des Gerätezwillings. Zeitstempel verwenden UTC und sind im [ISO8601]-Format codiert: `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Beispiel:

        {
            ...
            "properties": {
                "desired": {
                    "telemetryConfig": {
                        "sendFrequency": "5m"
                    },
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": {
                                "$lastUpdated": "2016-03-30T16:24:48.789Z"
                            },
                            "$lastUpdated": "2016-03-30T16:24:48.789Z"
                        },
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$version": 23
                },
                "reported": {
                    "telemetryConfig": {
                        "sendFrequency": "5m",
                        "status": "success"
                    }
                    "batteryLevel": "55%",
                    "$metadata": {
                        "telemetryConfig": {
                            "sendFrequency": "5m",
                            "status": {
                                "$lastUpdated": "2016-03-31T16:35:48.789Z"
                            },
                            "$lastUpdated": "2016-03-31T16:35:48.789Z"
                        }
                        "batteryLevel": {
                            "$lastUpdated": "2016-04-01T16:35:48.789Z"
                        },
                        "$lastUpdated": "2016-04-01T16:24:48.789Z"
                    },
                    "$version": 123
                }
            }
            ...
        }

Diese Informationen werden auf jeder Ebene (nicht nur die Zweige der JSON-Struktur) gespeichert, um Aktualisierungen beizubehalten, bei denen Objektschlüssel entfernt werden.

## <a name="optimistic-concurrency"></a>Optimistische Parallelität
Tags sowie gewünschte und gemeldete Eigenschaften unterstützen optimistische Parallelität.
Tags verfügen über ein ETag (gemäß [RFC7232]), das der JSON-Darstellung des Tags entspricht. ETags können bei bedingten Aktualisierungsvorgängen über das Lösungs-Back-End verwendet werden, um die Konsistenz zu gewährleisten.

Gewünschte und gemeldete Eigenschaften des Gerätezwillings verfügen zwar über keine ETags, aber über einen garantiert inkrementellen `$version`-Wert. Zur Gewährleistung der Konsistenz von Updates kann von der aktualisierenden Partei die Version genutzt werden (ähnlich wie ein ETag). Beispiele wären etwa eine Geräte-App für eine gemeldete Eigenschaft oder das Lösungs-Back-End für eine gewünschte Eigenschaft.

Versionen sind auch nützlich, wenn ein beobachtender Agent (beispielsweise die Geräte-App, die die gewünschten Eigenschaften beobachtet) Racebedingungen zwischen dem Ergebnis eines Abrufvorgangs und einer Aktualisierungsbenachrichtigung beheben muss. Im Abschnitt [Ablauf zur Wiederherstellung der Geräteverbindung][lnk-reconnection] finden Sie weitere Informationen.

## <a name="device-reconnection-flow"></a>Ablauf zur Wiederherstellung der Geräteverbindung
IoT Hub behält keine Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften für nicht verbundene Geräte bei. Ein Gerät, das eine Verbindung herstellt, muss daher zusätzlich zum Abonnieren von Aktualisierungsbenachrichtigungen das gesamte Dokument der gewünschten Eigenschaften abrufen. Wenn die Gefahr von Racebedingungen zwischen Aktualisierungsbenachrichtigungen und dem vollständigen Abruf besteht, muss der folgende Ablauf sichergestellt werden:

1. Geräte-App stellt eine Verbindung mit einem IoT Hub her.
2. Geräte-App abonniert Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften.
3. Geräte-App ruft das vollständige Dokument für gewünschte Eigenschaften ab.

Die Geräte-App kann alle Benachrichtigungen ignorieren, bei denen `$version` kleiner oder gleich der Version des vollständigen abgerufenen Dokuments ist. Diese Herangehensweise ist möglich, da IoT Hub sicherstellt, dass die Versionsnummern immer erhöht werden.

> [!NOTE]
> Diese Logik ist in den [Azure IoT-Geräte-SDKs][lnk-sdks] bereits implementiert. Diese Beschreibung ist nur dann nützlich, wenn die Geräte-App keine der Azure IoT-Geräte-SDKs verwenden kann und die MQTT-Schnittstelle direkt programmiert werden muss.
> 
> 

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Im Artikel [Referenz: IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge verfügbar macht.
* Unter [Kontingente und Drosselung][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, das bei Verwendung des Diensts zu erwarten ist.
* Im Artikel [Verstehen und Verwenden von Azure IoT SDKs][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Im Artikel [Referenz – IoT Hub-Abfragesprache für Gerätezwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* Der Artikel [Kommunikation mit Ihrem IoT Hub mithilfe des Protokolls MQTT][lnk-devguide-mqtt] enthält weitere Informationen zur IoT Hub-Unterstützung für das MQTT-Protokoll.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun etwas über Gerätezwillinge erfahren haben, sind möglicherweise die folgenden Themen aus dem IoT Hub-Entwicklerhandbuch für Sie interessant:

* [Aufrufen einer direkten Methode auf einem Gerät][lnk-methods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, sind möglicherweise die folgenden IoT Hub-Tutorials für Sie interessant:

* [Verwenden des Gerätezwillings][lnk-twin-tutorial]
* [Verwenden der Eigenschaften von Gerätezwillingen][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png

