---
title: Erste Schritte mit Gerätezwillingen | Microsoft Docs
description: In diesem Tutorial wird die Verwendung von Gerätezwillingen erläutert.
services: iot-hub
documentationcenter: node
author: fsautomata
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/13/2016
ms.author: elioda

---
# <a name="tutorial:-get-started-with-device-twins-(preview)"></a>Tutorial: Erste Schritte mit Gerätezwillingen (Vorschau)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolenanwendungen:

* **AddTagsAndQuery.js**, eine Node.js-App, die über das Back-End ausgeführt werden soll und mit der Tags hinzugefügt und Gerätezwillinge abgefragt werden.
* **TwinSimulatedDevice.js**, eine Node.js-App, die ein Gerät simuliert, das eine Verbindung mit dem IoT Hub mit der zuvor erstellten Geräteidentität herstellt und seine Verbindungsbedingung meldet.

> [!NOTE]
> Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Anwendungen erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Azure – Kostenlose Testversion][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, mit der dem Gerätezwilling, der **myDeviceId** zugeordnet ist, Standortmetadaten hinzugefügt werden. Anschließend werden die in dem Hub, der die Geräte in den USA auswählt, gespeicherten Gerätezwillinge abgefragt und dann diejenigen, die eine Mobilfunkverbindung melden.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **addtagsandqueryapp**. Erstellen Sie im Ordner **addtagsandqueryapp** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **addtagsandqueryapp** den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:
   
    ```
    npm install azure-iothub@dtpreview --save
    ```
3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **AddTagsAndQuery.js** im Ordner **addtagsandqueryapp**.
4. Fügen Sie in der Datei **AddTagsAndQuery.js** den folgenden Code ein, und ersetzen Sie den Platzhalter **{service connection string}** durch die Verbindungszeichenfolge, die Sie beim Erstellen des Hubs kopiert haben:
   
        'use strict';
        var iothub = require('azure-iothub');
        var connectionString = '{service hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var patch = {
                    tags: {
                        location: {
                            region: 'US',
                            plant: 'Redmond43'
                      }
                    }
                };
   
                twin.update(patch, function(err) {
                  if (err) {
                    console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                  } else {
                    console.log(twin.deviceId + ' twin updated successfully');
                    queryTwins();
                  }
                });
            }
        });
   
    Das **Registry**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Mit dem vorherigen Code werden zunächst das **Registry**-Objekt initialisiert, dann der Gerätezwilling für **myDeviceId** abgerufen und schließlich die zugehörigen Tags mit den gewünschten Standortinformationen aktualisiert.
   
    Nach dem Aktualisieren der Tags wird die Funktion **queryTwins** aufgerufen.
5. Fügen Sie am Ende der Datei **AddTagsAndQuery.js** den folgenden Code ein, um die Funktion **queryTwins** zu implementieren:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
   
            query = registry.createQuery("SELECT * FROM devices WHERE tags.location.plant = 'Redmond43' AND properties.reported.connectivity.type = 'cellular'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log("Devices in Redmond43 using cellular network: " + results.map(function(twin) {return twin.deviceId}).join(','));
                }
            });
        };
   
    Mithilfe dieses Codes werden zwei Abfragen ausgeführt: Mit der ersten werden nur die Gerätezwillinge von Geräten in der Anlage **Redmond43** ausgewählt. Mit der zweiten wird die Abfrage so angepasst, dass nur die Geräte ausgewählt werden, die über ein Mobilfunknetz verbunden sind.
   
    Beachten Sie, dass mit diesem Code beim Erstellen des **query**-Objekts eine maximale Anzahl von zurückgegebenen Dokumenten angegeben wird. Das **query**-Objekt enthält die boolesche Eigenschaft **hasMoreResults**, mit der Sie die **nextAsTwin**-Methoden mehrmals aufrufen können, um alle Ergebnisse abzurufen. Die **next**-Methode ist verfügbar für Ergebnisse, die keine Gerätezwillinge sind, z.B. Ergebnisse von Aggregationsabfragen.
6. Führen Sie die Anwendung mit folgendem Befehl aus:
   
        node AddTagsAndQuery.js
   
    In den Ergebnissen für die Abfrage, mit der alle Geräte in der Anlage **Redmond43** abgefragt werden, sollte ein Gerät angezeigt werden und keines für die Abfrage, mit der die Ergebnisse auf die über ein Mobilfunknetz verbundenen Geräte beschränkt werden.
   
    ![][1]

Im nächsten Abschnitt erstellen Sie eine Geräte-App, mit der die Verbindungsinformationen gemeldet und das Ergebnis der im vorherigen Abschnitt ausgeführten Abfrage geändert wird.

## <a name="create-the-device-app"></a>Erstellen der Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die als **myDeviceId** eine Verbindung mit dem Hub herstellt und dann die gemeldeten Eigenschaften des zugehörigen Gerätezwillings so aktualisiert, dass sie die Informationen über die Verbindung mit einem Mobilfunknetz enthalten.

> [!NOTE]
> Zurzeit ist der Zugriff auf Gerätezwillinge nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen. Anweisungen zum Konvertieren vorhandener Geräte-Apps für die Verwendung von MQTT finden Sie im Artikel [MQTT-Unterstützung][lnk-devguide-mqtt].
> 
> 

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **reportconnectivity**. Erstellen Sie im Ordner **reportconnectivity** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **reportconnectivity** den folgenden Befehl aus, um das Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```
3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **ReportConnectivity.js** im Ordner **reportconnectivity**.
4. Fügen Sie in der Datei **ReportConnectivity.js** den folgenden Code ein, und ersetzen Sie den Platzhalter **{device connection string}** durch die Verbindungszeichenfolge, die Sie beim Erstellen der Geräteidentität **myDeviceId** kopiert haben:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
   
            client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                var patch = {
                    connectivity: {
                        type: 'cellular'
                    }
                };
   
                twin.properties.reported.update(patch, function(err) {
                    if (err) {
                        console.error('could not update twin');
                    } else {
                        console.log('twin state reported');
                        process.exit();
                    }
                });
            }
            });
        }
        });
   
    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code wird nach dem Initialisieren des **Client**-Objekts der Gerätezwilling für **myDeviceId** abgerufen, und die zugehörige gemeldete Eigenschaft mit den Verbindungsinformationen wird aktualisiert.
5. Führen Sie die Geräte-App aus.
   
        node ReportConnectivity.js
   
    Die Meldung `twin state reported` sollte angezeigt werden.
6. Da das Gerät nun die Verbindungsinformationen gemeldet hat, sollten diese in beiden Abfragen angezeigt werden. Wechseln Sie zurück in den Ordner **addtagsandqueryapp**, und führen Sie die Abfragen erneut aus:
   
        node AddTagsAndQuery.js
   
    Nun sollte **myDeviceId** in beiden Abfrageergebnisse angezeigt werden.
   
    ![][3]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben Gerätemetadaten aus einer Back-End-Anwendung als Tags hinzugefügt und eine simulierte Geräte-App zum Melden der Verbindungsinformationen des Geräts im Gerätezwilling geschrieben. Sie haben zudem erfahren, wie diese Informationen mithilfe der SQL-ähnlichen IoT Hub-Abfragesprache abgefragt werden können.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Informationen zum Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted]
* Informationen zum Konfigurieren von Geräten mit den gewünschten Eigenschaften des Gerätezwillings im Tutorial [Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten][lnk-twin-how-to-configure]
* Informationen zur interaktiven Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Verwenden von direkten Methoden][lnk-methods-tutorial]

<!-- images -->
[1]: media/iot-hub-node-node-twin-getstarted/service1.png
[3]: media/iot-hub-node-node-twin-getstarted/service2.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-c2d-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md


<!--HONumber=Oct16_HO2-->


