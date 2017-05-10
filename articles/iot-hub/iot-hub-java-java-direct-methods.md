---
title: Verwenden direkter Azure IoT Hub-Methoden (.NET/Node) | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie direkte Azure IoT Hub-Methoden verwenden. Sie verwenden das Azure IoT-Geräte-SDK für Node.js, um eine simulierte Geräte-App zu implementieren, die eine direkte Methode enthält, und das Azure IoT-Dienst-SDK, um eine Dienst-App zu implementieren, die die direkte Methode aufruft."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 64bd7f356673b385581c8060b17cba721d0cf8e3
ms.openlocfilehash: bb58d485a7ae805489b468cfffd72654914f63f1
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="use-direct-methods-java"></a>Verwenden direkter Methoden (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

In diesem Tutorial erstellen Sie zwei Java-Konsolen-Apps:

* **invoke-direct-method**: Eine Java-Back-End-App, die eine Methode in der simulierten Geräte-App aufruft und die Antwort anzeigt.
* **simulated-device**: Eine Java-App, die ein Gerät simuliert, das sich mit Ihrem IoT Hub mit der von Ihnen erstellten Geräteidentität verbindet und auf die direkte Methode reagiert, die im Back-End aufgerufen wird.

> [!NOTE]
> Im Artikel [IoT Hub SDKs][lnk-hub-sdks] finden Sie Informationen über die verschiedenen Azure IoT SDKs, mit denen Sie sowohl Anwendungen für Geräte als auch das zugehörige Lösungs-Back-End erstellen können.

Für dieses Tutorial benötigen Sie Folgendes:

* Java SE 8. <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial Java unter Windows oder Linux installieren.
* Maven 3.  <br/> Unter [Vorbereiten Ihrer Entwicklungsumgebung][lnk-dev-setup] wird beschrieben, wie Sie für dieses Tutorial [Maven][lnk-maven] unter Windows oder Linux installieren.
* [Node.js-Version 0.10.0 oder höher](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Erstellen einer simulierten Geräte-App

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die auf eine vom Lösungs-Back-End aufgerufene Methode antwortet.

1. Erstellen Sie einen leeren Ordner mit dem Namen „iot-java-direct_method“. Erstellen Sie im Ordner „iot-java-direct-method“ ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

1. Erstellen Sie im Ordner „iot-java-direct-method“ ein Maven-Projekt namens **simulated-device**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“.

1. Öffnen Sie mit einem Text-Editor die Datei „pom.xml“ im Ordner „simulated-device“, und fügen Sie dem Knoten **dependencies** die folgenden Abhängigkeiten hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-device-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.1.24</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-device-client** mithilfe der [Maven-Suche][lnk-maven-device-search].

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

1. Speichern und schließen Sie die Datei „pom.xml“.

1. Öffnen Sie die Datei „simulated-device\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubname}** durch den Namen Ihres IoT Hub und **{yourdevicekey}** durch den Geräteschlüsselwert, den Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Diese Beispiel-App verwendet beim Instanziieren eines **DeviceClient**-Objekts die **protocol**-Variable. Derzeit ist für das Verwenden direkter Methoden das Protokoll MQTT erforderlich.

1. Fügen Sie die folgende geschachtelte Klasse der **App**-Klasse hinzu, um einen Statuscode an Ihren IoT Hub zurückzugeben:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. Fügen Sie die folgende geschachtelte Klasse der **App**-Klasse hinzu, um die Aufrufe direkter Methoden aus dem Lösungs-Back-End zu verarbeiten:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Fügen Sie der **App**-Klasse eine **main**-Methode hinzu, um einen **DeviceClient** zu erstellen, und lauschen Sie auf Aufrufe direkter Methoden:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Speichern und schließen Sie die Datei „simulated-device\src\main\java\com\mycompany\app\App.java“.

1. Erstellen Sie die App **simulated-device**, und korrigieren Sie etwaige Fehler. Navigieren Sie an der Eingabeaufforderung zum Ordner „simulated-device“, und führen Sie den folgenden Befehl aus:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Aufrufen einer direkten Methode auf einem Gerät

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die eine direkte Methode in der simulierten Geräte-App aufruft und dann die Antwort anzeigt. Diese Konsolen-App stellt eine Verbindung mit Ihrem IoT Hub her, um die direkte Methode aufzurufen.

1. Erstellen Sie im Ordner „iot-java-direct-method“ ein Maven-Projekt namens **invoke-direct-method**, indem Sie an der Eingabeaufforderung den folgenden Befehl ausführen. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigieren Sie an der Eingabeaufforderung zum Ordner „invoke-direct-method“.

1. Öffnen Sie mit einem Text-Editor im Ordner „invoke-direct-method“ die Datei „pom.xml“, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Mit dieser Abhängigkeit können Sie das Paket „iot-service-client“ in Ihrer App zum Kommunizieren mit Ihrem IoT-Hub verwenden:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.2.17</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche][lnk-maven-service-search].

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

1. Speichern und schließen Sie die Datei „pom.xml“.

1. Öffnen Sie die Datei „invoke-direct-method\src\main\java\com\mycompany\app\App.java“ mit einem Text-Editor.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Fügen Sie die folgenden Variablen auf Klassenebene der **App** -Klasse die folgende Variable auf Klassenebene hinzu. Ersetzen Sie **{youriothubname}** durch den Namen Ihres IoT Hubs und **{yourhubkey}** durch den Geräteschlüsselwert, den Sie im Abschnitt *Erstellen einer Geräteidentität* generiert haben:

    ```java
    public static final String iotHubConnectionString = "HostName={youriothubname}.azure-devices.net;SharedAccessKeyName=owner;SharedAccessKey={yourhubkey}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. Fügen Sie der **main**-Methode den folgenden Code hinzu, um die Methode auf dem simulierten Gerät aufzurufen:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

## <a name="run-the-apps"></a>Ausführen der Apps

Sie können nun die Konsolen-Apps ausführen.

Sie können die Apps nun ausführen.

1. Führen Sie an einer Eingabeaufforderung im Ordner „simulated-device“ den folgenden Befehl aus, um mit dem Lauschen auf Methodenaufrufe von Ihrer IoT Hub-Instanz zu beginnen:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Simulierte Java IoT Hub-Geräte-App zum Lauschen auf Aufrufe direkter Methoden][8]

1. Führen Sie an einer Eingabeaufforderung im Ordner „invoke-direct-method“ den folgenden Befehl aus, um von Ihrem IoT Hub aus eine Methode auf Ihrem simulierten Gerät aufzurufen:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT Hub-Dienst-App zum Aufrufen einer direkten Methode][7]

1. Das simulierte Gerät reagiert auf den Aufruf der direkten Methode:

    ![Simulierte Java IoT Hub-Geräte-App reagiert auf den Aufruf einer direkter Methode][9]

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie im Azure-Portal einen neuen IoT-Hub konfiguriert und anschließend in der Identitätsregistrierung des IoT-Hubs eine Geräteidentität erstellt. Sie haben diese Geräteidentität verwendet, um die simulierte Geräte-App für das Reagieren auf von der Cloud aufgerufene Methoden zu aktivieren. Sie haben außerdem eine App erstellt, die Methoden auf dem Gerät aufruft und die Antwort vom Gerät angezeigt.

Informationen zu anderen IoT-Szenarien finden Sie unter [Planen von Aufträgen auf mehreren Geräten][lnk-devguide-jobs].

Im Tutorial [Planen und Senden von Aufträgen][lnk-tutorial-jobs] erfahren Sie, wie Sie Ihre IoT-Lösung erweitern und Methodenaufrufe für mehrere Geräte planen.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22

