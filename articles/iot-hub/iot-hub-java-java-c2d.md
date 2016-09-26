<properties
	pageTitle="Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Azure"
	description="In diesem Tutorial erfahren Sie, wie Sie mithilfe von Azure IoT Hub und Java Cloud-zu-Gerät-Nachrichten senden."
	services="iot-hub"
	documentationCenter="java"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/13/2016"
     ms.author="dobett"/>

# Tutorial: Senden von Cloud-zu-Gerät-Nachrichten mithilfe von IoT Hub und Java

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## Einführung

Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Im Lernprogramm [Erste Schritte mit IoT Hub] erfahren Sie, wie ein IoT Hub erstellt, eine Geräteidentität im Hub bereitgestellt und ein simuliertes Gerät programmiert wird, das Gerät-zu-Cloud-Nachrichten sendet.

Dieses Tutorial baut auf [Erste Schritte mit IoT Hub] auf. Es beschreibt Folgendes:

- Senden von C2D-Nachrichten aus Ihrem Anwendungs-Cloud-Back-End an ein einzelnes Gerät über IoT Hub.
- Empfangen von C2D-Nachrichten auf einem Gerät.
- Anfordern einer Übermittlungsbestätigung (*Feedback*) von Ihrem Anwendungs-Cloud-Back-End für Nachrichten, die von IoT Hub an ein einzelnes Gerät gesendet wurden.

Weitere Informationen zu Cloud-zu-Gerät-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

Am Ende dieses Tutorials führen Sie zwei Java-Konsolenanwendungen aus:

* **simulated-device**, eine abgewandelte Version der in [Erste Schritte mit IoT Hub] erstellten App, die eine Verbindung mit IoT Hub herstellt und C2D-Nachrichten empfängt.
* **send-c2d-messages**, die über IoT Hub eine C2D-Nachricht an das simulierte Gerät sendet und dann die zugehörige Übermittlungsbestätigung empfängt.

> [AZURE.NOTE] IoT Hub bietet durch Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, Java und JavaScript). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit dem Code in diesem Tutorial sowie allgemeine Informationen zum Verbinden mit Azure IoT Hub.

Für dieses Tutorial benötigen Sie Folgendes:

+ Java SE 8. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Java unter Windows oder Linux installieren.

+ Maven 3. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Maven unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].)

## Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Erste Schritte mit IoT Hub] erstellt haben, um C2D-Nachrichten von IoT Hub zu empfangen.

1. Öffnen Sie die Datei „simulated-device\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

2. Fügen Sie die folgende **MessageCallback**-Klasse als geschachtelte Klasse innerhalb der **App**-Klasse hinzu. Die **execute**-Methode wird aufgerufen, wenn das Gerät eine Nachricht von IoT Hub empfängt. In diesem Beispiel informiert das Gerät den Hub immer darüber, dass die Nachricht abgeschlossen wurde:

    ```
    private static class MessageCallback implements
    com.microsoft.azure.iothub.MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));

        return IotHubMessageResult.COMPLETE;
      }
    }
    ```

3. Ändern Sie die **main**-Methode so, dass vor dem Öffnen des Clients eine **MessageCallback**-Instanz erstellt und die **SetMessageCallback**-Methode aufgerufen wird:

    ```
    client = new DeviceClient(connString, protocol);

    MessageCallback callback = new MessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [AZURE.NOTE] Wenn Sie anstelle von AMQP den HTTP/1-Transport verwenden, prüft die **DeviceClient**-Instanz nur selten (weniger als alle 25 Minuten), ob Nachrichten von IoT Hub vorliegen. Weitere Informationen zu den Unterschieden zwischen der AMQP- und HTTP/1-Unterstützung sowie zur IoT Hub-Drosselung finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

## Senden einer C2D-Nachricht

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die C2D-Nachrichten an die simulierte Geräte-App sendet. Sie benötigen die Geräte-ID des Geräts, das Sie im Tutorial [Erste Schritte mit IoT Hub] hinzugefügt haben. Sie benötigen auch die Verbindungszeichenfolge für den IoT Hub, die Sie im [Azure-Portal] finden.

1. Erstellen Sie mithilfe des folgenden Befehls über die Eingabeaufforderung ein Maven-Projekt namens **send-c2d-messages**. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie an der Eingabeaufforderung zu dem neuen Ordner „send-c2d-messages“.

3. Öffnen Sie die Datei „pom.xml“ aus dem Ordner „send-c2d-messages“ in einem Text-Editor, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Durch Hinzufügen der Abhängigkeit können Sie in Ihrer Anwendung das Paket **iothub-java-service-client** verwenden, um mit Ihrem IoT Hub-Dienst zu kommunizieren:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.7</version>
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „send-c2d-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“ in einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import**-Anweisungen hinzu:

    ```
    import com.microsoft.azure.iot.service.sdk.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Fügen Sie der **App**-Klasse die folgenden Klassenebenenvariablen hinzu, und ersetzen Sie dabei **{yourhubconnectionstring}** und **{yourdeviceid}** durch die zuvor notierten Werte:

    ```
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```
    
8. Ersetzen Sie die **main**-Methode durch den folgenden Code. Dieser stellt die Verbindung mit Ihrer IoT Hub-Instanz her, sendet eine Nachricht an Ihr Gerät und wartet dann auf eine Empfangs- und Verarbeitungsbestätigung des Geräts:

    ```
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
      
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver(deviceId);
        if (feedbackReceiver != null) feedbackReceiver.open();

        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);

        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");

        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }

        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.

## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an Ihren IoT Hub und Empfangen von C2D-Nachrichten, die von Ihrem Hub gesendet werden, zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Ausführen der simulierten Geräte-App][img-simulated-device]

2. Führen Sie an einer Befehlszeile im Ordner „send-c2d-messages“ den folgenden Befehl aus, um eine C2D-Nachricht zu senden, und warten Sie auf eine Feedbackbestätigung:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Ausführen des Befehls zum Senden der C2D-Nachricht][img-send-command]

## Nächste Schritte

In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten gesendet und empfangen werden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].


<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]: media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Erste Schritte mit IoT Hub]: iot-hub-java-java-getstarted.md
[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d
[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/java-devbox-setup.md
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-Portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

<!---HONumber=AcomDC_0914_2016-->