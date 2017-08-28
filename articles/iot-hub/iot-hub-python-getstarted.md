---
title: Erste Schritte mit Azure IoT Hub (Python) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mithilfe der IoT-SDKs für Python D2C-Nachrichten an Azure IoT Hub senden. Erstellen Sie simulierte Geräte- und Dienst-Apps zum Registrieren Ihres Geräts, zum Senden von Nachrichten und zum Lesen von Nachrichten von IoT Hub."
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: 05268924a182575b3df66fb6dad6bcac2700ec0c
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-python"></a>Herstellen einer Verbindung zwischen dem simulierten Gerät und Ihrem IoT Hub mit Python
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über zwei Python-Apps:

* **CreateDeviceIdentity.py**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrer simulierten Geräte-App erstellt.
* **SimulatedDevice.py**: Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und unter Verwendung des MQTT-Protokolls regelmäßig eine Telemetrienachricht gesendet.

> [!NOTE]
> Im Artikel [IoT Hub SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* [Python 2.x oder 3.x][lnk-python-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Fügen Sie Python Ihrer plattformspezifischen Umgebungsvariablen hinzu, wenn Sie während der Installation dazu aufgefordert werden. Bei Verwendung von Python 2.x müssen Sie ggf. *pip*, das Python-Paketverwaltungssystem, [installieren oder aktualisieren][lnk-install-pip].
* Wenn Sie das Windows-Betriebssystem nutzen, wird das [Visual C++ Redistributable Package][lnk-visual-c-redist] verwendet, um die Nutzung von nativen DLLs aus Python zu ermöglichen.
* [Node.js 4.0 oder höher][lnk-node-download]. Stellen Sie je nach Einrichtung sicher, dass die 32-Bit- bzw. die 64-Bit-Installation verwendet wird. Sie ist erforderlich, um das [IoT Hub-Explorer-Tool][lnk-iot-hub-explorer] zu installieren.
* Ein aktives Azure-Konto. Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.

> [!NOTE]
> Die *pip*-Pakete für `azure-iothub-service-client` und `azure-iothub-device-client` sind derzeit nur für das Windows-Betriebssystem verfügbar. Pakete für Linux/Mac OS finden Sie in den Abschnitten zu Linux und Mac OS im Beitrag [Prepare your development environment for Python][lnk-python-devbox] (Vorbereiten der Entwicklungsumgebung für Python).
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Sie haben nun Ihren IoT Hub erstellt. Verwenden Sie für das restliche Tutorial den IoT Hub-Hostnamen und die IoT Hub-Verbindungszeichenfolge.

> [!NOTE]
> Sie können Ihren IoT Hub auch leicht über eine Befehlszeile erstellen, indem Sie die auf Python oder Node.js basierende Azure-CLI nutzen. Der Artikel [Erstellen eines IoT Hubs mit der Azure CLI 2.0][lnk-azure-cli-hub] enthält eine Kurzanleitung mit den erforderlichen Schritten. 
> 

## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität
In diesem Abschnitt sind die Schritte zum Erstellen einer Python-Konsolen-App angegeben, mit der in der Identitätsregistrierung Ihres IoT Hub eine Geräteidentität erstellt wird. Ein Gerät kann nur dann eine Verbindung mit dem IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt **Identitätsregistrierung** des [IoT Hub-Entwicklerhandbuchs][lnk-devguide-identity]. Beim Ausführen dieser Konsolen-App werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Öffnen Sie eine Eingabeaufforderung, und installieren Sie das **Azure IoT Hub Service SDK für Python**. Schließen Sie die Eingabeaufforderung, nachdem Sie das SDK installiert haben.

    ```
    pip install azure-iothub-service-client
    ```

2. Erstellen Sie eine Python-Datei mit dem Namen **CreateDeviceIdentity.py**. Öffnen Sie die Datei in [einem Python-Editor bzw. einer IDE Ihrer Wahl][lnk-python-ide-list], z.B. standardmäßig [IDLE][lnk-idle].

3. Fügen Sie den folgenden Code hinzu, um die erforderlichen Module aus dem Dienst-SDK zu importieren:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Fügen Sie den folgenden Code hinzu, und ersetzen Sie dabei den Platzhalter für `[IoTHub Connection String]` durch die Verbindungszeichenfolge für den im vorherigen Abschnitt erstellten IoT Hub. Als `DEVICE_ID` können Sie einen beliebigen Namen verwenden.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Fügen Sie die folgende Funktion zum Drucken einiger Geräteinformationen hinzu.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Fügen Sie die folgende Funktion hinzu, um die Geräteidentifikation mit dem Registrierungs-Manager zu erstellen. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Fügen Sie abschließend wie folgt die main-Funktion hinzu, und speichern Sie die Datei.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using the Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. Führen Sie an der Eingabeaufforderung wie folgt die Datei **CreateDeviceIdentity.py** aus:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Sie sollten verfolgen können, wie das simulierte Gerät erstellt wird. Notieren Sie sich die **deviceId** und den **primaryKey** dieses Geräts. Sie benötigen diese Werte später beim Erstellen einer Anwendung, für die eine Verbindung mit IoT Hub als Gerät hergestellt wird.

    ![Erstellen des Geräteerfolgs][1]

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].
> 
> 


## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App
In diesem Abschnitt sind die Schritte zum Erstellen einer Python-Konsolen-App beschrieben, mit denen ein Gerät simuliert wird und D2C-Nachrichten (Device to Cloud) an Ihren IoT Hub gesendet werden.

1. Öffnen Sie eine neue Eingabeaufforderung, und installieren Sie wie folgt das Azure IoT Hub Device SDK für Python. Schließen Sie die Eingabeaufforderung nach der Installation.

    ```
    pip install azure-iothub-device-client
    ```
2. Erstellen Sie eine Datei mit dem Namen **SimulatedDevice.py**. Öffnen Sie diese Datei in einem Python-Editor bzw. einer IDE Ihrer Wahl (z.B. IDLE).

3. Fügen Sie den folgenden Code hinzu, um die erforderlichen Module aus dem Geräte-SDK zu importieren.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Fügen Sie den folgenden Code hinzu, und ersetzen Sie den Platzhalter für `[IoTHub Device Connection String]` durch die Verbindungszeichenfolge Ihres Geräts. Die Verbindungszeichenfolge des Geräts hat normalerweise das folgende Format: `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Verwenden Sie die **deviceId** und den **primaryKey** des Geräts, das Sie im vorherigen Abschnitt erstellt haben, um `<deviceId>` bzw. `<primaryKey>` zu ersetzen. Ersetzen Sie `<hostName>` durch den Hostnamen Ihres IoT Hub. Dieser lautet normalerweise `<IoT hub name>.azure-devices.net`.

    ```python
    # String containing Hostname, Device Id & Device Key in the format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Fügen Sie den folgenden Code hinzu, um einen Rückruf für eine Sendebestätigung zu definieren. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Fügen Sie den folgenden Code hinzu, um den Geräteclient zu initialisieren.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set the time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Fügen Sie die folgende Funktion hinzu, um eine Nachricht zu formatieren und von Ihrem simulierten Gerät an den IoT Hub zu senden.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Fügen Sie abschließend die main-Funktion hinzu. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Speichern und schließen Sie die Datei **SimulatedDevice.py**. Sie können die App nun ausführen.

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Empfangen von Nachrichten von Ihrem simulierten Gerät
Zum Empfangen von Telemetrienachrichten von Ihrem Gerät müssen Sie einen mit [Event Hubs][lnk-event-hubs-overview] kompatiblen Endpunkt verwenden, der vom IoT Hub verfügbar gemacht wird, über den die D2C-Nachrichten gelesen werden. Informationen zur Verarbeitung von Nachrichten von Event Hubs für den Event Hub-kompatiblen Endpunkt Ihres IoT Hub finden Sie im Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial]. Für Event Hubs werden Telemetriedaten in Python noch nicht unterstützt. Sie können also entweder eine Event Hubs-basierte [Node.js](iot-hub-node-node-getstarted.md#D2C_node)- oder [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp)-Konsolen-App erstellen, um die D2C-Nachrichten vom IoT Hub zu lesen. In diesem Tutorial wird veranschaulicht, wie Sie das [IoT Hub-Explorer-Tool][lnk-iot-hub-explorer] zum Lesen dieser Gerätenachrichten verwenden können.

1. Öffnen Sie eine Eingabeaufforderung, und installieren Sie den IoT Hub-Explorer. 

    ```
    npm install -g iothub-explorer
    ```

2. Führen Sie an der Eingabeaufforderung den folgenden Befehl aus, um damit zu beginnen, die D2C-Nachrichten von Ihrem Gerät zu überwachen. Verwenden Sie die Verbindungszeichenfolge Ihres IoT Hub im Platzhalter nach `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Öffnen Sie eine neue Eingabeaufforderung, und navigieren Sie zum Verzeichnis mit der Datei **SimulatedDevice.py**.

4. Führen Sie die Datei **SimulatedDevice.py** aus, mit der regelmäßig Telemetriedaten an Ihren IoT Hub gesendet werden. 
   
    ```
    python SimulatedDevice.py
    ```
5. Beachten Sie die Gerätenachrichten an der Eingabeaufforderung, indem Sie den IoT Hub-Explorer aus dem vorherigen Abschnitt ausführen. 

    ![Python-D2C-Nachrichten][2]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die SimulatedDevice-App für das Senden von D2C-Nachrichten an den IoT Hub zu aktivieren. Sie haben mithilfe des IoT Hub-Explorer-Tools die vom IoT Hub empfangenen Nachrichten verfolgt. 

Ausführlichere Informationen zum Python SDK für Azure IoT Hub finden Sie in [diesem GitHub-Repository][lnk-python-github]. Sie können die Datei [iothub_messaging_sample.py][lnk-messaging-sample] herunterladen und ausführen, um Informationen zu den Messagingfunktionen des Azure IoT Hub Service SDK für Python zu erhalten. Für die geräteseitige Simulation mit dem Azure IoT Hub Device SDK für Python können Sie die Datei [iothub_client_sample.py][lnk-client-sample] herunterladen und ausführen.

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Verbinden Ihres Geräts][lnk-connect-device]
* [Erste Schritte mit der Geräteverwaltung][lnk-device-management]
* [Erste Schritte mit Azure IoT Edge][lnk-iot-edge]

Informationen dazu, wie Sie Ihre IoT-Lösung erweitern und Ihre D2C-Nachrichten mit Skalierbarkeit verarbeiten, finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

