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
ms.date: 09/30/2016
ms.author: elioda
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: 3c9b3a9509493e8c6900d90b5ab6519de7a0721f


---
# <a name="device-twins"></a>Gerätezwillinge
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

* Speichern gerätespezifischer Metadaten, z.B. des Aufstellungsort eines Verkaufsautomaten, in der Cloud
* Melden aktueller Statusinformationen, wie verfügbarer Funktionen und Zustände aus der Geräte-App, z.B. bei einem Gerät, das über das Mobilfunknetz oder WiFi eine Verbindung herstellt
* Synchronisieren des Status lang andauernder Workflows zwischen der Geräte-App und der Back-End-App – etwa, wenn das Lösungs-Back-End die neu zu installierende Firmwareversion angibt und die Geräte-App die verschiedenen Phasen des Aktualisierungsvorgangs meldet
* Abfragen von Metadaten, Konfiguration oder Status des Geräts

Wenn Sie weitere Informationen zur Verwendung von gemeldeten Eigenschaften, D2C-Nachrichten oder Dateiuploads benötigen, hilft Ihnen das Thema [Leitfaden zur D2C-Kommunikation][lnk-d2c-guidance] weiter.
Falls Sie weitere Informationen zur Verwendung von gewünschten Eigenschaften, direkten Methoden oder C2D-Nachrichten benötigen, hilft Ihnen das Thema [Leitfaden zur C2D-Kommunikation][lnk-c2d-guidance] weiter.

## <a name="device-twins"></a>Gerätezwillinge
Auf Gerätezwillingen werden gerätespezifische Informationen gespeichert, auf die Folgendes zutrifft:

* Geräte und Back-Ends können mit ihnen Gerätezustände und -konfigurationen synchronisieren.
* Das Lösungs-Back-End kann mit ihnen lang andauernde Vorgänge abfragen und als Ziel verwenden.

Der Lebenszyklus eines Gerätezwillings ist mit der entsprechenden [Geräteidentität][lnk-identity] verknüpft. Gerätezwillinge werden implizit erstellt und gelöscht, wenn in IoT Hub eine Geräteidentität neu erstellt oder gelöscht wird.

Ein Gerätezwilling ist ein JSON-Dokument, das Folgendes enthält:

* **Tags**. Ein vom Lösungs-Back-End gelesenes und geschriebenes JSON-Dokument. Tags sind für Geräte-Apps nicht sichtbar.
* **Gewünschte Eigenschaften** werden in Verbindung mit gemeldeten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gewünschte Eigenschaften können nur vom Lösungs-Back-End festgelegt und von der Geräte-App gelesen werden. Die Geräte-App kann auch in Echtzeit über Änderungen an den gewünschten Eigenschaften benachrichtigt werden.
* **Gemeldete Eigenschaften** werden in Verbindung mit gewünschten Eigenschaften zum Synchronisieren von Gerätekonfigurationen oder -zuständen verwendet. Gemeldete Eigenschaften können nur von der Geräte-App festgelegt und vom Lösungs-Back-End gelesen und abgefragt werden.

Darüber hinaus enthält der Stamm des Gerätezwillings die schreibgeschützten Eigenschaften der zugehörigen Identität gemäß der [Registrierung der Identität][lnk-identity].

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

Im Stammobjekt befinden sich die Systemeigenschaften und Containerobjekte für `tags` sowie die `reported`- und `desired`-Eigenschaften. Der Container `properties` enthält einige schreibgeschützte Elemente (`$metadata`, `$etag` und `$version`), die in den Abschnitten zu den [Metadaten des Gerätezwillings][lnk-twin-metadata] und zur [optimistischen Parallelität][lnk-concurrency] beschrieben sind.

### <a name="reported-property-example"></a>Beispiel für eine gemeldete Eigenschaft
Im Beispiel oben enthält der Gerätezwilling eine `batteryLevel`-Eigenschaft, die von der Geräte-App gemeldet wird. Mit dieser Eigenschaft können Geräte anhand des letzten gemeldeten Akkustands abgefragt und Aktionen auf ihnen ausgeführt werden. Ein weiteres Beispiel wäre das Melden von Gerätefunktionen oder Verbindungsoptionen durch das Gerät.

Gemeldete Eigenschaften tragen zur Vereinfachung von Szenarien bei, in denen das Lösungs-Back-End den letzten bekannten Wert einer Eigenschaft abfragen soll. Verwenden Sie [Gerät-zu-Cloud-Nachrichten][lnk-d2c], wenn das Lösungs-Back-End Gerätetelemetriedaten in Form von Folgen von Ereignissen mit Zeitstempeln abfragen muss (beispielsweise als Zeitreihe).

### <a name="desired-property-example"></a>Beispiel für eine gewünschte Eigenschaft
Im obigen Beispiel werden die gewünschten und gemeldeten Eigenschaften des `telemetryConfig`-Gerätezwillings vom Lösungs-Back-End und von der Geräte-App verwendet, um die Telemetriekonfiguration für dieses Gerät zu synchronisieren. Beispiel:

1. Das Lösungs-Back-End legt die gewünschte Eigenschaft mit dem gewünschten Konfigurationswert fest. Dies ist der Teil des Dokuments mit der gewünschten Eigenschaft:
   
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
> Die obigen Codeausschnitte sind für die Lesbarkeit optimierte Beispiele. Sie stellen eine Möglichkeit zum Codieren einer Konfiguration und des Status eines Geräts dar. IoT Hub setzt kein bestimmtes Schema für die gewünschten und gemeldeten Eigenschaften in den Gerätezwillingen voraus.
> 
> 

In vielen Fällen werden Zwillinge zum Synchronisieren lang andauernder Vorgänge verwendet, z.B. Firmwareupdates. Weitere Informationen zur Verwendung von Eigenschaften zum Synchronisieren und Verfolgen lang andauernder Vorgänge über mehrere Geräte finden Sie unter [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-properties].

## <a name="back-end-operations"></a>Back-End-Vorgänge
Das Lösungs-Back-End greift mithilfe folgender atomarer Vorgänge, die über HTTP verfügbar gemacht werden, auf den Gerätezwilling zu:

1. **Abrufen des Gerätezwillings mittels ID**. Dieser Vorgang gibt den Inhalt des Dokuments für den Gerätezwilling zurück, einschließlich Tags und gewünschter, gemeldeter und Systemeigenschaften.
2. **Partielles Aktualisieren des Gerätezwillings**. Dieser Vorgang ermöglicht dem Lösungs-Back-End, die Tags oder gewünschten Eigenschaften des Gerätezwillings teilweise zu aktualisieren. Die teilweise Aktualisierung hat die Form eines JSON-Dokuments, das angegebene Eigenschaft hinzufügt oder aktualisiert. Auf `null` festgelegte Eigenschaften werden entfernt. Der folgende Code erstellt z.B. eine neue gewünschte Eigenschaft mit dem Wert `{"newProperty": "newValue"}`, überschreibt den vorhandenen Wert von `existingProperty` durch `"otherNewValue"` und entfernt `otherOldProperty` vollständig. An anderen vorhandenen Eigenschaften oder Tags werden keine Änderungen vorgenommen:
   
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
4. **Ersetzen von Tags**. Diese Vorgänge ermöglichen dem Lösungs-Back-End (analog zum Ersetzen gewünschter Eigenschaften) alle vorhandenen Tags vollständig zu überschreiben und ein neues JSON-Dokument für `tags` bereitzustellen.

Alle oben genannten Vorgänge unterstützen [optimistische Parallelität][lnk-concurrency] und erfordern die Berechtigung **ServiceConnect**, wie im Artikel [Sicherheit][lnk-security] definiert.

Zusätzlich zu diesen Vorgängen kann das Lösungs-Back-End die Gerätezwillinge über eine SQL-ähnliche [IoT Hub-Abfragesprache][lnk-query] abfragen und Vorgänge für große Mengen von Gerätezwillingen durch [Aufträge][lnk-jobs] ausführen.

## <a name="device-operations"></a>Gerätevorgänge
Die Geräte-App greift mithilfe folgender atomarer Vorgänge auf den Gerätezwilling zu:

1. **Abrufen des Gerätezwillings**. Dieser Vorgang gibt den Inhalt des Dokuments für den Gerätezwilling für das derzeit verbundene Gerät zurück, einschließlich Tags und gewünschter, gemeldeter und Systemeigenschaften.
2. **Teilweises Aktualisieren gemeldeter Eigenschaften**. Dieser Vorgang ermöglicht die teilweise Aktualisierung der gemeldeten Eigenschaften des derzeit verbundenen Geräts. Dabei wird das gleiche JSON-Updateformat wie bei der partiellen Aktualisierung der gewünschten Eigenschaften durch das Lösungs-Back-End verwendet.
3. **Beobachten gewünschter Eigenschaften**. Das derzeit verbundene Gerät kann über Aktualisierungen an den gewünschten Eigenschaften benachrichtigt werden, sobald sie auftreten. Das Gerät erhält die gleiche Form der Aktualisierung (partielle oder vollständige Ersetzung), die durch das Lösungs-Back-End ausgeführt wird.

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
IoT Hub gibt einen Fehler für alle Vorgänge zurück, die die Größe dieser Dokumente über den Grenzwert erhöhen würden.

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
Tags weisen ein ETag gemäß [RFC7232] auf, das der JSON-Darstellung des Tags entspricht. Dieses kann bei bedingten Aktualisierungsvorgängen über das Lösungs-Back-End verwendet werden, um die Konsistenz zu gewährleisten.

Gewünschte und gemeldete Eigenschaften des Gerätezwillings weisen keine ETags auf, jedoch einen `$version`-Wert, der garantiert inkrementell ist. Analog zum ETag kann die Version durch die aktualisierende Seite (etwa eine Geräte-App für eine gemeldete Eigenschaft oder das Lösungs-Back-End für eine gewünschte Eigenschaft) verwendet werden, um die Konsistenz von Aktualisierungen zu erzwingen.

Versionen sind auch nützlich, wenn ein beobachtender Agent (z.B. die Geräte-App, die die gewünschten Eigenschaften beobachtet) Racebedingungen zwischen dem Ergebnis eines Abrufvorgangs und einer Aktualisierungsbenachrichtigung beheben muss. Im Abschnitt [Ablauf zur Wiederherstellen der Geräteverbindung][lnk-reconnection] finden Sie weitere Informationen.

## <a name="device-reconnection-flow"></a>Ablauf zur Wiederherstellen der Geräteverbindung
IoT Hub behält keine Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften für nicht verbundene Geräte bei. Ein Gerät, das eine Verbindung herstellt, muss daher zusätzlich zum Abonnieren von Aktualisierungsbenachrichtigungen das gesamte Dokument der gewünschten Eigenschaften abrufen. Wenn die Gefahr von Racebedingungen zwischen Aktualisierungsbenachrichtigungen und dem vollständigen Abruf besteht, muss der folgende Ablauf sichergestellt werden:

1. Geräte-App stellt eine Verbindung mit einem IoT Hub her.
2. Geräte-App abonniert Aktualisierungsbenachrichtigungen für gewünschte Eigenschaften.
3. Geräte-App ruft das vollständige Dokument für gewünschte Eigenschaften ab.

Die Geräte-App kann alle Benachrichtigungen ignorieren, bei denen `$version` kleiner oder gleich der Version des vollständigen abgerufenen Dokuments ist. Dies ist möglich, da IoT Hub sicherstellt, dass die Versionsnummern immer erhöht werden.

> [!NOTE]
> Diese Logik ist in den [Azure IoT-Geräte-SDKs][lnk-sdks] bereits implementiert. Diese Beschreibung ist nur dann nützlich, wenn die Geräte-App keine der Azure IoT-Geräte-SDKs verwenden kann und die MQTT-Schnittstelle direkt programmiert werden muss.
> 
> 

## <a name="additional-reference-material"></a>Weiteres Referenzmaterial
Weitere Referenzthemen im IoT Hub-Entwicklerhandbuch:

* Unter [IoT Hub-Endpunkte][lnk-endpoints] werden die verschiedenen Endpunkte beschrieben, die jeder IoT-Hub für Laufzeit- und Verwaltungsvorgänge bereitstellt.
* Unter [Drosselung und Kontingente][lnk-quotas] werden die Kontingente für den IoT Hub-Dienst und das Drosselungsverhalten beschrieben, die bei Verwendung des Diensts zu erwarten sind.
* Unter [Azure IoT device and service SDKs][lnk-sdks] (Azure IoT SDKs für Geräte und Dienste) werden die verschiedenen Sprach-SDKs aufgelistet, die Sie bei der Entwicklung von Geräte- und Dienst-Apps für die Interaktion mit IoT Hub verwenden können.
* Unter [Referenz – Abfragesprache für Zwillinge und Aufträge][lnk-query] wird die IoT Hub-Abfragesprache beschrieben, mit der Sie von IoT Hub Informationen zu Gerätezwillingen und Aufträgen abrufen können.
* [IoT Hub-MQTT-Unterstützung][lnk-devguide-mqtt] enthält weitere Informationen zur Unterstützung für das MQTT-Protokoll in IoT Hub.

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



<!--HONumber=Dec16_HO1-->


