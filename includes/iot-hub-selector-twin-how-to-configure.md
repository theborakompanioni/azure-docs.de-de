> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> 
> 

## Einführung
<a id="introduction" class="xliff"></a>

Unter [Tutorial: Erste Schritte mit Gerätezwillingen (Vorschau)][lnk-twin-tutorial] wurde beschrieben, wie Sie Gerätemetadaten über Ihr Lösungs-Back-End mit *Tags* festlegen, Gerätebedingungen über eine Geräte-App mit *gemeldeten Eigenschaften* melden und diese Informationen mit einer SQL-ähnlichen Sprache abfragen.

In diesem Tutorial wird beschrieben, wie Sie die *gewünschten Eigenschaften* des Gerätezwillings zusammen mit *gemeldeten Eigenschaften* verwenden, um für Geräte-Apps die Remotekonfiguration durchzuführen. Genauer gesagt wird in diesem Tutorial veranschaulicht, wie die gemeldeten und gewünschten Eigenschaften eines Gerätezwillings eine mehrschrittige Konfiguration einer Geräteanwendung ermöglichen und für das Lösungs-Back-End die Ermittlung des Status dieses Vorgangs über alle Geräte hinweg möglich machen. Weitere Informationen zur Rolle der Gerätekonfiguration finden Sie unter [Übersicht über die Geräteverwaltung mit IoT Hub][lnk-dm-overview].

Ganz allgemein kann das Lösung-Back-End bei Verwendung von Gerätezwillingen die gewünschten Konfiguration für die verwalteten Geräte angeben, anstatt bestimmte Befehle zu senden. So kann das Gerät selbst die optimale Aktualisierungsmethode für seine Konfiguration einrichten (sehr wichtig in IoT-Szenarien, bei denen sich bestimmte Gerätebedingungen auf die Fähigkeit zum Ausführen bestimmter Befehle auswirken), während der aktuelle Status und potenzielle Fehlerbedingungen des Aktualisierungsprozesses kontinuierlich an das Lösungs-Back-End gemeldet werden. Dieses Muster ist wichtig für die Verwaltung von großen Gerätesätzen, da das Lösungs-Back-End so über alle Geräte hinweg über einen vollständigen Einblick in den Zustand des Konfigurationsprozesses verfügt.

> [!NOTE]
> In Szenarios, in denen Geräte auf interaktivere Weise gesteuert werden (Einschalten eines Ventilators über eine vom Benutzer gesteuerte App), können Sie auch [direkte Methoden][lnk-methods] verwenden.
> 
> 

In diesem Tutorial ändert das Lösungs-Back-End die Telemetriekonfiguration eines Zielgeräts. Infolgedessen durchläuft die Geräte-App einen Prozess mit mehreren Schritten, um ein Konfigurationsupdate anzuwenden (beispielsweise mit einem obligatorischen Neustart des Softwaremoduls, was in diesem Tutorial wird durch eine einfache Verzögerung simuliert wird).

Das Lösungs-Back-End speichert die Konfiguration wie folgt in den gewünschten Eigenschaften des Gerätezwillings:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

> [!NOTE]
> Da Konfigurationen komplexe Objekte sein können, werden ihnen normalerweise eindeutige IDs zugewiesen (Hashes oder [GUIDs][lnk-guid]), um Vergleiche zu vereinfachen.
> 
> 

Die Geräte-App meldet ihre aktuelle Konfiguration und spiegelt die gewünschte Eigenschaft **telemetryConfig** in den gemeldeten Eigenschaften:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Beachten Sie, dass die gemeldete Eigenschaft **telemetryConfig** über eine zusätzliche **status**-Eigenschaft verfügt. Sie wird verwendet, um den Zustand des Aktualisierungsprozesses für die Konfiguration zu melden.

Wenn eine neue gewünschte Konfiguration empfangen wird, meldet die Geräte-App eine ausstehende Konfiguration, indem die Informationen geändert werden:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Später meldet die Geräte-App dann mittels Aktualisierung der obigen Eigenschaft den Erfolg oder Misserfolg dieses Vorgangs.
Beachten Sie, dass das Lösungs-Back-End jederzeit den Status des Konfigurationsprozesses über alle Geräte hinweg abfragen kann.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Erstellen Sie eine simulierte Geräte-App, die Konfigurationsupdates vom Lösungs-Back-End erhält und mehrere Updates als *gemeldete Eigenschaften* für den Aktualisierungsprozess der Konfiguration meldet.
* Erstellen Sie eine Back-End-App, mit der die gewünschte Konfiguration eines Geräts aktualisiert und anschließend der Aktualisierungsprozess der Konfiguration abgefragt wird.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
