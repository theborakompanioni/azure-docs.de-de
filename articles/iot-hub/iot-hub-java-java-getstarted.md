---
title: Erste Schritte mit Azure IoT Hub (Java) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mithilfe der IoT-SDKs für Java D2C-Nachrichten an Azure IoT Hub senden. Erstellen Sie simulierte Geräte- und Dienst-Apps zum Registrieren Ihres Geräts, zum Senden von Nachrichten und zum Lesen von Nachrichten von IoT Hub."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 707356a49970bcd76a55ee1b8a6fbddf6a6ba390
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a>Herstellen einer Verbindung zwischen Ihrem Gerät und Ihrem IoT Hub mit Java
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Am Ende dieses Tutorials verfügen Sie über drei Java-Konsolen-Apps:

* **create-device-identity**: Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrer Geräte-App erstellt.
* **read-d2c-messages**: Hiermit wird die Telemetrie angezeigt, die Ihre Geräte-App sendet.
* **simulated-device**: Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des MQTT-Protokolls eine Telemetrienachricht gesendet.

> [!NOTE]
> Im Artikel [Azure IoT SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Apps für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

* Das neueste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Ein aktives Azure-Konto. (Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto][lnk-free-trial] erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Notieren Sie sich abschließend den Wert unter **Primärschlüssel**. Klicken Sie dann auf **Endpunkte** und den integrierten Endpunkt **Ereignisse**. Notieren Sie sich auf dem Blatt **Eigenschaften** die Angaben für **Event Hub-kompatibler Name** und die Adresse unter **Event Hub-kompatibler Endpunkt**. Sie benötigen diese drei Werte beim Erstellen der **read-d2c-messages**-Anwendung.

![Azure-Portal – IoT Hub – Blatt „Messaging“][6]

Sie haben nun Ihren IoT Hub erstellt. Sie verfügen jetzt über den IoT Hub-Hostnamen, die IoT Hub-Verbindungszeichenfolge, den IoT Hub-Primärschlüssel, den Event Hub-kompatiblen Namen und den Event Hub-kompatiblen Endpunkt. Diese Angaben benötigen Sie für die Schritte in diesem Tutorial.

## <a name="create-a-device-identity"></a>Erstellen einer Geräteidentität
In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, mit der eine Geräteidentität in der Identitätsregistrierung Ihres IoT Hub erstellt wird. Ein Gerät kann nur eine Verbindung mit IoT Hub herstellen, wenn in der Identitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt **Identitätsregistrierung** des [IoT Hub-Entwicklerhandbuchs][lnk-devguide-identity]. Beim Ausführen dieser Konsolen-App werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Erstellen Sie einen leeren Ordner mit dem Namen „iot-java-get-started“. Erstellen Sie im Ordner „iot-java-get-started“ ein Maven-Projekt namens **create-device-identity**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie an der Eingabeaufforderung zum Ordner „create-device-identity“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „create-device-identity“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche][lnk-maven-service-search].

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „create-device-identity\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Fügen Sie die folgenden Klassenebenenvariablen der **App**-Klasse hinzu, und ersetzen Sie dabei **{yourhubconnectionstring}** durch den zuvor notierten Wert:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Ändern Sie die Signatur der **main** -Methode, um die Ausnahmen wie folgt einzufügen:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Fügen Sie den folgenden Code als Textkörper der **main** -Methode hinzu. Mit diesem Code wird ein Gerät namens *javadevice* in Ihrer IoT Hub-Identitätsregistrierung erstellt, sofern es noch nicht vorhanden ist. Anschließend werden die Geräte-ID und der Schlüssel angezeigt, die Sie später benötigen:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Speichern und schließen Sie die Datei „App.java“.

11. Führen Sie zum Erstellen der App **create-device-identity** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „create-device-identity“ aus:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Führen Sie zum Ausführen der Anwendung **create-device-identity** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „create-device-identity“ aus:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Notieren Sie sich die **Geräte-ID** und den **Geräteschlüssel**. Sie benötigen diese Werte später beim Erstellen einer App, für die eine Verbindung mit IoT Hub als Gerät hergestellt wird.

> [!NOTE]
> Die Identitätsregistrierung in IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den IoT-Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre App das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein App-spezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Empfangen von Gerät-an-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub liest. Ein IoT-Hub macht einen mit [Event Hub][lnk-event-hubs-overview] kompatiblen Endpunkt verfügbar, um Ihnen das Lesen von D2C-Nachrichten zu ermöglichen. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial] wird gezeigt, wie Sie D2C-Nachrichten mit Skalierung verarbeiten. Das Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial] enthält weitere Informationen zum Verarbeiten der Nachrichten von Event Hubs und gilt für Endpunkte, die mit IoT Hub-Event Hubs kompatibel sind.

> [!NOTE]
> Der mit Event Hubs kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQP-Protokoll.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben, ein Maven-Projekt namens **read-d2c-messages**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie an der Eingabeaufforderung zum Ordner „read-d2c-messages“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „read-d2c-messages“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „eventhubs-client“ in Ihrer App zum Lesen vom mit Event Hub kompatiblen Endpunkt verwenden:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „read-d2c-messages\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Fügen Sie der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubkey}**, **{youreventhubcompatibleendpoint}** und **{youreventhubcompatiblename}** durch die Werte, die Sie zuvor notiert haben:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Fügen Sie der **App**-Klasse die folgende **receiveMessages**-Methode hinzu. Mit dieser Methode wird eine **EventHubClient**-Instanz erstellt, um eine Verbindung mit dem Event Hub-kompatiblen Endpunkt herzustellen. Anschließend wird asynchron eine **PartitionReceiver**-Instanz zum Lesen von einer Event Hub-Partition erstellt. Sie wird in einer Dauerschleife ausgeführt und gibt die Nachrichtendetails aus, bis die App beendet wird.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some evenst");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Bei dieser Methode wird beim Erstellen des Receiver-Elements ein Filter verwendet, damit das Receiver-Element nur Nachrichten liest, die nach Beginn der Ausführung des Receiver-Elements an IoT Hub gesendet werden. Dieses Verfahren ist in einer Testumgebung hilfreich, damit Sie die aktuelle Gruppe von Nachrichten sehen. In einer Produktionsumgebung sollte mit Ihrem Code sichergestellt werden, dass alle Nachrichten verarbeitet werden. Weitere Informationen hierzu finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

9. Ändern Sie die Signatur der **main** -Methode, um die Ausnahme wie folgt einzufügen:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Fügen Sie der **main**-Methode in der **App**-Klasse den folgenden Code hinzu. Dieser Code erstellt die zwei **EventHubClient**- und **PartitionReceiver**-Instanzen und ermöglicht Ihnen nach der Verarbeitung von Nachrichten das Schließen der App:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > In diesem Code wird davon ausgegangen, dass Sie Ihren IoT Hub auf der Ebene F1 (Free) erstellt haben. Ein kostenloser IoT Hub verfügt über zwei Partitionen mit den Bezeichnungen „0“ und „1“.

11. Speichern und schließen Sie die Datei „App.java“.

12. Führen Sie zum Erstellen der App **read-d2c-messages** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „read-d2c-messages“ aus:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Erstellen einer Geräte-App
In diesem Abschnitt erstellen Sie eine Java-Konsolenanwendung, die ein Gerät simuliert, das D2C-Nachrichten (Device to Cloud, Gerät zu Cloud) an einen IoT Hub sendet.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben, ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „simulated-device“, und fügen Sie dem Knoten **dependencies** die folgenden Abhängigkeiten hinzu. Mit dieser Abhängigkeit können Sie das Paket „iothub-java-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub und Serialisieren von Java-Objekten nach JSON verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-device-client** mithilfe der [Maven-Suche][lnk-maven-device-search].

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „simulated-device\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubname}** durch den Namen Ihres IoT Hub und **{yourdevicekey}** durch den Geräteschlüsselwert, den Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Diese Beispiel-App verwendet beim Instanziieren eines **DeviceClient**-Objekts die **protocol**-Variable. Sie können das MQTT-, AMQP- oder HTTP-Protokoll verwenden, um mit IoT Hub zu kommunizieren.

8. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **TelemetryDataPoint**-Klasse hinzu, um die Telemetriedaten festzulegen, die Ihr Gerät an den IoT Hub senden soll:

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **EventCallback**-Klasse hinzu, um den Status der Bestätigung anzuzeigen, die der IoT Hub bei der Verarbeitung einer Nachricht von der Geräte-App zurückgibt. Diese Methode benachrichtigt zudem den Hauptthread der App, wenn die Nachricht verarbeitet wurde:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **MessageSender**-Klasse hinzu. Die **run** -Methode in dieser Klasse generiert Beispieltelemetriedaten zum Senden an den IoT Hub und wartet auf eine Bestätigung, bevor die nächste Nachricht gesendet wird:

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
            System.out.println("Sending: " + msgStr);
    
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
        } catch (InterruptedException e) {
          System.out.println("Finished.");
        }
      }
    }
    ```

    Diese Methode sendet eine Sekunde, nachdem der IoT Hub die vorherige Nachricht bestätigt, eine neue D2C-Nachricht. Die Nachricht enthält ein JSON-serialisiertes Objekt mit der Geräte-ID sowie zufällig generierten Zahlen, um einen Temperatur- und Feuchtigkeitssensor zu simulieren.

11. Ersetzen Sie die **main** -Methode durch den folgenden Code, der einen Thread zum Senden von D2C-Nachrichten an Ihren IoT Hub erstellt:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Speichern und schließen Sie die Datei „App.java“.

13. Führen Sie zum Erstellen der App **simulated-device** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „simulated-device“ aus:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Transient Fault Handling (Behandeln vorübergehender Fehler)][lnk-transient-faults] beschrieben.

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können die Apps nun ausführen.

1. Führen Sie an einer Befehlszeile im Ordner „read-d2c“ den folgenden Befehl aus, um mit der Überwachung der ersten Partition Ihres IoT-Hubs zu beginnen:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java-basierte IoT Hub-Dienst-App zum Überwachen von D2C-Nachrichten][7]

2. Führen Sie an einer Befehlszeile im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an Ihren IoT-Hub zu beginnen:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java-basierte IoT Hub-Geräte-App zum Senden von D2C-Nachrichten][8]

3. Über die Kachel **Nutzung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den IoT-Hub gesendeten Nachrichten angezeigt:

    ![Azure-Portal-Kachel „Nutzung“ mit der Anzahl von Nachrichten, die an IoT Hub gesendet wurden][43]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die Geräte-App für das Senden von D2C-Nachrichten an den IoT Hub zu aktivieren. Sie haben außerdem eine App erstellt, mit der die vom IoT-Hub empfangenen Nachrichten angezeigt werden.

Informationen zu den weiteren ersten Schritten mit IoT Hub und zum Kennenlernen anderer IoT-Szenarien finden Sie in den folgenden Artikeln:

* [Verbinden Ihres Geräts][lnk-connect-device]
* [Erste Schritte mit der Geräteverwaltung][lnk-device-management]
* [Erste Schritte mit Azure IoT Edge][lnk-iot-edge]

Informationen dazu, wie Sie Ihre IoT-Lösung erweitern und Ihre D2C-Nachrichten mit Skalierbarkeit verarbeiten, finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22

