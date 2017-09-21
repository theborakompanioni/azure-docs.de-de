---
title: "Verwenden von Azure IoT Hub-Gerätezwillingseigenschaften (Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Geräte konfigurieren. Sie verwenden die Azure IoT SDKs für Node.js, um eine simulierte Geräte-App und eine Dienst-App zu implementieren, die eine Gerätekonfiguration mithilfe eines Gerätezwillings ändert."
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: elioda
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: 6ff6f1c331d5a77e7ac0a47af6806f5d90fb0fdc
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="use-desired-properties-to-configure-devices-node"></a>Verwenden von gewünschten Eigenschaften zum Konfigurieren von Geräten (Node)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolen-Apps:

* **SimulateDeviceConfiguration.js**, eine simulierte Geräte-App, die auf eine gewünschte Konfigurationsaktualisierung wartet und den Status eines simulierten Konfigurationsaktualisierungsvorgangs meldet.
* **SetDesiredConfigurationAndQuery.js**, eine Node.js-Back-End-App, die die gewünschte Konfiguration für ein Gerät festlegt und den Konfigurationsaktualisierungsprozess abfragt.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js Version 4.0.x oder höher
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

Wenn Sie das Tutorial [Erste Schritte mit Gerätezwillingen][lnk-twin-tutorial] ausgeführt haben, haben Sie bereits einen IoT Hub und die Geräteidentität **myDeviceId** und können daher mit dem Abschnitt [Erstellen der simulierten Geräte-App][lnk-how-to-configure-createapp] fortfahren.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-the-simulated-device-app"></a>Erstellen der simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die als **myDeviceId** eine Verbindung mit dem Hub herstellt, auf eine gewünschte Konfigurationsaktualisierung wartet und dann Aktualisierungen für den simulierten Konfigurationsaktualisierungsvorgang meldet.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulatedeviceconfiguration**. Erstellen Sie im Ordner **simulatedeviceconfiguration** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **simulatedeviceconfiguration** den folgenden Befehl aus, um das Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **SimulateDeviceConfiguration.js** im Ordner **simulatedeviceconfiguration**.
4. Fügen Sie in der Datei **SimulateDeviceConfiguration.js** den folgenden Code ein, und ersetzen Sie den Platzhalter **{device connection string}** durch die Geräteverbindungszeichenfolge, die Sie beim Erstellen der Geräteidentität **myDeviceId** kopiert haben:
   
        'use strict';
        var Client = require('azure-iot-device').Client;
        var Protocol = require('azure-iot-device-mqtt').Mqtt;
   
        var connectionString = '{device connection string}';
        var client = Client.fromConnectionString(connectionString, Protocol);
   
        client.open(function(err) {
            if (err) {
                console.error('could not open IotHub client');
            } else {
                client.getTwin(function(err, twin) {
                    if (err) {
                        console.error('could not get twin');
                    } else {
                        console.log('retrieved device twin');
                        twin.properties.reported.telemetryConfig = {
                            configId: "0",
                            sendFrequency: "24h"
                        }
                        twin.on('properties.desired', function(desiredChange) {
                            console.log("received change: "+JSON.stringify(desiredChange));
                            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
                            if (desiredChange.telemetryConfig &&desiredChange.telemetryConfig.configId !== currentTelemetryConfig.configId) {
                                initConfigChange(twin);
                            }
                        });
                    }
                });
            }
        });
   
    Das **Client**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Geräts erforderlich sind. Mit diesem Code wird nach dem Initialisieren des **Client**-Objekts der Gerätezwilling für **myDeviceId** abgerufen und ein Handler für die Aktualisierung der gewünschten Eigenschaften angefügt. Der Handler überprüft durch einen Vergleich der configIds, ob eine tatsächliche Anforderung für eine Konfigurationsänderung vorliegt, und ruft dann eine Methode zum Starten der Konfigurationsänderung auf.
   
    Beachten Sie, dass beim vorherigen Code der Einfachheit halber ein hartcodierter Standardwert für die anfängliche Konfiguration verwendet wird. Mit einer realen App wird diese Konfiguration wahrscheinlich aus einem lokalen Speicher geladen.
   
   > [!IMPORTANT]
   > Änderungsereignisse der gewünschten Eigenschaften werden bei der Geräteverbindung immer einmal ausgegeben. Stellen Sie vor dem Ausführen einer Aktion sicher, dass bei den gewünschten Eigenschaften eine tatsächliche Änderung vorliegt.
   > 
   > 
5. Fügen Sie vor dem `client.open()`-Aufruf die folgenden Methoden hinzu:
   
        var initConfigChange = function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.pendingConfig = twin.properties.desired.telemetryConfig;
            currentTelemetryConfig.status = "Pending";
   
            var patch = {
            telemetryConfig: currentTelemetryConfig
            };
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.log('Could not report properties');
                } else {
                    console.log('Reported pending config change: ' + JSON.stringify(patch));
                    setTimeout(function() {completeConfigChange(twin);}, 60000);
                }
            });
        }
   
        var completeConfigChange =  function(twin) {
            var currentTelemetryConfig = twin.properties.reported.telemetryConfig;
            currentTelemetryConfig.configId = currentTelemetryConfig.pendingConfig.configId;
            currentTelemetryConfig.sendFrequency = currentTelemetryConfig.pendingConfig.sendFrequency;
            currentTelemetryConfig.status = "Success";
            delete currentTelemetryConfig.pendingConfig;
   
            var patch = {
                telemetryConfig: currentTelemetryConfig
            };
            patch.telemetryConfig.pendingConfig = null;
   
            twin.properties.reported.update(patch, function(err) {
                if (err) {
                    console.error('Error reporting properties: ' + err);
                } else {
                    console.log('Reported completed config change: ' + JSON.stringify(patch));
                }
            });
        };
   
    Die **initConfigChange**-Methode aktualisiert gemeldete Eigenschaften des lokalen Gerätezwillingsobjekts mit der Anforderung zur Konfigurationsaktualisierung, legt den Status auf **Ausstehend** fest und aktualisiert dann den Gerätezwilling für den Dienst. Nach dem erfolgreichen Aktualisieren des Gerätezwillings wird ein lang ausgeführter Vorgang simuliert, der mit der Ausführung von **completeConfigChange** endet. Diese Methode aktualisiert die gemeldeten Eigenschaften des lokalen Gerätezwillings. Dabei wird der Status auf **Success** festgelegt und das **pendingConfig**-Objekt entfernt. Anschließend wird der Gerätezwilling für den Dienst aktualisiert.
   
    Beachten Sie, dass zum Einsparen von Bandbreite beim Aktualisieren der gemeldeten Eigenschaften nicht das gesamte Dokument ersetzt wird, sondern nur die zu ändernden Eigenschaften (mit dem Namen **patch** im Code oben) angegeben werden.
   
   > [!NOTE]
   > In diesem Tutorial wird nicht das Verhalten für gleichzeitige Konfigurationsaktualisierungen simuliert. Bei einigen Konfigurationsaktualisierungsvorgängen können Änderungen der Zielkonfiguration möglicherweise während der Ausführung der Aktualisierung erfolgen, und bei einigen müssen diese eventuell in die Warteschlange gestellt werden. Bei anderen wiederum können sie ohne eine Fehlerbedingung abgelehnt werden. Stellen Sie sicher, dass Sie das gewünschte Verhalten für den spezifischen Konfigurationsvorgang berücksichtigen, und fügen Sie vor dem Initiieren der Konfigurationsänderung die entsprechende Logik hinzu.
   > 
   > 
6. Führen Sie die Geräte-App aus:
   
        node SimulateDeviceConfiguration.js
   
    Die Meldung `retrieved device twin` sollte angezeigt werden. Führen Sie die App weiter aus.

## <a name="create-the-service-app"></a>Erstellen der Dienst-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die die *gewünschten Eigenschaften* für den Gerätezwilling, der **myDeviceId** zugeordnet ist, mit einem neuen Telemetriekonfigurationsobjekt aktualisiert. Anschließend werden die im IoT-Hub gespeicherten Gerätezwillinge abgefragt und die Unterschiede zwischen den gewünschten und den gemeldeten Konfigurationen des Geräts angezeigt.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **setdesiredandqueryapp**. Erstellen Sie im Ordner **setdesiredandqueryapp** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl ausführen. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **setdesiredandqueryapp** den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:
   
    ```
    npm install azure-iothub node-uuid --save
    ```
3. Erstellen Sie mithilfe eines Text-Editors die neue Datei **SetDesiredAndQuery.js** im Ordner **setdesiredandqueryapp**.
4. Fügen Sie in der Datei **SetDesiredAndQuery.js** den folgenden Code ein, und ersetzen Sie den Platzhalter **{iot hub connection string}** durch die IoT Hub-Verbindungszeichenfolge, die Sie beim Erstellen des Hubs kopiert haben:
   
        'use strict';
        var iothub = require('azure-iothub');
        var uuid = require('node-uuid');
        var connectionString = '{iot hub connection string}';
        var registry = iothub.Registry.fromConnectionString(connectionString);
   
        registry.getTwin('myDeviceId', function(err, twin){
            if (err) {
                console.error(err.constructor.name + ': ' + err.message);
            } else {
                var newConfigId = uuid.v4();
                var newFrequency = process.argv[2] || "5m";
                var patch = {
                    properties: {
                        desired: {
                            telemetryConfig: {
                                configId: newConfigId,
                                sendFrequency: newFrequency
                            }
                        }
                    }
                }
                twin.update(patch, function(err) {
                    if (err) {
                        console.error('Could not update twin: ' + err.constructor.name + ': ' + err.message);
                    } else {
                        console.log(twin.deviceId + ' twin updated successfully');
                    }
                });
                setInterval(queryTwins, 10000);
            }
        });

    Das **Registry**-Objekt macht alle Methoden verfügbar, die für die Interaktion mit Gerätezwillingen des Diensts erforderlich sind. Mit diesem Code wird nach dem Initialisieren des **Registry**-Objekts der Gerätezwilling für **myDeviceId** abgerufen, und die zugehörigen gewünschten Eigenschaften werden mit einem neuen Telemetriekonfigurationsobjekt aktualisiert. Anschließend wird die Funktion **queryTwins** alle 10 Sekunden aufgerufen.

    > [!IMPORTANT]
    > Diese Anwendung fragt IoT Hub zur Veranschaulichung alle 10 Sekunden ab. Verwenden Sie Abfragen, um Berichte für Benutzer für viele Geräte zu generieren, und nicht, um Änderungen zu erkennen. Wenn für Ihre Lösung Echtzeitbenachrichtigungen von Geräteereignissen erforderlich sind, verwenden Sie [twin notifications][lnk-twin-notifications].
    > 
    >.

1. Fügen Sie direkt vor dem `registry.getDeviceTwin()`-Aufruf den folgenden Code zum Implementieren der Funktion **queryTwins** ein:
   
        var queryTwins = function() {
            var query = registry.createQuery("SELECT * FROM devices WHERE deviceId = 'myDeviceId'", 100);
            query.nextAsTwin(function(err, results) {
                if (err) {
                    console.error('Failed to fetch the results: ' + err.message);
                } else {
                    console.log();
                    results.forEach(function(twin) {
                        var desiredConfig = twin.properties.desired.telemetryConfig;
                        var reportedConfig = twin.properties.reported.telemetryConfig;
                        console.log("Config report for: " + twin.deviceId);
                        console.log("Desired: ");
                        console.log(JSON.stringify(desiredConfig, null, 2));
                        console.log("Reported: ");
                        console.log(JSON.stringify(reportedConfig, null, 2));
                    });
                }
            });
        };
   
    Mit dem vorherigen Code werden die in IoT Hub gespeicherten Gerätezwillinge abgefragt und die gewünschten und gemeldeten Telemetriekonfigurationen gedruckt. Informationen zum Generieren von umfassenden Berichten für alle Ihre Geräte finden Sie im Artikel zur [IoT Hub-Abfragesprache][lnk-query].
2. Während **SimulateDeviceConfiguration.js** ausgeführt wird, führen Sie die Anwendung folgendermaßen aus:
   
        node SetDesiredAndQuery.js 5m
   
    Mit der neuen aktiven Sendehäufigkeit von fünf Minuten anstatt 24 Stunden sollte sich die gemeldete Konfigurationsänderung von **Success** in **Pending** und wieder in **Success** ändern.
   
   > [!IMPORTANT]
   > Es gibt eine Verzögerung von bis zu einer Minute zwischen dem Vorgang für den Gerätebericht und dem Ergebnis der Abfrage. Dies ermöglicht die Abfrageinfrastruktur mit umfangreicher Skalierung. Zum Abrufen einheitlicher Ansichten eines einzelnen Gerätezwillings verwenden Sie die **getDeviceTwin**-Methode in der **Registry**-Klasse.
   > 
   > 

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine gewünschte Konfiguration als *gewünschte Eigenschaften* in einer Back-End-App festgelegt und eine simulierte Geräte-App geschrieben, die diese Änderung erkennt und einen Aktualisierungsvorgang in mehreren Schritten simuliert, bei dem der entsprechende Status als *gemeldete Eigenschaften* an den Gerätezwilling gemeldet wird.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub][lnk-iothub-getstarted]
* Planen oder Durchführen von Vorgängen für eine Vielzahl von Geräten im Tutorial [Schedule and broadcast jobs][lnk-schedule-jobs] (Planen und Übertragen von Aufträgen)
* Interaktive Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Use direct methods][lnk-methods-tutorial] (Verwenden von direkten Methoden)

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-node-node-getstarted.md
[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app

