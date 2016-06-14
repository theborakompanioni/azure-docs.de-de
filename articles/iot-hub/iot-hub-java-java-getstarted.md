<properties
	pageTitle="Erste Schritte mit Azure IoT Hub für Java | Microsoft Azure"
	description="Tutorial für den Einstieg in Azure IoT Hub mit Java. Verwenden Sie Azure IoT Hub und Java mit den Microsoft Azure IoT SDKs, um eine IoT-Lösung zu implementieren."
	services="iot-hub"
	documentationCenter="java"
	authors="dominicbetts"
	manager="timlt"
	editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="java"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/06/2016"
     ms.author="dobett"/>

# Erste Schritte mit Azure IoT Hub für Java

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

Am Ende dieses Tutorials verfügen Sie über drei Java-Konsolenanwendungen:

* **create-device-identity**. Hiermit werden eine Geräteidentität und ein zugehöriger Sicherheitsschlüssel zum Verbinden mit Ihrem simulierten Gerät erstellt.
* **read-d2c-messages**. Hiermit wird die Telemetrie angezeigt, die Ihr simuliertes Gerät sendet.
* **simulated-device**. Hiermit wird mithilfe der zuvor erstellten Geräteidentität eine Verbindung mit Ihrem IoT Hub hergestellt und jede Sekunde unter Verwendung des AMQPS-Protokolls eine Telemetrienachricht gesendet.

> [AZURE.NOTE] Im Artikel [IoT Hub-SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

+ Java SE 8. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Java unter Windows oder Linux installieren.

+ Maven 3. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Maven unter Windows oder Linux installieren.

+ Ein aktives Azure-Konto. <br/>Wenn Sie noch kein Konto haben, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Einzelheiten finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Klicken Sie im letzten Schritt auf dem IoT Hub-Blatt auf **Einstellungen** und auf dem Blatt **Einstellungen** auf **Messaging**. Notieren Sie sich auf dem Blatt **Messaging** die Angaben für **Event Hub-kompatibler Name** und **Event Hub-kompatibler Endpunkt**. Sie benötigen diese Werte beim Erstellen der Anwendung **read-d2c-messages**.

![][6]

Sie haben nun Ihren IoT Hub erstellt und verfügen über den IoT Hub-Hostnamen, die IoT Hub-Verbindungszeichenfolge, den Event Hub-kompatiblen Namen und den Event Hub-kompatiblen Endpunkt. Diese Angaben benötigen Sie, um den Rest dieses Tutorials abschließen zu können.

## Erstellen einer Geräteidentität

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, mit der eine neue Geräteidentität in der Identitätsregistrierung Ihres IoT Hub erstellt wird. Ein Gerät kann nur eine Verbindung mit dem IoT Hub herstellen, wenn in der Geräteidentitätsregistrierung ein Eintrag für dieses Gerät vorhanden ist. Weitere Informationen finden Sie im Abschnitt **Geräteidentitätsregistrierung** des [Entwicklungsleitfadens für IoT Hub][lnk-devguide-identity]. Beim Ausführen dieser Konsolenanwendung werden eine eindeutige Geräte-ID und ein Schlüssel erstellt, mit denen sich das Gerät beim Senden von D2C-Nachrichten (Device-to-Cloud, Gerät-an-Cloud) beim IoT Hub identifizieren kann.

1. Erstellen Sie einen neuen leeren Ordner mit dem Namen „iot-java-get-started“. Erstellen Sie im Ordner „iot-java-get-started“ ein neues Maven-Projekt namens **create-device-identity**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt.

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Eingabeaufforderung zum neuen Ordner „create-device-identity“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „create-device-identity“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Sie können dann das Paket „iothub-service-sdk“ in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-service-client</artifactId>
      <version>1.0.2</version>
    </dependency>
    ```
    
4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „create-device-identity\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import**-Anweisungen hinzu:

    ```
    import com.microsoft.azure.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.iot.service.sdk.Device;
    import com.microsoft.azure.iot.service.sdk.RegistryManager;

    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Fügen Sie die folgenden Klassenebenenvariablen der **App**-Klasse hinzu, und ersetzen Sie dabei **{yourhubname}** und **{yourhubkey}** durch die zuvor notierten Werte:

    ```
    private static final String connectionString = "HostName={yourhubname}.azure-devices.net;SharedAccessKeyName=iothubowner;SharedAccessKey={yourhubkey}";
    private static final String deviceId = "javadevice";
    
    ```
    
8. Ändern Sie die Signatur der **main**-Methode, um die folgenden Ausnahmen einzufügen:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```
    
9. Fügen Sie den folgenden Code als Textkörper der **main**-Methode hinzu. Mit diesem Code wird ein Gerät namens *javadevice* in Ihrer IoT Hub-Identitätsregistrierung erstellt, sofern es noch nicht vorhanden ist. Anschließend werden die Geräte-ID und der dazugehörige Schlüssel angezeigt, die Sie später benötigen:

    ```
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }
    System.out.println("Device id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Speichern und schließen Sie die Datei „App.java“.

11. Führen Sie zum Erstellen der Anwendung **create-device-identity** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „create-device-identity“ aus:

    ```
    mvn clean package -DskipTests
    ```

12. Führen Sie zum Ausführen der Anwendung **create-device-identity** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „create-device-identity“ aus:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Notieren Sie sich die Geräte-ID und den Geräteschlüssel. Sie benötigen diese Angaben später beim Erstellen einer Anwendung, für die eine Verbindung mit IoT Hub als Gerät hergestellt wird.

> [AZURE.NOTE] Die Identitätsregistrierung im IoT Hub speichert nur Geräteidentitäten, um einen sicheren Zugriff auf den Hub zu ermöglichen. Sie speichert Geräte-IDs und Schlüssel, die als Sicherheitsanmeldeinformationen verwendet werden, sowie ein Aktiviert/Deaktiviert-Kennzeichen, mit dem Sie den Zugriff für ein einzelnes Gerät deaktivieren können. Wenn Ihre Anwendung das Speichern weiterer gerätespezifischer Metadaten erfordert, sollte dafür ein anwendungsspezifischer Speicher verwendet werden. Weitere Informationen finden Sie im [IoT Hub-Entwicklerhandbuch][lnk-devguide-identity].

## Empfangen von Gerät-an-Cloud-Nachrichten

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die D2C-Nachrichten (Device-to-Cloud) aus dem IoT Hub liest. Ein IoT Hub macht einen [Event Hub][lnk-event-hubs-overview]-kompatiblen Endpunkt verfügbar, der Ihnen das Lesen von D2C-Nachrichten ermöglicht. Zur Vereinfachung wird in diesem Tutorial ein einfacher Reader erstellt, der für eine Bereitstellung mit hohem Durchsatz nicht geeignet ist. Im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial] wird gezeigt, wie Sie D2C-Nachrichten in größerem Umfang verarbeiten. Das Tutorial [Erste Schritte mit Event Hubs][lnk-eventhubs-tutorial] enthält weitere Informationen zum Verarbeiten der Nachrichten von Event Hubs und gilt für Endpunkte, die mit IoT Hub-Event Hubs kompatibel sind.

> [AZURE.NOTE] Der mit Event Hub kompatible Endpunkt zum Lesen von D2C-Nachrichten verwendet immer das AMQPS-Protokoll.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben, ein neues Maven-Projekt namens **read-d2c-messages**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt.

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Eingabeaufforderung zum neuen Ordner „read-d2c-messages“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „read-d2c-messages“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. So können Sie das Paket „eventhubs-client“ in Ihrer Anwendung zum Lesen vom Event Hub-kompatiblen Endpunkt verwenden:

    ```
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.7.1</version> 
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „read-d2c-messages\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import**-Anweisungen hinzu:

    ```
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;
    
    import java.io.IOException;
    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.Collection;
    import java.util.concurrent.ExecutionException;
    import java.util.function.*;
    import java.util.logging.*;
    ```

7. Fügen Sie der **App**-Klasse die folgenden Variablen auf Klassenebene hinzu: Ersetzen Sie **{youriothubkey}**, **{youreventhubcompatiblenamespace}** und **{youreventhubcompatiblename}** durch die Werte, die Sie zuvor notiert haben. Der Wert des Platzhalters **{youreventhubcompatiblenamespace}** stammt vom **Event Hub-kompatiblen Endpunkt** und hat das folgende Format: **xyznamespace** (Sie müssen also das Präfix **sb://** und das Suffix **.servicebus.windows.net** vom Event Hub-kompatiblen Endpunktwert im Portal entfernen):

    ```
    private static String namespaceName = "{youreventhubcompatiblenamespace}";
    private static String eventHubName = "{youreventhubcompatiblename}";
    private static String sasKeyName = "iothubowner";
    private static String sasKey = "{youriothubkey}";
    private static long now = System.currentTimeMillis();
    ```

8. Fügen Sie der **App**-Klasse die folgende **receiveMessages**-Methode hinzu. Mit dieser Methode wird eine **EventHubClient**-Instanz erstellt, um eine Verbindung mit dem Event Hub-kompatiblen Endpunkt herzustellen. Anschließend wird asynchron eine **PartitionReceiver**-Instanz zum Lesen von einer Event Hub-Partition erstellt. Sie wird in einer Dauerschleife ausgeführt und gibt die Nachrichtendetails aus, bis die Anwendung beendet wird.

    ```
    private static EventHubClient receiveMessages(final String partitionId)
    {
      EventHubClient client = null;
      try {
        ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
        client = EventHubClient.createFromConnectionString(connStr.toString()).get();
      }
      catch(Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
        System.exit(1);
      }
      try {
        client.createReceiver( 
          EventHubClient.DEFAULT_CONSUMER_GROUP_NAME,  
          partitionId,  
          Instant.now()).thenAccept(new Consumer<PartitionReceiver>()
        {
          public void accept(PartitionReceiver receiver)
          {
            System.out.println("** Created receiver on partition " + partitionId);
            try {
              while (true) {
                Iterable<EventData> receivedEvents = receiver.receive().get();
                int batchSize = 0;
                if (receivedEvents != null)
                {
                  for(EventData receivedEvent: receivedEvents)
                  {
                    System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s", 
                      receivedEvent.getSystemProperties().getOffset(), 
                      receivedEvent.getSystemProperties().getSequenceNumber(), 
                      receivedEvent.getSystemProperties().getEnqueuedTime()));
                    System.out.println(String.format("| Device ID: %s", receivedEvent.getProperties().get("iothub-connection-device-id")));
                    System.out.println(String.format("| Message Payload: %s", new String(receivedEvent.getBody(),
                      Charset.defaultCharset())));
                    batchSize++;
                  }
                }
                System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId,batchSize));
              }
            }
            catch (Exception e)
            {
              System.out.println("Failed to receive messages: " + e.getMessage());
            }
          }
        });
      }
      catch (Exception e)
      {
        System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

    > [AZURE.NOTE] Bei dieser Methode wird beim Erstellen des Receiver-Elements ein Filter verwendet, damit das Receiver-Element nur Nachrichten liest, die nach Beginn der Ausführung des Receiver-Elements an IoT Hub gesendet werden. Dies ist in einer Testumgebung nützlich, da Sie den aktuellen Satz von Nachrichten anzeigen können. In einer Produktionsumgebung sollte Ihr Code hingegen sicherstellen, dass alle Nachrichten verarbeitet werden. Weitere Informationen hierzu finden Sie im Tutorial [Verarbeiten von D2C-Nachrichten mit IoT Hub][lnk-process-d2c-tutorial].

9. Ändern Sie die Signatur der **main**-Methode, um die folgende Ausnahme einzufügen:

    ```
    public static void main( String[] args ) throws IOException
    ```

10. Fügen Sie der **main**-Methode in der **App**-Klasse den folgenden Code hinzu. Dieser Code erstellt die zwei **EventHubClient**- und **PartitionReceiver**-Instanzen und ermöglicht Ihnen nach der Verarbeitung von Nachrichten das Schließen der Anwendung:

    ```
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try
    {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    }
    catch (ServiceBusException sbe)
    {
      System.exit(1);
    }
    ```

    > [AZURE.NOTE] In diesem Code wird davon ausgegangen, dass Sie Ihren IoT Hub auf der Ebene F1 (Free) erstellt haben. Ein kostenloser IoT Hub verfügt über zwei Partitionen mit den Bezeichnungen „0“ und „1“.

11. Speichern und schließen Sie die Datei „App.java“.

12. Führen Sie zum Erstellen der Anwendung **read-d2c-messages** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „read-d2c-messages“ aus:

    ```
    mvn clean package -DskipTests
    ```

## Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Java-Konsolenanwendung, die ein Gerät simuliert, das D2C-Nachrichten (device to Cloud, Gerät zu Cloud) an einen IoT Hub sendet.

1. Erstellen Sie im Ordner „iot-java-get-started“, den Sie im Abschnitt *Erstellen einer Geräteidentität* erstellt haben, ein neues Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigieren Sie in der Eingabeaufforderung zum neuen Ordner „simulated-device“.

3. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „simulated-device“, und fügen Sie dem Knoten **dependencies** die folgenden Abhängigkeiten hinzu. Dadurch können Sie das Paket „iothub-java-client“ zum Kommunizieren mit Ihrem IoT Hub und Serialisieren von Java-Objekten nach JSON in Ihrer Anwendung verwenden:

    ```
    <dependency>
      <groupId>com.microsoft.azure.iothub-java-client</groupId>
      <artifactId>iothub-java-device-client</artifactId>
      <version>1.0.2</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

4. Speichern und schließen Sie die Datei „pom.xml“.

5. Öffnen Sie die Datei „simulated-device\\src\\main\\java\\com\\mycompany\\app\\App.java“ mit einem Text-Editor.

6. Fügen Sie der Datei die folgenden **import**-Anweisungen hinzu:

    ```
    import com.microsoft.azure.iothub.DeviceClient;
    import com.microsoft.azure.iothub.IotHubClientProtocol;
    import com.microsoft.azure.iothub.Message;
    import com.microsoft.azure.iothub.IotHubStatusCode;
    import com.microsoft.azure.iothub.IotHubEventCallback;
    import com.microsoft.azure.iothub.IotHubMessageResult;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.security.InvalidKeyException;
    import java.util.Random;
    import javax.naming.SizeLimitExceededException;
    import com.google.gson.Gson;
    ```

7. Fügen Sie der **App**-Klasse die folgenden Variablen auf Klassenebene hinzu, und ersetzen Sie **{youriothubname}** durch den Namen Ihres IoT Hubs und **{yourdeviceid}** (Geräte-ID) und **{yourdevicekey}** (Geräteschlüssel) mit den Gerätewerten, die Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.AMQPS;
    private static boolean stopThread = false;
    ```

    Diese Beispielanwendung verwendet beim Instanziieren eines **DeviceClient**-Objekts die **protocol**-Variable. Sie können das HTTPS- oder AMQPS-Protokoll verwenden, um mit IoT Hub zu kommunizieren.

8. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **TelemetryDataPoint**-Klasse hinzu, um die Telemetriedaten festzulegen, die Ihr Gerät an den IoT Hub senden soll:

    ```
    private static class TelemetryDataPoint {
      public String deviceId;
      public double windSpeed;
        
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```

9. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **EventCallback**-Klasse hinzu, um den Status der Bestätigung anzuzeigen, die der IoT Hub bei der Verarbeitung einer Nachricht vom simulierten Gerät zurückgibt. Diese Methode benachrichtigt zudem den Hauptthread der Anwendung, wenn die Nachricht verarbeitet wurde:

    ```
    private static class EventCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status " + status.name());
      
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Fügen Sie innerhalb der **App**-Klasse die folgende geschachtelte **MessageSender**-Klasse hinzu. Die **run**-Methode in dieser Klasse generiert Beispieltelemetriedaten zum Senden an den IoT Hub und wartet auf eine Bestätigung, bevor die nächste Nachricht gesendet wird:

    ```
    private static class MessageSender implements Runnable {
      public volatile boolean stopThread = false;

      public void run()  {
        try {
          double avgWindSpeed = 10; // m/s
          Random rand = new Random();
          DeviceClient client;
          client = new DeviceClient(connString, protocol);
          client.open();
        
          while (!stopThread) {
            double currentWindSpeed = avgWindSpeed + rand.nextDouble() * 4 - 2;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = "myFirstDevice";
            telemetryDataPoint.windSpeed = currentWindSpeed;
      
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            System.out.println(msgStr);
        
            Object lockobj = new Object();
            EventCallback callback = new EventCallback();
            client.sendEventAsync(msg, callback, lockobj);
    
            synchronized (lockobj) {
              lockobj.wait();
            }
            Thread.sleep(1000);
          }
          client.close();
        } catch (Exception e) {
          e.printStackTrace();
        }
      }
    }
    ```

    Diese Methode sendet eine Sekunde, nachdem der IoT Hub die vorherige Nachricht bestätigt, eine neue D2C-Nachricht. Die Nachricht enthält ein JSON-serialisiertes Objekt mit der Geräte-ID sowie eine zufällig generierte Zahl, um einen Windgeschwindigkeitssensor zu simulieren.

11. Ersetzen Sie die **main**-Methode durch den folgenden Code, der einen Thread zum Senden von D2C-Nachrichten an Ihren IoT Hub erstellt:

    ```
    public static void main( String[] args ) throws IOException, URISyntaxException {
    
      MessageSender ms0 = new MessageSender();
      Thread t0 = new Thread(ms0);
      t0.start(); 
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      ms0.stopThread = true;
    }
    ```

12. Speichern und schließen Sie die Datei „App.java“.

13. Führen Sie zum Erstellen der Anwendung **simulated-device** mit Maven den folgenden Befehl an der Eingabeaufforderung im Ordner „simulated-device“ aus:

    ```
    mvn clean package -DskipTests
    ```

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z. B. einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler][lnk-transient-faults] beschrieben.

## Ausführen der Anwendungen

Sie können nun die Anwendungen ausführen.

1. Führen Sie an einer Befehlszeile im Ordner „read-d2c“ den folgenden Befehl aus, um mit der Überwachung der ersten Partition Ihres IoT Hubs zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="0"
    ```

    ![][7]

1. Führen Sie an einer Befehlszeile im Ordner „read-d2c“ den folgenden Befehl aus, um mit der Überwachung der zweiten Partition Ihres IoT Hubs zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"  -Dexec.args="1"
    ```

    ![][7]

2. Führen Sie an einer Befehlszeile im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Senden von Telemetriedaten an Ihren IoT Hub zu beginnen:

    ```
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![][8]

3. Über die Kachel **Verwendung** im [Azure-Portal][lnk-portal] wird die Anzahl der an den Hub gesendeten Nachrichten angezeigt:

    ![][43]

## Nächste Schritte

In diesem Tutorial haben Sie im Portal einen neuen IoT Hub konfiguriert und anschließend in der Identitätsregistrierung des Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um der simulierten Geräte-App das Senden von D2C-Nachrichten an den Hub zu ermöglichen, und Sie haben eine App erstellt, die die vom Hub empfangenen Nachrichten anzeigt. In den folgenden Tutorials werden weitere Features und Szenarien für IoT Hubs vorgestellt:

- Unter [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d-tutorial] wird erläutert, wie Sie Nachrichten an Geräte senden und das von IoT Hub generierte Feedback zur Übermittlung verarbeiten.
- In [Verarbeiten von D2C-Nachrichten][lnk-process-d2c-tutorial] wird erläutert, wie Sie zuverlässig Telemetriedaten und interaktive Nachrichten von Geräten verarbeiten können.
- In [Hochladen von Dateien von Geräten][lnk-upload-tutorial] wird beschrieben, wie mithilfe von C2D-Nachrichten Dateiuploads von Geräten erleichtert werden.

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide.md#identityregistry
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/java/device/doc/devbox_setup.md
[lnk-c2d-tutorial]: iot-hub-csharp-csharp-c2d.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-upload-tutorial]: iot-hub-csharp-csharp-file-upload.md

[lnk-hub-sdks]: iot-hub-sdks-summary.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

<!---HONumber=AcomDC_0608_2016-->