---
title: Entwicklerhandbuch – Grundlegendes zu Gerätezwillingen | Microsoft Docs
description: Azure IoT Hub-Entwicklerhandbuch – Verwenden Sie Gerätezwillinge zum Synchronisieren von Status und Daten zwischen IoT Hub und Ihren Geräten
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: elioda

---
# <a name="understand-device-twins---preview"></a>Grundlegendes zu Gerätezwillingen – Vorschau
## <a name="overview"></a>Übersicht
*Gerätezwillinge* sind JSON-Dokumente, in denen Gerätestatusinformationen (Metadaten, Konfigurationen und Bedingungen) gespeichert werden. IoT Hub speichert einen Gerätezwilling für jedes Gerät, das Sie mit IoT Hub verbinden. In diesem Artikel wird Folgendes beschrieben:

* die Struktur des Gerätezwillings: *Tags*, *gewünschte* und *gemeldete Eigenschaften* und
* die Vorgänge, die Geräte-Apps und Back-Ends mit Gerätezwillingen durchführen können.

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen. Anweisungen zum Konvertieren vorhandener Geräte-Apps für die Verwendung von MQTT finden Sie im Artikel [MQTT-Unterstützung][lnk-devguide-mqtt].
> 
> 

### <a name="when-to-use"></a>Einsatzgebiete
Verwenden Sie Gerätezwillinge für Folgendes:

* Speichern gerätespezifischer Metadaten, wie z.B. des Aufstellungsort eines Verkaufsautomaten, in der Cloud
* Melden aktueller Statusinformationen, wie verfügbarer Funktionen und Zustände aus der Geräte-App, z.B. bei einem Gerät, das über das Mobilfunknetz oder WLAN eine Verbindung herstellt
* Synchronisieren des Status lang andauernder Workflows zwischen der Geräte-App und dem Back-End, z.B. zur Angabe der neuen zu installierenden Firmwareversion durch das Back-End und zum Melden der verschiedenen Phasen des Aktualisierungsvorgangs durch die Geräte-App
* Abfragen von Metadaten, Konfiguration oder Status des Geräts

Verwenden Sie [Gerät-zu-Cloud-Nachrichten][lnk-d2c] für Folgen von Ereignissen mit Zeitstempel, z.B. einer Zeitserie von Sensordaten oder Alarmen. Verwenden Sie [Cloud-zu-Gerät-Nachrichten][lnk-methods] für die interaktive Steuerung von Geräten, z.B. das Einschalten eines Lüfters.

## <a name="device-twins"></a>Gerätezwillinge
Auf Gerätezwillingen werden gerätespezifische Informationen gespeichert, auf die Folgendes zutrifft:

* Geräte und Back-Ends können mit ihnen Gerätezustände und -konfigurationen synchronisieren.
* Das Anwendungs-Back-End kann mit ihnen lang andauernde Vorgänge abfragen und als Ziel verwenden.

Der Lebenszyklus eines Gerätezwillings ist mit der entsprechenden [Geräteidentität][lnk-identity] verknüpft. Zwillinge werden implizit erstellt und gelöscht, wenn eine neue Geräteidentität in IoT Hub erstellt oder gelöscht wird.

Ein Gerätezwilling ist ein JSON-Dokument, das Folgendes enthält:

* **Tags**. Ein vom Back-End gelesenes und geschriebenes JSON-Dokument. Tags sind für Geräte-Apps nicht sichtbar.
* **Gewünschte Eigenschaften** werden in Verbindung mit gemeldeten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gewünschte Eigenschaften können nur vom Anwendungs-Back-End festgelegt werden. Das Lesen ist über die Geräte-App möglich. Die Geräte-App kann auch in Echtzeit über Änderungen an den gewünschten Eigenschaften benachrichtigt werden.
* **Gemeldete Eigenschaften** werden in Verbindung mit gewünschten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gemeldete Eigenschaften können nur von der Geräte-App festgelegt werden. Das Lesen und Abfragen ist vom Anwendungs-Back-End möglich.

Darüber hinaus enthält der Stamm des Gerätezwillings die schreibgeschützten Eigenschaften der zugehörigen Identität gemäß der [Registrierung der Geräteidentität][lnk-identity].

![][img-twin]

Dies ist ein Beispiel für das JSON-Dokument eines Gerätezwillings:

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

Im Stammobjekt befinden sich die Systemeigenschaften und Containerobjekte für `tags` sowie für `reported` und `desired properties`. Der Container `properties` enthält einige schreibgeschützte Elemente (`$metadata`, `$etag` und `$version`), die in den Abschnitten zu den [Metadaten des Zwillings][lnk-twin-metadata] und zur [optimistischen Parallelität][lnk-concurrency] beschrieben sind.

### <a name="reported-property-example"></a>Beispiel für eine gemeldete Eigenschaft
Im Beispiel oben enthält der Gerätezwilling eine `batteryLevel`-Eigenschaft, die von der Geräte-App gemeldet wird. Mit dieser Eigenschaft können Geräte anhand des letzten gemeldeten Akkustands abgefragt und Aktionen auf ihnen ausgeführt werden. Ein weiteres Beispiel wäre das Melden von Gerätefunktionen oder Verbindungsoptionen durch das Gerät.

Beachten Sie, dass durch gemeldete Eigenschaften Szenarios vereinfacht werden, in denen das Back-End den letzten bekannten Wert einer Eigenschaft abfragen soll. Verwenden Sie [Gerät-zu-Cloud-Nachrichten][lnk-d2c], wenn das Back-End Gerätetelemetriedaten in Form von Folgen von Ereignissen mit Zeitstempeln abfragen muss, z.B. als Zeitserie.

### <a name="desired-configuration-example"></a>Beispiel für eine gewünschte Konfiguration
Im Beispiel oben werden die gewünschten und gemeldeten Eigenschaften in `telemetryConfig` vom Back-End und der Geräte-App verwendet, um die Telemetriekonfiguration für dieses Gerät zu synchronisieren. Beispiel:

1. Das App-Back-End legt für die gewünschte Eigenschaft den gewünschten Konfigurationswert fest. Dies ist der Teil des Dokuments mit der gewünschten Eigenschaft:
   
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
3. Das App-Back-End kann die Ergebnisse des Konfigurationsvorgangs auf vielen Geräten durch [Abfragen][lnk-query] des Zwillings verfolgen.

> [!NOTE]
> Die obigen Codeausschnitte sind für die Lesbarkeit optimierte Beispiele. Sie stellen eine Möglichkeit zum Codieren einer Konfiguration und des Status eines Geräts dar. IoT Hub setzt kein bestimmtes Schema für die gewünschten und gemeldeten Eigenschaften in den Gerätezwillingen voraus.
> 
> 

In vielen Fällen werden Zwillinge zum Synchronisieren lang andauernder Vorgänge verwendet, z.B. Firmwareupdates. Weitere Informationen zur Verwendung von Eigenschaften zum Synchronisieren und Verfolgen lang andauernder Vorgänge über mehrere Geräte finden Sie unter [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-properties].

## <a name="back-end-operations"></a>Back-End-Vorgänge
Das Back-End greift mithilfe folgender atomarer Vorgänge, die über HTTP verfügbar gemacht werden, auf den Zwilling zu:

1. **Abrufen des Zwillings nach ID**. Dieser Vorgang gibt den Inhalt des Dokuments für den Zwilling zurück, einschließlich Tags und gewünschter, gemeldeter und Systemeigenschaften.
2. **Teilweises Aktualisieren des Zwillings**. Dieser Vorgang ermöglicht dem Back-End, die Tags oder gewünschten Eigenschaften des Zwillings teilweise zu aktualisieren. Die teilweise Aktualisierung hat die Form eines JSON-Dokuments, das angegebene Eigenschaft hinzufügt oder aktualisiert. Auf `null` festgelegte Eigenschaften werden entfernt. Der folgende Code erstellt z.B. eine neue gewünschte Eigenschaft mit dem Wert `{"newProperty": "newValue"}`, überschreibt den vorhandenen Wert von `existingProperty` durch `"otherNewValue"` und entfernt `otherOldProperty` vollständig. An anderen vorhandenen Eigenschaften oder Tags werden keine Änderungen vorgenommen:
   
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
3. **Ersetzen gewünschter Eigenschaften**. Dieser Vorgang ermöglicht dem Back-End, alle vorhandenen gewünschten Eigenschaften vollständig zu überschreiben und ein neues JSON-Dokument für `properties/desired` bereitzustellen.
4. **Ersetzen von Tags**. Diese Vorgänge ermöglichen dem Back-End, analog zum Ersetzen gewünschter Eigenschaften, alle vorhandenen Tags vollständig zu überschreiben und ein neues JSON-Dokument für `tags` bereitzustellen.

Alle oben genannten Vorgänge unterstützen [optimistische Parallelität][lnk-concurrency] und erfordern die Berechtigung **ServiceConnect**, wie im Artikel [Sicherheit][lnk-security] definiert.

Zusätzlich zu diesen Vorgängen kann das Back-End den Zwilling über eine SQL-ähnliche [Abfragesprache][lnk-query] abfragen und Vorgänge für große Mengen von Zwillingen durch [Aufträge][lnk-jobs] ausführen.

## <a name="device-operations"></a>Gerätevorgänge
Die Geräte-App greift mithilfe folgender atomarer Vorgänge auf den Zwilling zu:

1. **Abrufen des Zwillings**. Dieser Vorgang gibt den Inhalt des Dokuments für den Zwilling für das derzeit verbundene Gerät zurück, einschließlich Tags und gewünschter, gemeldeter und Systemeigenschaften.
2. **Teilweises Aktualisieren gemeldeter Eigenschaften**. Dieser Vorgang ermöglicht die teilweise Aktualisierung der gemeldeten Eigenschaften des derzeit verbundenen Geräts. Dabei wird das gleiche JSON-Updateformat wie bei der teilweisen Aktualisierung der gewünschten Eigenschaften durch das Back-End verwendet.
3. **Beobachten gewünschter Eigenschaften**. Das derzeit verbundene Gerät kann über Aktualisierungen an den gewünschten Eigenschaften benachrichtigt werden, sobald sie auftreten. Das Gerät erhält die gleiche Form der Aktualisierung (Teil- oder Vollaustausch), die durch das Back-End ausgeführt wird.

Alle oben genannten Vorgänge erfordern die Berechtigung **DeviceConnect**, wie im Artikel [Sicherheit][lnk-security] definiert.

Die [Azure IoT-Geräte-SDKs][lnk-sdks] vereinfachen die Verwendung der oben genannten Vorgänge mit vielen Sprachen und Plattformen. Weitere Informationen zu den Details der IoT Hub-Grundtypen für die Synchronisierung gewünschter Eigenschaften finden Sie im [Ablauf zur Wiederherstellung der Geräteverbindung][lnk-reconnection].

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen.
> 
> 

## <a name="reference"></a>Referenz
### <a name="tags-and-properties-format"></a>Format von Tags und Eigenschaften
Tags, gewünschte und gemeldete Eigenschaften sind JSON-Objekte mit den folgenden Einschränkungen:

* Alle Schlüssel in JSON-Objekten sind UNICODE-Zeichenfolgen mit 128 Zeichen, bei denen die Groß- und Kleinschreibung berücksichtigt werden muss. UNICODE-Steuerzeichen (Segmente C0 und C1) sowie `'.'`, `' '` und `'$'` gehören nicht zu den zulässigen Zeichen.
* Alle Werte im JSON-Objekt können die folgenden JSON-Typen aufweisen: boolescher Wert, Zahl, Zeichenfolge, Objekt. Arrays sind nicht zulässig.

### <a name="twin-size"></a>Größe von Zwillingen
IoT Hub erzwingt eine Größenbegrenzung von 8 KB für die Werte von `tags`, `properties/desired` und `properties/reported`, ausgenommen schreibgeschützte Elemente.
Die Größe wird durch Zusammenzählen aller Zeichen mit Ausnahme von UNICODE-Steuerzeichen (Segmente C0 und C1) und Leerzeichen `' '` außerhalb einer Zeichenfolgenkonstante berechnet.
IoT Hub gibt einen Fehler für alle Vorgänge zurück, die die Größe dieser Dokumente über den Grenzwert erhöhen würden.

### <a name="twin-metadata"></a>Metadaten von Zwillingen
IoT Hub verwaltet den Zeitstempel der letzten Aktualisierung für jedes JSON-Objekt in den gewünschten und gemeldeten Eigenschaften. Zeitstempel verwenden UTC und sind im [ISO8601]-Format codiert: `YYYY-MM-DDTHH:MM:SS.mmmZ`.
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

### <a name="optimistic-concurrency"></a>Optimistische Parallelität
Tags sowie gewünschte und gemeldete Eigenschaften unterstützen optimistische Parallelität.
Tags weisen ein ETag gemäß [RFC7232] auf, das der JSON-Darstellung des Tags entspricht. Dies kann in bedingten Aktualisierungsvorgängen vom Back-End verwendet werden, um Konsistenz sicherzustellen.

Gewünschte und gemeldete Eigenschaften weisen keine ETags auf, jedoch einen `$version`-Wert, der garantiert inkrementell ist. Analog zum ETag kann die Version von der aktualisierenden Seite (z.B. eine Geräte-App für eine gemeldete Eigenschaft oder das Back-End für eine gewünschte Eigenschaft) verwendet werden, um die Konsistenz von Aktualisierungen zu erzwingen.

Versionen sind auch nützlich, wenn ein beobachtender Agent (z.B. die Geräte-App, die die gewünschten Eigenschaften beobachtet) Racebedingungen zwischen dem Ergebnis eines Abrufvorgangs und einer Aktualisierungsbenachrichtigung beheben muss. Im Abschnitt [Ablauf zur Wiederherstellen der Geräteverbindung][lnk-reconnection] finden Sie weitere Informationen.

### <a name="device-reconnection-flow"></a>Ablauf zur Wiederherstellen der Geräteverbindung
IoT Hub behält keine Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften für nicht verbundene Geräte bei. Ein Gerät, das eine Verbindung herstellt, muss daher zusätzlich zum Abonnieren von Aktualisierungsbenachrichtigungen das gesamte Dokument der gewünschten Eigenschaften abrufen. Wenn die Gefahr von Racebedingungen zwischen Aktualisierungsbenachrichtigungen und dem vollständigen Abruf besteht, muss der folgende Ablauf sichergestellt werden:

1. Geräte-App stellt eine Verbindung mit einem IoT Hub her.
2. Geräte-App abonniert Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften.
3. Geräte-App ruft das vollständige Dokument für gewünschte Eigenschaften ab.

Die Geräte-App kann alle Benachrichtigungen ignorieren, bei denen `$version` kleiner oder gleich der Version des vollständigen abgerufenen Dokuments ist. Dies ist möglich, da IoT Hub sicherstellt, dass die Versionsnummern immer erhöht werden.

> [!NOTE]
> Diese Logik ist in den [Azure IoT-Geräte-SDKs][lnk-sdks] bereits implementiert. Diese Beschreibung ist nur dann nützlich, wenn die Geräte-App keine der Azure IoT-Geräte-SDKs verwenden kann und die MQTT-Schnittstelle direkt programmiert werden muss.
> 
> 

### <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Geräte- und Dienst-SDKs für IoT Hub][lnk-sdks] werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienstanwendungen für die Interaktion mit IoT Hub verwenden können.
* Unter [Abfragesprache für Zwillinge, Methoden und Aufträge][lnk-query] wird die Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen über Gerätezwillinge, Methoden und Aufträge abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun etwas über Gerätezwillinge erfahren haben, sind möglicherweise die folgenden Themen im Entwicklerhandbuch für Sie interessant:

* [Aufrufen einer direkten Methode auf einem Gerät][lnk-methods]
* [Planen von Aufträgen auf mehreren Geräten][lnk-jobs]

Wenn Sie einige der in diesem Artikel beschriebenen Konzepte ausprobieren möchten, sind möglicherweise die folgenden IoT Hub-Tutorials für Sie interessant:

* [Gewusst wie: Verwenden des Gerätezwillings][lnk-twin-tutorial]
* [Gewusst wie: Verwenden der Eigenschaften von Zwillingen][lnk-twin-properties]

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

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png


<!--HONumber=Oct16_HO2-->


