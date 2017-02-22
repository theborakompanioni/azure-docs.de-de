---
title: "Erste Schritte mit der Azure IoT Hub-Geräteverwaltung (Node) | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe der IoT Hub-Geräteverwaltung den Neustart eines Remotegeräts initiieren. Sie verwenden das Azure IoT SDK für Node.js, um eine simulierte Geräte-App mit einer direkten Methode und eine Dienst-App zu implementieren, die die direkte Methode aufruft."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/30/2016
ms.author: juanpere
translationtype: Human Translation
ms.sourcegitcommit: a243e4f64b6cd0bf7b0776e938150a352d424ad1
ms.openlocfilehash: e1bb89ba369818d7ba0e92a54a4712033f648187


---
# <a name="get-started-with-device-management-node"></a>Erste Schritte mit der Geräteverwaltung (Node)
## <a name="introduction"></a>Einführung
IoT-Cloudanwendungen können Primitive (d.h. die Gerätezwillingsmethode und die direkte Methode) in Azure IoT Hub verwenden, um Geräteverwaltungsaktionen auf Geräten per Remotezugriff zu starten und zu überwachen.  Dieser Artikel enthält Informationen dazu, wie eine IoT-Cloudanwendung und ein Gerät zusammen verwendet werden können, um einen Remoteneustart des Geräts mit IoT Hub zu initiieren und zu überwachen.

Verwenden Sie für den Remotestart und die Remoteüberwachung von Geräteverwaltungsaktionen auf Ihren Geräten von einer cloudbasierten Back-End-App aus Azure IoT Hub-Grundtypen, z.B. die [Gerätezwillingsmethode][lnk-devtwin] und [direkte Methoden][lnk-c2dmethod]. In diesem Tutorial wird veranschaulicht, wie eine Back-End-App und ein Gerät zusammen verwendet werden können, damit Sie einen Remoteneustart des Geräts von IoT Hub aus initiieren und überwachen können.

Sie verwenden eine direkte Methode, um Geräteverwaltungsaktionen (wie Neustarts, Zurücksetzen auf Werkseinstellungen und Firmwareaktualisierung) von einer Back-End-App aus in der Cloud zu initiieren. Das Gerät ist für Folgendes verantwortlich:

* Verarbeiten der von IoT Hub gesendeten Methodenanforderung
* Initiieren der entsprechenden gerätespezifischen Aktion auf dem Gerät
* Senden von Statusupdates über die gemeldeten Eigenschaften an IoT Hub

Sie können eine Back-End-App in der Cloud verwenden, um Gerätezwillingsabfragen auszuführen und Berichte zum Status der Geräteverwaltungsaktionen zu erstellen.

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

* Verwenden des Azure-Portals zum Erstellen einer IoT Hub-Instanz und einer Geräteidentität im IoT Hub
* Erstellen einer simulierten Geräte-App, die eine direkte Methode aufweist, die den Neustart ermöglicht und von der Cloud aufgerufen werden kann
* Erstellen einer Node.js-Konsolenanwendung, die über Ihre IoT Hub-Instanz eine direkte Neustartmethode in der simulierten Geräte-App aufruft

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolen-Apps:

**dmpatterns_getstarted_device.js**: Stellt mit der zuvor erstellten Geräteidentität eine Verbindung mit dem IoT Hub her, empfängt eine direkte Methode zum Neustart, simuliert einen physischen Neustart und meldet den Zeitpunkt des letzten Neustarts.

**dmpatterns_getstarted_service.js**: Ruft eine direkte Methode in der simulierten Geräte-App auf und zeigt die Antwort sowie die gemeldeten aktualisierten Eigenschaften an.

Für dieses Tutorial benötigen Sie Folgendes:

* Node.js Version 0.12.x oder höher, <br/>  Unter [Prepare your development environment][lnk-dev-setup] (Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt werden Sie folgende Schritte ausführen:

* Erstellen einer Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene direkte Methode antwortet
* Auslösen eines simulierten Geräteneustarts
* Verwenden der gemeldeten Eigenschaften, um Gerätezwillingsabfragen zu ermöglichen, die Geräte und den Zeitpunkt ihres letzten Neustarts ermitteln

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **manageddevice**.  Erstellen Sie im Ordner **manageddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **manageddevice** die neue Datei **dmpatterns_getstarted_device.js**.
4. Fügen Sie am Anfang der Datei **dmpatterns_getstarted_device.js** die folgenden require-Anweisungen ein:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen einer **Client**-Instanz.  Ersetzen Sie die Verbindungszeichenfolge durch Ihre Geräteverbindungszeichenfolge.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Fügen Sie die folgende Funktion hinzu, um die direkte Methode auf dem Gerät zu implementieren
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Öffnen Sie die Verbindung mit dem IoT Hub, und starten Sie den Listener der direkten Methode:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Speichern und schließen Sie die Datei **dmpatterns_getstarted_device.js**.
   
   [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Auslösen eines Remoteneustarts auf dem Gerät über eine direkte Methode
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die mit einer direkten Methode einen Remoteneustart auf einem Gerät initiiert und mithilfe von Gerätezwillingsabfragen den letzten Neustartzeitpunkt für dieses Gerät abruft.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **triggerrebootondevice**.  Erstellen Sie im Ordner **triggerrebootondevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen.  Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **triggerrebootondevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iothub** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iothub --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **triggerrebootondevice** die neue Datei **dmpatterns_getstarted_service.js**.
4. Fügen Sie am Anfang der Datei **dmpatterns_getstarted_service.js** die folgenden require-Anweisungen ein:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Fügen Sie die folgenden Variablendeklarationen hinzu, und ersetzen Sie die Platzhalterwerte:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Fügen Sie die folgende Funktion hinzu, um durch Aufrufen der Gerätemethode das Zielgerät neu zu starten:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```
7. Fügen Sie die folgende Funktion hinzu, um das Gerät abzufragen und den letzten Neustartzeitpunkt zu erhalten:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
8. Fügen Sie den folgenden Code hinzu, um die Funktionen aufrufen, die die direkte Methode zum Neustart auslösen und den letzten Neustartzeitpunkt abfragen:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Speichern und schließen Sie die Datei **dmpatterns_getstarted_service.js**.

## <a name="run-the-apps"></a>Ausführen der Apps
Sie können die Apps nun ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **manageddevice** den folgenden Befehl aus, um mit dem Lauschen auf die direkte Methode zum Neustarten zu beginnen.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **triggerrebootondevice** den folgenden Befehl aus, um den Remoteneustart und die Abfrage an den Gerätezwilling zum Ermitteln des letzten Neustartzeitpunkts auszulösen.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. Die Reaktion des Geräts auf die direkte Methode wird in der Konsole angezeigt.

## <a name="customize-and-extend-the-device-management-actions"></a>Anpassen und Erweitern der Geräteverwaltungsaktionen
Ihre IoT-Lösungen können die festgelegten Geräteverwaltungsmuster erweitern oder benutzerdefinierte Muster ermöglichen. Dazu werden die Primitiven für die Gerätezwillingsmethode oder die direkte Methode verwendet. Andere Beispiele für Geräteverwaltungsaktionen sind das Zurücksetzen auf die Werkseinstellungen, Firmware- und Softwareaktualisierungen, Energieverwaltung, Netzwerk- und Konnektivitätsverwaltung und Datenverschlüsselung.

## <a name="device-maintenance-windows"></a>Gerätewartungsfenster
In der Regel konfigurieren Sie die Ausführung von Aktionen für Geräte so, dass Unterbrechungen und Ausfallzeiten auf ein Minimum beschränkt sind.  Bei Gerätewartungsfenstern handelt es sich um ein häufig verwendetes Muster zum Festlegen des Zeitpunkts, zu dem ein Gerät seine Konfiguration aktualisieren soll. Ihre Back-End-Lösungen können die gewünschten Eigenschaften des Gerätezwillings verwenden, um auf Ihrem Gerät eine Richtlinie zur Aktivierung eines Wartungsfensters festzulegen und zu aktivieren. Wenn ein Gerät die Wartungsfensterrichtlinie erhält, kann es mithilfe der gemeldeten Eigenschaft des Gerätezwillings den Richtlinienstatus melden. Die Back-End-App kann dann mithilfe von Gerätezwillingsabfragen die Konformität von Geräten und den einzelnen Richtlinien sicherstellen.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie eine direkte Methode zum Auslösen eines Remoteneustarts auf einem Gerät und die gemeldeten Eigenschaften zum Melden des letzten Neustartzeitpunkts des Geräts verwendet. Darüber hinaus haben Sie den Gerätezwilling abgefragt, um den letzten Neustartzeitpunkt des Geräts aus der Cloud zu ermitteln.

Informationen zu den weiteren ersten Schritten mit IoT Hub und Geräteverwaltungsmustern, z.B. drahtloses Firmware-Remoteupdate, finden Sie unter:

[Tutorial: Durchführen eines Firmwareupdates][lnk-fwupdate]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

Informationen zu den weiteren ersten Schritten mit IoT Hub finden Sie unter [Erste Schritte mit dem Azure IoT Gateway SDK (Linux)][lnk-gateway-SDK].

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx



<!--HONumber=Dec16_HO1-->


