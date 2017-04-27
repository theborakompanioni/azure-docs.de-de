---
title: "Herstellen einer Geräteverbindung mit Node.js | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie ein Gerät mit der vorkonfigurierten Remoteüberwachungslösung von Azure IoT Suite verbinden, indem Sie eine in Node.js geschriebene Anwendung ausführen."
services: 
suite: iot-suite
documentationcenter: na
author: dominicbetts
manager: timlt
editor: 
ms.assetid: fc50a33f-9fb9-42d7-b1b8-eb5cff19335e
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: 757d6f778774e4439f2c290ef78cbffd2c5cf35e
ms.openlocfilehash: 8f45d0e86a95779d5ceeddb72638b14e0e7a80eb
ms.lasthandoff: 04/10/2017


---
# <a name="connect-your-device-to-the-remote-monitoring-preconfigured-solution-nodejs"></a>Verbinden Ihres Geräts mit der vorkonfigurierten Remoteüberwachungslösung (Node.js)
[!INCLUDE [iot-suite-selector-connecting](../../includes/iot-suite-selector-connecting.md)]

## <a name="create-a-nodejs-sample-solution"></a>Erstellen einer Node.js-Beispiellösung

Stellen Sie sicher, dass auf dem Entwicklungscomputer mindestens die Node.js-Version 0.11.5 oder höher installiert ist. Sie können `node --version` in der Befehlszeile ausführen, um die Version zu überprüfen.

1. Erstellen Sie auf Ihrem Entwicklungscomputer einen Ordner namens **RemoteMonitoring**. Navigieren Sie in der Befehlszeilenumgebung zu diesem Ordner.

1. Führen Sie die folgenden Befehle zum Herunterladen und Installieren der Pakete aus, die Sie zum Fertigstellen der Beispiel-App benötigen:

    ```
    npm init
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. Erstellen Sie im Ordner **RemoteMonitoring** eine Datei namens **remote_monitoring.js**. Öffnen Sie diese Datei in einem Texteditor.

1. Fügen Sie der Datei **remote_monitoring.js** die folgenden `require`-Anweisungen hinzu:

    ```nodejs
    'use strict';

    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var ConnectionString = require('azure-iot-device').ConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

1. Fügen Sie die folgenden Variablendeklarationen nach den `require` -Anweisungen hinzu. Ersetzen Sie die Platzhalterwerte „[Device Id]“ und „[Device Key]“ durch die Werte, die Sie für Ihr Gerät aus dem Dashboard der Remoteüberwachungslösung notiert haben. Ersetzen Sie „[IoTHub Name]“ durch den IoT Hub-Hostnamen aus dem Lösungsdashboard. Beispiel: Wenn der IoT Hub-Hostname **contoso.azure-devices.net** lautet, ersetzen Sie [IoTHub Name] durch **contoso**:

    ```nodejs
    var connectionString = 'HostName=[IoTHub Name].azure-devices.net;DeviceId=[Device Id];SharedAccessKey=[Device Key]';
    var deviceId = ConnectionString.parse(connectionString).DeviceId;
    ```

1. Fügen Sie die folgenden Variablen hinzu, um einige grundlegende Telemetriedaten zu definieren:

    ```nodejs
    var temperature = 50;
    var humidity = 50;
    var externalTemperature = 55;
    ```

1. Fügen Sie die folgende Hilfsfunktion hinzu, um Ergebnisse des Vorgangs auszugeben:

    ```nodejs
    function printErrorFor(op) {
        return function printError(err) {
            if (err) console.log(op + ' error: ' + err.toString());
        };
    }
    ```

1. Fügen Sie die folgende Hilfsfunktion für die zufällige Festlegung der Telemetriewerte hinzu:

    ```nodejs
    function generateRandomIncrement() {
        return ((Math.random() * 2) - 1);
    }
    ```

1. Fügen Sie die folgende Definition für das **DeviceInfo**-Objekt hinzu, das das Gerät beim Start sendet:

    ```nodejs
    var deviceMetaData = {
        'ObjectType': 'DeviceInfo',
        'IsSimulatedDevice': 0,
        'Version': '1.0',
        'DeviceProperties': {
            'DeviceID': deviceId,
            'HubEnabledState': 1
        }
    };
    ```

1. Fügen Sie die folgende Definition für die vom Gerätezwilling gemeldeten Werte hinzu. Diese Definition umfasst Beschreibungen der direkten Methoden, die das Gerät unterstützt:

    ```nodejs
    var reportedProperties = {
        "Device": {
            "DeviceState": "normal",
            "Location": {
                "Latitude": 47.642877,
                "Longitude": -122.125497
            }
        },
        "Config": {
            "TemperatureMeanValue": 56.7,
            "TelemetryInterval": 45
        },
        "System": {
            "Manufacturer": "Contoso Inc.",
            "FirmwareVersion": "2.22",
            "InstalledRAM": "8 MB",
            "ModelNumber": "DB-14",
            "Platform": "Plat 9.75",
            "Processor": "i3-9",
            "SerialNumber": "SER99"
        },
        "Location": {
            "Latitude": 47.642877,
            "Longitude": -122.125497
        },
        "SupportedMethods": {
            "Reboot": "Reboot the device",
            "InitiateFirmwareUpdate--FwPackageURI-string": "Updates device Firmware. Use parameter FwPackageURI to specifiy the URI of the firmware file"
        },
    }
    ```

1. Fügen Sie die folgende Funktion zur Behandlung des Aufrufs der direkten Methode **Reboot** hinzu:

    ```nodejs
    function onReboot(request, response) {
        // Implement actual logic here.
        console.log('Simulated reboot...');

        // Complete the response
        response.send(200, "Rebooting device", function(err) {
            if(!!err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```

1. Fügen Sie die folgende Funktion zur Behandlung des Aufrufs der direkten Methode **InitiateFirmwareUpdate** hinzu. Diese direkte Methode verwendet einen Parameter zum Angeben des Speicherorts des Firmware-Images, das heruntergeladen werden soll, und leitet das Firmwareupdate auf dem Gerät asynchron ein:

    ```nodejs
    function onInitiateFirmwareUpdate(request, response) {
        console.log('Simulated firmware update initiated, using: ' + request.payload.FwPackageURI);

        // Complete the response
        response.send(200, "Firmware update initiated", function(err) {
            if(!!err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });

        // Add logic here to perform the firmware update asynchronously
    }
    ```

1. Fügen Sie den folgenden Code zum Erstellen einer Clientinstanz hinzu:

    ```nodejs
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. Fügen Sie den folgenden Code zu folgenden Zwecken hinzu:

    * Öffnen der Verbindung.
    * Senden des **DeviceInfo**-Objekts.
    * Einrichten eines Handlers für die gewünschten Eigenschaften.
    * Senden gemeldeter Eigenschaften.
    * Registrieren des Handlers für die direkten Methoden.
    * Starten des Sendens von Telemetriedaten.

    ```nodejs
    client.open(function (err) {
        if (err) {
            printErrorFor('open')(err);
        } else {
            console.log('Sending device metadata:\n' + JSON.stringify(deviceMetaData));
            client.sendEvent(new Message(JSON.stringify(deviceMetaData)), printErrorFor('send metadata'));

            // Create device twin
            client.getTwin(function(err, twin) {
                if (err) {
                    console.error('Could not get device twin');
                } else {
                    console.log('Device twin created');

                    twin.on('properties.desired', function(delta) {
                        console.log('Received new desired properties:');
                        console.log(JSON.stringify(delta));
                    });

                    // Send reported properties
                    twin.properties.reported.update(reportedProperties, function(err) {
                        if (err) throw err;
                        console.log('twin state reported');
                    });

                    // Register handlers for direct methods
                    client.onDeviceMethod('Reboot', onReboot);
                    client.onDeviceMethod('InitiateFirmwareUpdate', onInitiateFirmwareUpdate);
                }
            });

            // Start sending telemetry
            var sendInterval = setInterval(function () {
                temperature += generateRandomIncrement();
                externalTemperature += generateRandomIncrement();
                humidity += generateRandomIncrement();

                var data = JSON.stringify({
                    'DeviceID': deviceId,
                    'Temperature': temperature,
                    'Humidity': humidity,
                    'ExternalTemperature': externalTemperature
                });

                console.log('Sending device event data:\n' + data);
                client.sendEvent(new Message(data), printErrorFor('send event'));
            }, 5000);

            client.on('error', function (err) {
                printErrorFor('client')(err);
                if (sendInterval) clearInterval(sendInterval);
                client.close(printErrorFor('client.close'));
            });
        }
    });
    ```

1. Speichern Sie die Änderungen in der Datei **remote_monitoring.js**.

1. Führen Sie den folgenden Befehl an einer Eingabeaufforderung aus, um die Beispielanwendung zu starten:
   
    ```
    node remote_monitoring.js
    ```

[!INCLUDE [iot-suite-visualize-connecting](../../includes/iot-suite-visualize-connecting.md)]

[lnk-github-repo]: https://github.com/azure/azure-iot-sdk-node
[lnk-github-prepare]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md

