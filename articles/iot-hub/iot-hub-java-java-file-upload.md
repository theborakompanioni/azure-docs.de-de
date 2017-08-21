---
title: "Hochladen von Dateien von Geräten nach Azure IoT Hub mit Java | Microsoft-Dokumentation"
description: "Informationen zum Hochladen von Dateien von einem Gerät in die Cloud mithilfe des Azure IoT-Geräte-SDK für Java. Hochgeladene Dateien werden in einem Azure Storage-Blobcontainer gespeichert."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 49bc337dac9d3372da188afc3fa7dff8e907c905
ms.openlocfilehash: 910365d58a3d3ab672988459f6b9c2bd26a5a3a7
ms.contentlocale: de-de
ms.lasthandoff: 07/14/2017

---
# <a name="upload-files-from-your-device-to-the-cloud-with-iot-hub"></a>Hochladen von Dateien von Ihrem Gerät in die Cloud mit IoT Hub

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Dieses Tutorial baut auf dem Code im Tutorial [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub](iot-hub-java-java-c2d.md) auf, um zu zeigen, wie Sie die [IoT Hub-Funktionen zum Hochladen von Dateien](iot-hub-devguide-file-upload.md) zum Hochladen einer Datei in [Azure Blob Storage](../storage/index.md) nutzen. Das Tutorial veranschaulicht folgende Vorgehensweisen:

- Sicheres Bereitstellen eines Geräts mit einem Azure-Blob-URI für das Hochladen einer Datei.
- Verwenden der IoT Hub-Dateihochlade-Benachrichtigungen zum Auslösen der Dateiverarbeitung in Ihrem App-Back-End.

Die Tutorials [Erste Schritte mit IoT Hub](iot-hub-java-java-getstarted.md) und [Senden von C2D-Nachrichten mit IoT Hub](iot-hub-java-java-c2d.md) veranschaulichen die grundlegenden Gerät-zu-Cloud- und Cloud-zu-Gerät-Messagingfunktionen von IoT Hub. Im Tutorial [Verarbeiten von D2C-Nachrichten](iot-hub-java-java-process-d2c.md) wird eine Möglichkeit für das zuverlässige Speichern von D2C-Nachrichten in Azure-Blobspeicher beschrieben. In einigen Szenarien können Sie allerdings nicht einfach die Daten, die Ihre Geräte senden, den relativ kleinen D2C-Nachrichten zuordnen, die IoT Hub akzeptiert. Beispiel:

* Große Dateien, die Bilder enthalten
* Videos
* Vibrationsdaten, die mit hoher Häufigkeit als Stichproben erfasst werden
* Eine Form vorverarbeiteter Daten.

Diese Dateien werden normalerweise als Batch in der Cloud mit Tools wie [Azure Data Factory](../data-factory/index.md) oder dem [Hadoop](../hdinsight/index.md)-Stapel verarbeitet. Wenn Sie Dateien von einem Gerät hochladen müssen, können Sie weiterhin die Sicherheit und Zuverlässigkeit des IoT Hub nutzen.

Am Ende dieses Tutorials führen Sie zwei Java-Konsolen-Apps aus:

* **simulated-device**, eine geänderte Version der App, die im Tutorial [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub] erstellt wurde. Diese App lädt mithilfe eines SAS-URI, den Ihr IoT Hub bereitstellt, eine Datei in den Speicher hoch.
* **read-file-upload-notification**, die Benachrichtigungen zum Hochladen von Dateien von Ihrem IoT Hub empfängt.

> [!NOTE]
> IoT Hub bietet über Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, .NET und JavaScript). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit Azure IoT Hub.

Für dieses Lernprogramm benötigen Sie Folgendes:

* Das neueste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ein aktives Azure-Konto. (Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](http://azure.microsoft.com/pricing/free-trial/) erstellen.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Hochladen einer Datei von einer Geräte-App

In diesem Abschnitt ändern Sie die Geräte-App, die Sie in [Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub](iot-hub-java-java-c2d.md) erstellt haben, um eine Datei nach IoT Hub hochzuladen.

1. Kopieren Sie eine Bilddatei in den `simulated-device`-Ordner und benennen Sie sie in `myimage.png` um.

1. Öffnen Sie in einem Text-Editor die Datei `simulated-device\src\main\java\com\mycompany\app\App.java`.

1. Fügen Sie die Variablendeklaration der **App**-Klasse hinzu:

    ```java
    private static String fileName = "myimage.png";
    ```

1. Fügen Sie zum Verarbeiten der Rückrufmeldungen zum Dateihochladestatus die folgende geschachtelte Klasse der **App**-Klasse hinzu:

    ```java
    // Define a callback method to print status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to file upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. Um Bilder in IoT Hub hochzuladen, fügen Sie die folgende Methode zum Hochladen von Bildern in IoT Hub der **App**-Klasse hinzu:

    ```java
    // Use IoT Hub to upload a file asynchronously to Azure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. Ändern Sie die Methode **main** zum Aufrufen der Methode **uploadFile**, wie im folgenden Codeausschnitt dargestellt:

    ```java
    client.open();

    try
    {
      // Get the filename and start the upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. Verwenden Sie den folgenden Befehl zum Erstellen der **simulated-device**-App, und prüfen Sie, ob Fehler vorliegen:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Erhalten einer Benachrichtigung zum Dateiupload

In diesem Abschnitt erstellen Sie eine Java-Konsolen-App, die Uploadbenachrichtigungen von IoT Hub empfängt.

Sie müssen diesen Abschnitt mit der Verbindungszeichenfolge **iothubowner** für Ihren IoT Hub abschließen. Die Verbindungszeichenfolge finden Sie im [Azure-Portal](https://portal.azure.com/) auf dem Blatt **SAS-Richtlinie**.

1. Erstellen Sie mithilfe des folgenden Befehls über die Eingabeaufforderung ein Maven-Projekt namens **read-file-upload-notification**. Beachten Sie, dass es sich hierbei um einen einzelnen langen Befehl handelt:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Navigieren Sie an der Eingabeaufforderung zum neuen `read-file-upload-notification`-Ordner.

1. Öffnen Sie mit einem Text-Editor die Datei `pom.xml` im Ordner `read-file-upload-notification`, und fügen Sie dem Knoten **dependencies** die folgende Abhängigkeit hinzu. Durch Hinzufügen der Abhängigkeit können Sie in Ihrer Anwendung das Paket **iothub-java-service-client** verwenden, um mit Ihrem IoT Hub-Dienst zu kommunizieren:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Sie finden die aktuelle Version von **iot-service-client** mithilfe der [Maven-Suche](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Speichern und schließen Sie die `pom.xml`-Datei.

1. Öffnen Sie in einem Text-Editor die Datei `read-file-upload-notification\src\main\java\com\mycompany\app\App.java`.

1. Fügen Sie der Datei die folgenden **import** -Anweisungen hinzu:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Fügen Sie der **App** -Klasse die folgenden Klassenebenenvariablen hinzu:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. Um Informationen über das Hochladen der Datei an der Konsole auszugeben, fügen Sie die folgende geschachtelte Klasse der **App**-Klasse hinzu:

    ```java
    // Create a thread to receive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. Um den Thread zu starten, der auf Dateiuploadbenachrichtigungen lauscht, fügen Sie den folgenden Code in die **main**-Methode ein:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from the ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start the thread to receive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER to exit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. Speichern und schließen Sie die `read-file-upload-notification\src\main\java\com\mycompany\app\App.java`-Datei.

1. Verwenden Sie den folgenden Befehl zum Erstellen der **read-file-upload-notification**-App, und prüfen Sie, ob Fehler vorliegen:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a>Ausführen der Anwendungen

Sie können jetzt die Anwendung ausführen.

Führen Sie an der Eingabeaufforderung im `read-file-upload-notification`-Ordner folgenden Befehl aus:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Führen Sie an der Eingabeaufforderung im `simulated-device`-Ordner folgenden Befehl aus:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Der folgende Screenshot zeigt die Ausgabe der **simulated-device**-App an:

![Ausgabe der simulated-device-App](media/iot-hub-java-java-upload/simulated-device.png)

Der folgende Screenshot zeigt die Ausgabe der **read-file-upload-notification**-App:

![Ausgabe der read-file-upload-notification-App](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Sie können das Verwaltungsportal verwenden, um die hochgeladene Datei im Speichercontainer anzuzeigen, den Sie konfiguriert haben:

![Hochgeladene Datei](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die IoT Hub-Funktionen zum Hochladen von Dateien nutzen, um Dateiuploads zu vereinfachen. In den folgenden Artikeln werden weitere IoT Hub-Features und -Szenarien vorgestellt:

* [Programmgesteuertes Erstellen eines IoT Hubs][lnk-create-hub]
* [Einführung in das C SDK][lnk-c-sdk]
* [Azure IoT SDKs][lnk-sdks]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [Simulieren eines Geräts mit IoT Edge][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md



