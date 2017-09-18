---
title: "Erste Schritte mit der Azure IoT Hub-Gerätezwillingen (Java) | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie mithilfe von Azure IoT Hub-Gerätezwillingen Tags hinzufügen und eine IoT Hub-Abfrage verwenden. Sie verwenden das Azure IoT-Geräte-SDK für Java, um eine Geräte-App zu implementieren, und das Azure IoT-Dienst-SDK für Java, um eine Dienst-App zu implementieren, die die Tags hinzufügt und die IoT Hub-Abfrage ausführt."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/04/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 9b7316a5bffbd689bdb26e9524129ceed06606d5
ms.openlocfilehash: 6d306d4742a53789d8e69c80d7fbdfc4e1ade4bf
ms.contentlocale: de-de
ms.lasthandoff: 09/08/2017

---
# <a name="get-started-with-device-twins-java"></a>Erste Schritte mit Gerätezwillingen (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

In diesem Tutorial erstellen Sie zwei Java-Konsolen-Apps:

* **add-tags-query** ist eine Java-Back-End-App, die Tags hinzufügt und Gerätezwillinge abfragt.
* **simulated-device**, eine Java-Geräte-App, die eine Verbindung mit Ihrem IoT Hub herstellt und seine Konnektivitätsbedingung mit einer gemeldeten Eigenschaft meldet.

> [!NOTE]
> Im Artikel [Azure IoT SDKs](iot-hub-devguide-sdks.md) finden Sie Informationen zu den verschiedenen Azure IoT SDKs, mit denen Sie sowohl Geräte- als auch Back-End-Apps erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

* Das neueste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/) erstellen.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Wenn Sie die Geräteidentität programmgesteuert erstellen möchten, lesen Sie den entsprechenden Abschnitt im Artikel [Herstellen einer Verbindung zwischen dem Gerät und Ihrer IoT Hub-Instanz mit Java](iot-hub-java-java-getstarted.md#create-a-device-identity).

## <a name="create-the-service-app"></a>Erstellen der Dienst-App

In diesem Abschnitt erstellen Sie eine Java-App, die dem Gerätezwilling in IoT Hub, der **myDeviceId** zugeordnet ist, Standortmetadaten als Tag hinzufügt. Die App fragt IoT Hub zuerst nach Geräten in den USA ab, und dann nach Geräten, die eine Mobilfunknetzverbindung melden.

1. Erstellen Sie auf dem Entwicklungscomputer einen leeren Ordner mit dem Namen `iot-java-twin-getstarted`.

1. Erstellen Sie im Ordner `iot-java-twin-getstarted` ein Maven-Projekt namens **add-tags-query**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum `add-tags-query`-Ordner.

1. Öffnen Sie mit einem Text-Editor die Datei `pom.xml` im Ordner `add-tags-query`, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket **iot-service-client** in Ihrer App zum Kommunizieren mit Ihrem IoT Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Fügen Sie den Knoten **build** hinter dem Knoten **dependencies** hinzu. Diese Konfiguration weist Maven an, Java 1.8 zu verwenden, um die App zu erstellen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Speichern und schließen Sie die `pom.xml`-Datei.

1. Öffnen Sie in einem Text-Editor die Datei `add-tags-query\src\main\java\com\mycompany\app\App.java`.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie `{youriothubconnectionstring}` durch die IoT Hub-Verbindungszeichenfolge, die Sie im Abschnitt *Erstellen einer IoT Hub-Instanz* notiert haben:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Aktualisieren Sie die Signatur der **main**-Methode und schließen Sie die folgende `throws`-Klausel ein:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Fügen Sie der **main**-Methode zum Erstellen der Objekte **DeviceTwin** und **DeviceTwinDevice** den folgenden Code hinzu. Das **DeviceTwin**-Objekt führt die Kommunikation mit Ihrem IoT Hub durch. Das **DeviceTwinDevice**-Objekt stellt den Gerätezwilling mit seinen Eigenschaften und Tags dar:

    ```java
    // Get the DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Fügen Sie den folgenden `try/catch`-Block der **main**-Methode hinzu:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. Fügen Sie zum Aktualisieren der Gerätezwillingstags **region** und **plant** in Ihrem Gerätezwilling folgenden Code im `try`-Block hinzu:

    ```java
    // Get the device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from the existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create the tags and attach them to the DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update the device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve the device twin with the tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. Um die Gerätezwillinge in IoT Hub abzufragen, fügen Sie den folgenden Code dem `try`-Block nach dem Code hinzu, den Sie im vorherigen Schritt hinzugefügt haben. Der Code führt zwei Abfragen aus. Jede Abfrage gibt maximal 100 Geräte zurück:

    ```java
    // Query the device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct the query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run the query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct the query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run the query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Speichern und schließen Sie die `add-tags-query\src\main\java\com\mycompany\app\App.java`-Datei.

1. Erstellen Sie die App **add-tags-query**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum `add-tags-query`-Ordner, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Erstellen einer Geräte-App

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die einen gemeldeten Eigenschaftswert festlegt, der an IoT Hub gesendet wird.

1. Erstellen Sie im Ordner `iot-java-twin-getstarted` ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum `simulated-device`-Ordner.

1. Öffnen Sie mit einem Text-Editor die Datei `pom.xml` im Ordner `simulated-device`, und fügen Sie dem Knoten **dependencies** die folgenden Abhängigkeiten hinzu. Mit dieser Abhängigkeit können Sie das Paket **iot-device-client** in Ihrer App zum Kommunizieren mit Ihrem IoT Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-device-client** mithilfe der [Maven-Suche](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Fügen Sie den Knoten **build** hinter dem Knoten **dependencies** hinzu. Diese Konfiguration weist Maven an, Java 1.8 zu verwenden, um die App zu erstellen:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Speichern und schließen Sie die `pom.xml`-Datei.

1. Öffnen Sie in einem Text-Editor die Datei `simulated-device\src\main\java\com\mycompany\app\App.java`.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie `{youriothubname}` durch den Namen Ihres IoT Hubs und `{yourdevicekey}` durch den Geräteschlüsselwert, den Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    Diese Beispiel-App verwendet beim Instanziieren eines **DeviceClient**-Objekts die **protocol**-Variable. 

1. Fügen Sie den folgenden Code der **main**-Methode hinzu:
    * Erstellen Sie einen Geräteclient zur Kommunikation mit IoT Hub.
    * Erstellen Sie ein **Device**-Objekt, das die Gerätezwillingseigenschaften speichert.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object to store the device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed to " + propertyValue);
      }
    };
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um eine gemeldete **connectivityType**-Eigenschaft zu erstellen und an den IoT Hub zu senden:

    ```java
    try {
      // Open the DeviceClient and start the device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it to your IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Fügen Sie den folgenden Code am Ende der **main**-Methode hinzu. Das Warten auf die **EINGABETASTE** lässt IoT Hub genügend Zeit, um den Status der Gerätezwillingsvorgänge zu melden:

    ```java
    System.out.println("Press any key to exit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Speichern und schließen Sie die `simulated-device\src\main\java\com\mycompany\app\App.java`-Datei.

1. Erstellen Sie die App **simulated-device**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum `simulated-device`-Ordner, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können nun die Konsolen-Apps ausführen.

1. Führen Sie an der Eingabeaufforderung im `add-tags-query`-Ordner den folgenden Befehl zum Ausführen der **add-tags-query**-Dienst-App aus:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub-Dienst-App zum Aktualisieren von Tagwerten und Ausführen von Geräteabfragen](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Sie sehen die Tags **plant** und **region**, die dem Gerätezwilling hinzugefügt wurden. Die erste Abfrage gibt das Gerät zurück, die zweite jedoch nicht.

1. Führen Sie an der Eingabeaufforderung im `simulated-device`-Ordner den folgenden Befehl zum Hinzufügen der gemeldeten **connectivityType**-Eigenschaft an den Gerätezwilling aus:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Der Geräteclient fügt die gemeldete **connectivityType**-Eigenschaft hinzu](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. Führen Sie an der Eingabeaufforderung im `add-tags-query`-Ordner den folgenden Befehl zum zweiten Ausführen der **add-tags-query**-Dienst-App aus:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub-Dienst-App zum Aktualisieren von Tagwerten und Ausführen von Geräteabfragen](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Nachdem Ihr Gerät nun die **connectivityType**-Eigenschaft an IoT Hub gesendet hat, gibt die zweite Abfrage das Gerät zurück.

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben Gerätemetadaten aus einer Back-End-App als Tags hinzugefügt und eine Geräte-App zum Melden von Gerätekonnektivitätsinformationen im Gerätezwilling geschrieben. Sie haben zudem erfahren, wie die Gerätezwillingsinformationen mithilfe der SQL-ähnlichen IoT Hub-Abfragesprache abgefragt werden können.

Weitere Informationen finden Sie in den folgenden Ressourcen:

* Senden von Telemetriedaten von Geräten im Tutorial [Erste Schritte mit IoT Hub](iot-hub-java-java-getstarted.md).
* Informationen zur interaktiven Steuerung von Geräten (z.B. Einschalten eines Lüfters über eine benutzergesteuerte App) im Tutorial [Verwenden von direkten Methoden](iot-hub-java-java-direct-methods.md).

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

