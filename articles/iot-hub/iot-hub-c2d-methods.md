---
title: Verwenden von direkten Methoden | Microsoft Docs
description: In diesem Tutorial wird die Verwendung direkter Methoden beschrieben.
services: iot-hub
documentationcenter: ''
author: nberdy
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/05/2016
ms.author: nberdy

---
# <a name="tutorial:-use-direct-methods"></a>Tutorial: Verwenden von direkten Methoden
## <a name="introduction"></a>Einführung
Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Frühere Tutorials ([Erste Schritte mit IoT Hub] und [Senden von C2D-Nachrichten mit IoT Hub]) veranschaulichen die grundlegenden Gerät-zu-Cloud- und Cloud-zu-Gerät-Messagingfunktionen von IoT Hub. IoT Hub gibt Ihnen außerdem die Möglichkeit, nicht dauerhafte Methoden auf Geräten aus der Cloud aufzurufen. Methoden stellen eine Anforderung-Antwort-Interaktion mit einem Gerät dar, die einem HTTP-Aufruf darin ähnelt, dass sie unverzüglich (nach einem vom Benutzer angegebenen Timeout) zu einem Erfolg oder Fehler führt, um den Benutzer über den Status des Aufrufs zu informieren. [Aufrufen von direkten Methoden auf einem Gerät][lnk-devguide-methods] beschreibt Methoden ausführlicher und bietet Hinweise dazu, wann Methoden oder Cloud-zu-Gerät-Nachrichten verwendet werden sollten.

Dieses Lernprogramm beschreibt Folgendes:

* Verwenden des Azure-Portals zum Erstellen eines IoT Hubs und einer Geräteidentität im IoT Hub
* Erstellen eines simulierten Geräts, das eine direkte Methode aufweist, die von der Cloud aufgerufen werden kann
* Erstellen einer Konsolenanwendung, die eine direkte Methode auf dem simulierten Gerät über den IoT Hub aufruft

> [!NOTE]
> Zurzeit ist der Zugriff auf direkte Methoden nur von Geräten aus möglich, die über das MQTT-Protokoll Verbindungen mit IoT Hub herstellen. Anweisungen zum Konvertieren vorhandener Geräte-Apps für die Verwendung von MQTT finden Sie im Artikel [MQTT-Unterstützung][lnk-devguide-mqtt].
> 
> 

Am Ende dieses Tutorials verfügen Sie über zwei Node.js-Konsolenanwendungen:

* **CallMethodOnDevice.js**, die eine Methode auf dem simulierten Gerät aufruft und die Antwort anzeigt.
* **SimulatedDevice.js**, die mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub herstellt und auf die von der Cloud aufgerufene Methode antwortet.

> [!NOTE]
> Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen zu den verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 
> 

Für dieses Lernprogramm benötigen Sie Folgendes:

* Node.js, Version 0.10.x oder höher.
* Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [Azure – Kostenlose Testversion][lnk-free-trial].)

[!INCLUDE [iot-hub-get-started-create-hub-pp](../../includes/iot-hub-get-started-create-hub-pp.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die auf eine von der Cloud aufgerufene Methode antwortet.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulateddevice**. Erstellen Sie im Ordner **simulateddevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **simulateddevice** den folgenden Befehl aus, um das Geräte-SDK-Paket **azure-iot-device** und das Paket **azure-iot-device-mqtt** zu installieren:
   
    ```
    npm install azure-iot-device@dtpreview azure-iot-device-mqtt@dtpreview --save
    ```
3. Erstellen Sie mit einem Text-Editor im Ordner **simulateddevice** die neue Datei **SimulatedDevice.js**.
4. Fügen Sie am Anfang der Datei **SimulatedDevice.js** die folgenden `require`-Anweisungen hinzu:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen eines Geräteclients. Ersetzen Sie **{device connection string}** durch die Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Fügen Sie die folgende Funktion hinzu, um die Methode auf dem Gerät zu implementieren:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Öffnen Sie die Verbindung mit dem IoT Hub, und beginnen Sie mit dem Initialisieren des Methodenlisteners:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Speichern und schließen Sie die Datei **SimulatedDevice.js** .

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen erneuten Verbindungsversuch), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.
> 
> 

## <a name="call-a-method-on-a-device"></a>Aufrufen einer Methode auf einem Gerät
In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die eine Methode auf dem simulierten Gerät aufruft und dann die Antwort anzeigt.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **callmethodondevice**. Erstellen Sie im Ordner **callmethodondevice** die Datei „package.json“, indem Sie an der Eingabeaufforderung den folgenden Befehl eingeben. Übernehmen Sie alle Standardeinstellungen:
   
    ```
    npm init
    ```
2. Führen Sie an der Eingabeaufforderung im Ordner **callmethodondevice** den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:
   
    ```
    npm install azure-iothub@dtpreview --save
    ```
3. Erstellen Sie mit einem Text-Editor die Datei **CallMethodOnDevice.js** im Ordner **callmethodondevice**.
4. Fügen Sie am Anfang der Datei **CallMethodOnDevice.js** die folgenden `require`-Anweisungen hinzu:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Fügen Sie die folgende Variablendeklaration hinzu, und ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für Ihren IoT Hub:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. Erstellen Sie den Client zum Öffnen der Verbindung mit Ihrem IoT Hub.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Fügen Sie die folgende Funktion hinzu, um die Gerätemethode aufrufen und die Antwort des Geräts auf der Konsole anzuzeigen:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'a line to be written',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Speichern und schließen Sie die Datei **CallMethodOnDevice.js** .

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner **simulateddevice** den folgenden Befehl aus, um mit dem Lauschen auf Methodenaufrufe von Ihrem IoT Hub zu beginnen:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Führen Sie an einer Befehlszeile im Ordner **callmethodondevice** den folgenden Befehl aus, um mit der Überwachung Ihres IoT Hubs zu beginnen:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Durch das Ausgeben der Meldung sehen Sie das Gerät, das auf die Methode reagiert. Außerdem können Sie die Anwendung, die die Methode aufgerufen hat, mit der angezeigten Antwort vom Gerät anzeigen:
   
    ![][9]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die simulierte Geräte-App für das Reagieren auf von der Cloud aufgerufene Methoden zu aktivieren. Sie haben außerdem eine App erstellt, die Methoden auf dem Gerät aufruft und die Antwort vom Gerät angezeigt. 

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Erste Schritte mit IoT Hub]
* [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs]

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- Images. -->
[7]: ./media/iot-hub-c2d-methods/run-simulated-device.png
[8]: ./media/iot-hub-c2d-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-c2d-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Senden von Nachrichten von Cloud-zu-Gerät mit IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Verarbeiten von Gerät-zu-Cloud-Nachrichten]: iot-hub-csharp-csharp-process-d2c.md
[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md



<!--HONumber=Oct16_HO2-->


