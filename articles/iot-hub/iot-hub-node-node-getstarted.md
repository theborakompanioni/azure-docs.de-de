<properties
	pageTitle="Erste Schritte mit Azure IoT Hub für Node.js | Microsoft Azure"
	description="Tutorial für den Einstieg in Azure IoT Hub mit Node.js. Verwenden Sie Azure IoT Hub und Node.js mit den Microsoft Azure IoT SDKs, um eine IoT-Lösung zu implementieren."
	services="iot-hub"
	documentationCenter="nodejs"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="javascript"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="03/22/2016"
     ms.author="dobett"/>

# Erste Schritte mit Azure IoT Hub für Node.js

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Lösungs-Back-End ermöglicht. Eines der größten Probleme im Zusammenhang mit IoT-Projekten ist die sichere und zuverlässige Verbindung von Geräten mit dem Lösungs-Back-End. Um diese Herausforderungen zu meistern, bietet IoT Hub:

- Ein zuverlässiges, hyperskalierbares Messaging zwischen Geräten und Cloud (Device-to-Cloud, D2C) sowie zwischen Cloud und Geräten (Cloud-to-Device, C2D)
- Eine sichere Kommunikation unter Verwendung von Zugriffssteuerung und Sicherheitsanmeldeinformationen auf Gerätebasis
- Gerätebibliotheken für die gängigsten Sprachen und Plattformen

Dieses Tutorial veranschaulicht folgende Vorgehensweisen:

- Erstellen eines IoT Hubs mit dem Azure-Portal
- Erstellen einer Geräteidentität im IoT Hub
- Erstellen Sie ein simuliertes Gerät, das Telemetriedaten an Ihr Cloud-Back-End sendet.

Am Ende dieses Tutorials verfügen Sie über drei Node.js-Konsolenanwendungen:

* **CreateDeviceIdentity.js**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrem simulierten Gerät erstellt.
* **ReadDEviceToCloudMessages.js**: Hiermit wird die Telemetrie angezeigt, die Ihr simuliertes Gerät sendet.
* **SimulatedDevice.js**. Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des AMQPS-Protokolls eine Telemetrienachricht gesendet.

> [AZURE.NOTE] Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

+ Node.js Version 0.12.x oder höher. <br/> Unter [Prepare your development environment][lnk-dev-setup] \(Vorbereiten Ihrer Entwicklungsumgebung) wird beschrieben, wie Sie Node.js für dieses Tutorial unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. <br/>Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Klicken Sie im letzten Schritt auf dem IoT Hub-Blatt auf **Einstellungen** und auf dem Blatt **Einstellungen** auf **Messaging**. Notieren Sie sich auf dem Blatt **Messaging** die Angaben für **Event Hub-kompatibler Name** und **Event Hub-kompatibler Endpunkt**. Sie benötigen diese Werte beim Erstellen der Anwendung **read-d2c-messages**.

![][6]

Sie haben nun Ihren IoT Hub erstellt und verfügen über den IoT Hub-Hostnamen, die IoT Hub-Verbindungszeichenfolge, den Event Hub-kompatiblen Namen und Event Hub-kompatible Endpunktwerte. Diese Angaben benötigen Sie, um den Rest dieses Tutorials durcharbeiten zu können.

## Erstellen einer Geräteidentität

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die eine neue Geräteidentität in der Identitätsregistrierung Ihres IoT Hubs erstellt. Ein Gerät kann nur eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteidentitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt **Geräteidentitätsregistrierung** des [Entwicklungsleitfadens für IoT Hub][lnk-devguide-identity]. Beim Ausführen dieser Konsolenanwendung werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **createdeviceidentity**. Erstellen Sie im Ordner **createdeviceidentity** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner **createdeviceidentity** den folgenden Befehl aus, um das Paket **azure-iothub** zu installieren:

    ```
    npm install azure-iothub --save
    ```

3. Erstellen Sie mit einem Text-Editor im Ordner **createdeviceidentity** die neue Datei **CreateDeviceIdentity.js**.

4. Fügen Sie am Anfang der Datei **CreateDeviceIdentity.js** die folgende `require`-Anweisung hinzu:

    ```
    'use strict';
    
    var iothub = require('azure-iothub');
    ```

5. Fügen Sie der Datei **CreateDeviceIdentity.js** den folgenden Code hinzu, und ersetzen Sie den Platzhalterwert durch die Verbindungszeichenfolge für den IoT Hub, den Sie im vorherigen Abschnitt erstellt haben:

    ```
    var connectionString = '{iothub connection string}';
    
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```

6. Fügen Sie den unten angegebenen Code hinzu, um in der Geräteidentitätsregistrierung im IoT Hub eine neue Gerätedefinition zu erstellen. Mit diesem Code wird ein neues Gerät erstellt, wenn die Geräte-ID in der Registrierung nicht vorhanden ist. Andernfalls wird der Schlüssel des vorhandenen Geräts zurückgegeben:

    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });

    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```

7. Speichern und schließen Sie die Datei **CreateDeviceIdentity.js**.

8. Führen Sie zum Ausführen der Anwendung **create-device-identity** den folgenden Befehl an der Eingabeaufforderung im Ordner „createdeviceidentity“ aus:

    ```
    node CreateDeviceIdentity.js 
    ```

9. Notieren Sie sich die **Geräte-ID** und den **Geräteschlüssel**. Sie benötigen diese Angaben später beim Erstellen einer Anwendung, für die eine Verbindung mit IoT Hub als Gerät hergestellt wird.

> [AZURE.NOTE] Die Identitätsregistrierung im IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

## Empfangen von Gerät-an-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Node.js-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub liest. Ein IoT Hub macht einen [Event Hubs][lnk-event-hubs-overview]-kompatiblen Endpunkt verfügbar, der Ihnen das Lesen von D2C-Nachrichten ermöglicht. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial] wird gezeigt, wie Sie D2C-Nachrichten in größerem Umfang verarbeiten. Das Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial] enthält weitere Informationen zum Verarbeiten der Nachrichten von Event Hubs und gilt für Endpunkte, die mit IoT Hub-Event Hubs kompatibel sind.

> [AZURE.NOTE] Der mit Event Hubs kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQPS-Protokoll.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **readdevicetocloudmessages**. Erstellen Sie im Ordner **readdevicetocloudmessages** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner **readdevicetocloudmessages** den folgenden Befehl aus, um die Pakete **amqp10** und **bluebird** zu installieren:

    ```
    npm install amqp10 bluebird --save
    ```

3. Erstellen Sie mit einem Text-Editor im Ordner **readdevicetocloudmessages** die neue Datei **ReadDeviceToCloudMessages.js**.

4. Fügen Sie am Anfang der Datei **ReadDeviceToCloudMessages.js** die folgenden `require`-Anweisungen hinzu:

    ```
    'use strict';

    var AMQPClient = require('amqp10').Client;
    var Policy = require('amqp10').Policy;
    var translator = require('amqp10').translator;
    var Promise = require('bluebird');
    ```

5. Fügen Sie die folgenden Variablendeklarationen hinzu, und ersetzen Sie die Platzhalter durch die Werte, die Sie zuvor notiert haben. Der Wert des Platzhalters **{your event hub-compatible namespace}** stammt vom Feld **Event Hub-kompatibler Endpunkt** im Portal und hat das folgende Format: **namespace.servicebus.windows.net** (ohne das Präfix **sb://*).

    ```
    var protocol = 'amqps';
    var eventHubHost = '{your event hub-compatible namespace}';
    var sasName = 'iothubowner';
    var sasKey = '{your iot hub key}';
    var eventHubName = '{your event hub-compatible name}';
    var numPartitions = 2;
    ```

    > [AZURE.NOTE] In diesem Code wird davon ausgegangen, dass Sie Ihren IoT Hub auf der Ebene F1 (Free) erstellt haben. Ein kostenloser IoT Hub verfügt über zwei Partitionen mit den Bezeichnungen „0“ und „1“. Wenn Sie Ihren IoT Hub mit einem der anderen Tarife erstellt haben, sollten Sie den Code so anpassen, dass ein **MessageReceiver**-Element für jede Partition erstellt wird.

6. Fügen Sie die folgende Filterdefinition hinzu. Bei dieser Anwendung wird beim Erstellen eines Receiver-Elements ein Filter verwendet, damit das Receiver-Element nur Nachrichten liest, die nach Beginn der Ausführung des Receiver-Elements an IoT Hub gesendet werden. Dies ist in einer Testumgebung nützlich, da Sie den aktuellen Satz von Nachrichten anzeigen können. In einer Produktionsumgebung sollte Ihr Code hingegen sicherstellen, dass alle Nachrichten verarbeitet werden. Weitere Informationen hierzu finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

    ```
    var filterOffset = new Date().getTime();
    var filterOption;
    if (filterOffset) {
      filterOption = {
      attach: { source: { filter: {
      'apache.org:selector-filter:string': translator(
        ['described', ['symbol', 'apache.org:selector-filter:string'], ['string', "amqp.annotation.x-opt-enqueuedtimeutc > " + filterOffset + ""]])
        } } }
      };
    }
    ```

7. Fügen Sie den folgenden Code hinzu, um die Empfangsadresse und einen AMQP-Client zu erstellen:

    ```
    var uri = protocol + '://' + encodeURIComponent(sasName) + ':' + encodeURIComponent(sasKey) + '@' + eventHubHost;
    var recvAddr = eventHubName + '/ConsumerGroups/$default/Partitions/';
    
    var client = new AMQPClient(Policy.EventHub);
    ```

8. Fügen Sie die folgenden beiden Funktionen hinzu, mit denen die Ausgabe an die Konsole erfolgt:

    ```
    var messageHandler = function (partitionId, message) {
      console.log('Received(' + partitionId + '): ', message.body);
    };
    
    var errorHandler = function(partitionId, err) {
      console.warn('** Receive error: ', err);
    };
    ```

9. Fügen Sie die folgende Funktion hinzu, die als Receiver-Element für eine Partition mit Verwendung des Filters fungiert:

    ```
    var createPartitionReceiver = function(partitionId, receiveAddress, filterOption) {
      return client.createReceiver(receiveAddress, filterOption)
        .then(function (receiver) {
          console.log('Listening on partition: ' + partitionId);
          receiver.on('message', messageHandler.bind(null, partitionId));
          receiver.on('errorReceived', errorHandler.bind(null, partitionId));
        });
    };
    ```

10. Fügen Sie den folgenden Code hinzu, um eine Verbindung mit dem Event Hub-kompatiblen Endpunkt herzustellen und die Receiver-Elemente zu starten:

    ```
    client.connect(uri)
      .then(function () {
        var partitions = [];
        for (var i = 0; i < numPartitions; ++i) {
          partitions.push(createPartitionReceiver(i, recvAddr + i, filterOption));
        }
        return Promise.all(partitions);
    })
    .error(function (e) {
        console.warn('Connection error: ', e);
    });
    ```

11. Speichern und schließen Sie die Datei **ReadDeviceToCloudMessages.js**.

## Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Node.js-Konsolenanwendung, die ein Gerät simuliert, mit dem D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) an einen IoT Hub gesendet werden.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen **simulateddevice**. Erstellen Sie im Ordner **simulateddevice** die neue Datei „package.json“, indem Sie an der Eingabeaufforderung den unten angegebenen Befehl verwenden. Übernehmen Sie alle Standardeinstellungen:

    ```
    npm init
    ```

2. Führen Sie an der Eingabeaufforderung im Ordner **simulateddevice** den folgenden Befehl aus, um das Paket **azure-iot-device-amqp** zu installieren:

    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```

3. Erstellen Sie mit einem Text-Editor im Ordner **simulateddevice** die neue Datei **SimulatedDevice.js**.

4. Fügen Sie am Anfang der Datei **SimulatedDevice.js** die folgenden `require`-Anweisungen hinzu:

    ```
    'use strict';

    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```

5. Fügen Sie die Variable **connectionString** hinzu, und verwenden Sie sie zum Erstellen eines Geräteclients. Ersetzen Sie **{youriothubname}** durch den Namen Ihres IoT Hubs und **{yourdeviceid}** sowie **{yourdevicekey}** durch die Gerätewerte, die Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```
    var connectionString = 'HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    
    var client = clientFromConnectionString(connectionString);
    ```

6. Fügen Sie die folgende Funktion hinzu, um die Ausgabe der Anwendung anzuzeigen:

    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

7. Erstellen Sie einen Rückruf, und verwenden Sie die Funktion **setInterval**, um jede Sekunde eine neue Nachricht an Ihren IoT Hub zu senden:

    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');

        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'mydevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 2000);
      }
    };
    ```

8. Öffnen Sie die Verbindung mit dem IoT Hub, und beginnen Sie mit dem Senden von Nachrichten:

    ```
    client.open(connectCallback);
    ```

9. Speichern und schließen Sie die Datei **SimulatedDevice.js**.

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z. B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.


## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an der Eingabeaufforderung im Ordner **readdevicetocloudmessages** den folgenden Befehl aus, um mit der Überwachung von IoT Hub zu beginnen:

    ```
    node ReadDeviceToCloudMessages.js 
    ```

    ![][7]

2. Führen Sie an einer Befehlszeile im Ordner **simulateddevice** den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an IoT Hub zu beginnen:

    ```
    node SimulatedDevice.js
    ```

    ![][8]

3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den Hub gesendeten Nachrichten angezeigt:

    ![][43]

## Nächste Schritte

In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um der simulierten Geräte-App das Senden von D2C-Nachrichten an den Hub zu ermöglichen, und Sie haben eine App erstellt, die die vom Hub empfangenen Nachrichten anzeigt. In den folgenden Tutorials werden weitere Features und Szenarien für IoT Hubs vorgestellt:

- Unter [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d-tutorial] wird erläutert, wie Sie Nachrichten an Geräte senden und das von IoT Hub generierte Feedback zur Übermittlung verarbeiten.
- In [Verarbeiten von D2C-Nachrichten][lnk-process-d2c-tutorial] wird erläutert, wie Sie zuverlässig Telemetriedaten und interaktive Nachrichten von Geräten verarbeiten können.
- In [Hochladen von Dateien von Geräten][lnk-upload-tutorial] wird beschrieben, wie mithilfe von C2D-Nachrichten Dateiuploads von Geräten erleichtert werden.

<!-- Images. -->
[6]: ./media/iot-hub-node-node-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0615_2016-->