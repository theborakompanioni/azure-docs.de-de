<properties
	pageTitle="Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Azure"
	description="In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure IoT Hub und Java Cloud-zu-Gerät-Nachrichten senden."
	services="iot-hub"
	documentationCenter="nodejs"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="07/04/2016"
     ms.author="dobett"/>

# Tutorial: Senden von C2D-Nachrichten mithilfe von IoT Hub und „Node.js“

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Im Lernprogramm [Erste Schritte mit IoT Hub] erfahren Sie, wie ein IoT Hub erstellt, eine Geräteidentität im Hub bereitgestellt und ein simuliertes Gerät programmiert wird, das Gerät-zu-Cloud-Nachrichten sendet.

Dieses Tutorial baut auf [Erste Schritte mit IoT Hub] auf. Es beschreibt Folgendes:

- Senden von C2D-Nachrichten aus Ihrem Anwendungs-Cloud-Back-End an ein einzelnes Gerät über IoT Hub.
- Empfangen von C2D-Nachrichten auf einem Gerät.
- Anfordern einer Übermittlungsbestätigung (*Feedback*) von Ihrem Anwendungs-Cloud-Back-End für Nachrichten, die von IoT Hub an ein einzelnes Gerät gesendet wurden.

Weitere Informationen zu Cloud-zu-Gerät-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

Am Ende dieses Tutorials führen Sie zwei Node.js-Konsolenanwendungen aus:

* **SimulatedDevice**, eine abgewandelte Version der in [Erste Schritte mit IoT Hub] erstellten App, die eine Verbindung mit IoT Hub herstellt und Cloud-zu-Gerät-Nachrichten empfängt.
* **SendCloudToDeviceMessage**, die über IoT Hub eine C2D-Nachricht an das simulierte Gerät sendet und dann die zugehörige Übermittlungsbestätigung empfängt.

> [AZURE.NOTE] IoT Hub bietet durch Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, Java und JavaScript). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit dem Code in diesem Tutorial sowie allgemeine Informationen zum Verbinden mit Azure IoT Hub.

Für dieses Tutorial benötigen Sie Folgendes:

+ Node.js Version 0.12.x oder höher. <br/> Unter [Prepare your development environment][lnk-dev-setup] \(Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].)

## Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Erste Schritte mit IoT Hub] erstellt haben, um C2D-Nachrichten von IoT Hub zu empfangen.

1. Öffnen Sie die Datei „SimulatedDevice.js“ mit einem Text-Editor.

2. Ändern Sie die **connectCallback**-Funktion, sodass sie von IoT Hub gesendete Nachrichten verarbeitet. In diesem Beispiel ruft das Gerät immer die **complete**-Funktion auf, um IoT Hub mitzuteilen, dass sie die Nachricht verarbeitet hat. Die neue Version der **connectCallback**-Funktion sieht folgendermaßen aus:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```

    > [AZURE.NOTE] Wenn Sie anstelle von AMQP den HTTP/1-Transport verwenden, prüft die **DeviceClient**-Instanz nur selten (weniger als alle 25 Minuten), ob Nachrichten von IoT Hub vorliegen. Weitere Informationen zu den Unterschieden zwischen der AMQP- und HTTP/1-Unterstützung sowie zur IoT Hub-Drosselung finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

## Senden einer C2D-Nachricht

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die C2D-Nachrichten an die simulierte Geräte-App sendet. Dazu benötigen Sie die Geräte-ID des Geräts, das Sie im Tutorial [Erste Schritte mit IoT Hub] hinzugefügt haben, sowie die Verbindungszeichenfolge für Ihre IoT Hub-Instanz, die Sie im [Azure-Portal] finden können.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **sendcloudtodevicemessage**. Erstellen Sie im Ordner **sendcloudtodevicemessage** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie im Ordner **sendcloudtodevicemessage** an der Eingabeaufforderung den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:

    ```
    npm install azure-iothub --save
    ```

3. Mithilfe eines Texteditors erstellen Sie eine neue Datei **SendCloudToDeviceMessage.js** im Ordner **sendcloudtodevicemessage**.

4. Fügen Sie am Anfang der Datei **SendCloudToDeviceMessage.js** die folgenden `require`-Anweisungen hinzu:

    ```
    'use strict';
    
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```

5. Fügen Sie **SendCloudToDeviceMessage.js** den folgenden Code hinzu, und ersetzen Sie den Verbindungszeichenfolgen-Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, und den Platzhalter für das Zielgerät durch die Geräte-ID des Geräts, das Sie im Tutorial [Erste Schritte mit IoT Hub] hinzugefügt haben:

    ```
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';

    var serviceClient = Client.fromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion hinzu, um Ergebnisse des Vorgangs an der Konsole auszugeben:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Fügen Sie die folgende Funktion hinzu, um Übermittlungsfeedbacknachrichten an der Konsole auszugeben:

    ```
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```

8. Fügen Sie den folgenden Code hinzu, um eine Nachricht an Ihr Gerät zu senden und die Feedbacknachricht zu verarbeiten, wenn das Gerät die C2D-Nachricht bestätigt:

    ```
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```

7. Speichern und schließen Sie die Datei **SendCloudToDeviceMessage.js**.

## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner **simulateddevice** den folgenden Befehl aus, um Telemetriedaten an Ihren IoT Hub zu senden und C2D-Nachrichten zu empfangen:

    ```
    node SimulatedDevice.js 
    ```

    ![Ausführen der simulierten Geräte-App][img-simulated-device]

2. Führen Sie an einer Befehlszeile im Ordner **sendcloudtodevicemessage** den folgenden Befehl aus, um eine C2D-Nachricht zu senden, und warten Sie auf das Bestätigungsfeedback:

    ```
    node SendCloudToDeviceMessage.js 
    ```

    ![Ausführen der App zum Senden des C2D-Befehls][img-send-command]

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

## Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten gesendet und empfangen werden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]: media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Erste Schritte mit IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d
[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-Portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

<!---HONumber=AcomDC_0727_2016-->