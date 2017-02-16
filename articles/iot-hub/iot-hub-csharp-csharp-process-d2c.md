---
title: Verarbeiten von Azure IoT Hub-D2C-Nachrichten mit Routen (.NET) | Microsoft-Dokumentation
description: Wie IoT Hub-D2C-Nachrichten mithilfe von Routen verarbeitet werden, um Nachrichten an andere Back-End-Dienste zu senden.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 5177bac9-722f-47ef-8a14-b201142ba4bc
ms.service: iot-hub
ms.devlang: csharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/12/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: d2da282a849496772fe57b9429fe2a180f37328d
ms.openlocfilehash: 1ca480c4be2cca2c2558b13d2c3a5e5dea8b561e


---
# <a name="process-iot-hub-device-to-cloud-messages-using-routes-net"></a>Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen (.NET)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>Einführung
Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von Geräten und einem Lösungs-Back-End ermöglicht. Weitere Tutorials ([Erste Schritte mit IoT Hub] und [Senden von C2D-Nachrichten mit IoT Hub][lnk-c2d]) veranschaulichen, wie Sie die grundlegenden Device-to-Cloud- und Cloud-to-Device-Messagingfunktionen von IoT Hub verwenden.

Dieses Tutorial stützt sich auf den Code, der im Tutorial [Erste Schritte mit IoT Hub] vorgestellt wird, und zeigt Ihnen, wie Sie das Nachrichtenrouting auf einfache Weise und auf Konfigurationsbasis für das Senden von D2C-Nachrichten einsetzen. Das Tutorial veranschaulicht das Isolieren von Nachrichten, die vom Lösungs-Back-End sofortiges Eingreifen zur weiteren Verarbeitung erfordern. Beispielsweise könnte ein Gerät eine Alarmnachricht senden, die das Einfügen eines Tickets in ein CRM-System auslöst. Im Gegensatz dazu werden Datenpunktnachrichten einfach in ein Analysemodul eingegeben. Temperaturtelemetriedaten von einem Gerät, die zur späteren Analyse gespeichert werden, sind beispielsweise Datenpunktnachrichten.

Am Ende dieses Tutorials führen Sie drei .NET-Konsolen-Apps aus:

* **SimulatedDevice**, eine angepasste Version der im Tutorial [Erste Schritte mit IoT Hub] erstellten App, die jede Sekunde Datenpunkt-D2C-Nachrichten und alle 10 Sekunden interaktive D2C-Nachrichten sendet. Für diese App wird das AMQP-Protokoll für die Kommunikation mit IoT Hub verwendet.
* **ReadDeviceToCloudMessages** zeigt die nicht schwerwiegende Telemetrie an, die Ihre simulierte Geräte-App sendet.
* **ReadCriticalQueue** holt die von Ihrer simulierten Geräte-App gesendeten kritischen Nachrichten aus der Service Bus-Warteschlange, die mit dem IoT Hub verbunden ist.

> [!NOTE]
> IoT Hub verfügt über SDK-Unterstützung für zahlreiche Geräteplattformen und Sprachen, z.B. C, Java und JavaScript. Informationen zum Ersetzen des simulierten Geräts in diesem Tutorial durch ein physisches Gerät und zum Verbinden von Geräten mit einem IoT Hub finden Sie im [Azure IoT Developer Center].
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Visual Studio 2015.
* Ein aktives Azure-Konto. <br/>Wenn Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen.

Sie sollten über grundlegende Kenntnisse zu [Azure Storage] und [Azure Service Bus] verfügen.

## <a name="send-interactive-messages-from-a-simulated-device-app"></a>Senden interaktiver Nachrichten von einer simulierten Geräte-App aus
In diesem Abschnitt ändern Sie die simulierte Geräte-App, die Sie im Tutorial [Erste Schritte mit IoT Hub] erstellt haben, sodass sie gelegentlich Nachrichten sendet, die sofort verarbeitet werden müssen.

- Ersetzen Sie in Visual Studio im **SimulatedDevice**-Projekt die `SendDeviceToCloudMessagesAsync`-Methode durch den folgenden Code.
   
    ```
    private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                string levelValue;

                if (rand.NextDouble() > 0.7)
                {
                    messageString = "This is a critical message";
                    levelValue = "critical";
                }
                else
                {
                    levelValue = "normal";
                }
                
                var message = new Message(Encoding.ASCII.GetBytes(messageString));
                message.Properties.Add("level", levelValue);
                
                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sent message: {1}", DateTime.Now, messageString);

                await Task.Delay(1000);
            }
        }
    ```
   
     Hiermit wird vom Gerät gesendeten Nachrichten nach dem Zufallsprinzip die Eigenschaft `"level": "critical"` hinzugefügt, wodurch eine Nachricht simuliert wird, die vom Lösungs-Back-End sofortiges Eingreifen erfordert. Die Geräte-App übergibt diese Information den Eigenschaften der Nachricht statt dem Nachrichtentext, sodass IoT Hub die Nachricht an das richtige Nachrichtenziel weiterleiten kann.

   > [!NOTE]
   > Sie können Nachrichteneigenschaften zum Weiterleiten von Nachrichten für eine Vielzahl von Szenarien zusätzlich zu dem hier gezeigten Beispiel des langsamsten Pfads verwenden – einschließlich der Cold-Path-Verarbeitung.
   > 
   > 
   
   > [!NOTE]
   > Der Einfachheit halber wird in diesem Tutorial keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie eine Wiederholungsrichtlinie implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler]beschrieben.
   > 
   > 

## <a name="add-a-queue-to-your-iot-hub-and-route-messages-to-it"></a>Hinzufügen einer Warteschlange zu Ihrem IoT Hub und Weiterleiten von Nachrichten an sie
In diesem Abschnitt erstellen Sie eine Service Bus-Warteschlange, verbinden sie mit Ihrem IoT Hub und konfigurieren Ihren IoT Hub zum Senden von Nachrichten an die Warteschlange, sofern eine Eigenschaft in der Nachricht vorhanden ist. Weitere Informationen zum Verarbeiten von Nachrichten von der Service Bus-Warteschlange finden Sie in [Erste Schritte mit Event Hubs][Service Bus queue].

1. Erstellen Sie eine Service Bus-Warteschlange wie in [erste Schritte mit Warteschlangen][Service Bus queue] beschrieben. Die Warteschlange muss sich in dem gleichen Abonnement und der gleichen Region wie der IoT Hub befinden. Notieren Sie den Namespace und den Warteschlangennamen.

2. Öffnen Sie Ihren IoT Hub im Azure-Portal, und klicken Sie auf **Endpunkte**.
    
    ![Endpunkte in IoT Hub][30]

3. Klicken Sie oben auf dem Blatt „Endpunkte“ auf **Hinzufügen**, um die Warteschlange Ihrem IoT Hub hinzuzufügen. Benennen Sie den Endpunkt „CriticalQueue“ und, wählen Sie mithilfe der Dropdownlisten **Service Bus-Warteschlange**, den Service Bus-Namespace, in dem sich die Warteschlange befindet, und den Namen der Warteschlange. Klicken Sie anschließend am unteren Rand auf **Speichern**.
    
    ![Hinzufügen eines Endpunkts][31]
    
4. Klicken Sie jetzt in Ihrem IoT Hub auf **Routen**. Klicken Sie am oberen Rand des Blatts auf **Hinzufügen**, um eine Regel zu erstellen, die Nachrichten an die gerade von Ihnen hinzugefügte Warteschlange leitet. Wählen Sie **DeviceTelemetry** als Quelle der Daten. Geben Sie `level="critical"` als Bedingung ein, und wählen Sie die Warteschlange, die Sie gerade als Endpunkt hinzugefügt haben, als Routenendpunkt. Klicken Sie anschließend am unteren Rand auf **Speichern**.
    
    ![Hinzufügen einer Route][32]
    
    Stellen Sie sicher, dass die Fallbackroute auf EIN festgelegt ist. Dies ist die Standardkonfiguration des IoT Hubs.
    
    ![Fallbackroute][33]

## <a name="read-from-the-queue-endpoint"></a>Lesen aus dem Warteschlangen-Endpunkt
In diesem Abschnitt lesen Sie Nachrichten aus dem Warteschlangen-Endpunkt.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein Visual C#-Windows-Projekt. Nennen Sie das Projekt **ReadCriticalQueue**.

2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt **ReadCriticalQueue**, und klicken Sie dann auf **NuGet-Pakete verwalten**. Mit diesem Vorgang wird das Fenster **NuGet-Paket-Manager** angezeigt.

3. Suchen Sie nach **WindowsAzure.ServiceBus**, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. Mit diesem Vorgang wird Azure Service Bus mit allen Abhängigkeiten heruntergeladen, installiert und mit einem Verweis versehen.

4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden **using**-Anweisungen hinzu:
   
    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu. Ersetzen Sie die Verbindungszeichenfolge durch **Lauschen**-Berechtigungen für die Warteschlange:
   
    ```
    Console.WriteLine("Receive critical messages. Ctrl-C to exit.\n");
    var connectionString = "{service bus listen string}";
    var queueName = "{queue name}";
    
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);

    client.OnMessage(message =>
        {
            Stream stream = message.GetBody<Stream>();
            StreamReader reader = new StreamReader(stream, Encoding.ASCII);
            string s = reader.ReadToEnd();
            Console.WriteLine(String.Format("Message body: {0}", s));
        });
        
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>Ausführen der Anwendungen
Sie können jetzt die Anwendung ausführen.

1. Klicken Sie in Visual Studio im Projektmappen-Explorer mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen**aus. Wählen Sie **Mehrere Startprojekte** aus, und wählen Sie dann für die Projekte **ReadDeviceToCloudMessages**, **SimulatedDevice** und **ReadCriticalQueue** als Aktion die Option **Starten** aus.
2. Drücken Sie** **F5, um die drei Konsolen-Apps zu starten. Die **ReadDeviceToCloudMessages**-App verfügt nur über unkritische, von der **SimulatedDevice**-Anwendung gesendete Nachrichten, und die **ReadCriticalQueue**-App verfügt nur über kritische Nachrichten.
   
   ![Drei Konsolen-Apps][50]

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie gelernt, D2C-Nachrichten zuverlässig mit der Nachrichtenroutingfunktion von IoT Hub zu versenden.

Im Tutorial [Gewusst wie: Senden von C2D-Nachrichten mithilfe von IoT Hub][lnk-c2d] erfahren Sie, wie Sie Nachrichten aus Ihrem Lösungs-Back-End an Ihre Geräte senden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite][lnk-suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].

Weitere Informationen zum Nachrichtenrouting in IoT Hub finden Sie unter [Senden und Empfangen von Nachrichten mit IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/route-creation.png
[33]: ./media/iot-hub-csharp-csharp-process-d2c/fallback-route.png

<!-- Links -->

[Azure blob storage]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure Data Factory]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[Service Bus queue]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[IoT Hub developer guide - Device to cloud]: iot-hub-devguide-messaging.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[About Azure Storage]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Get Started with Event Hubs]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure Storage scalability Guidelines]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Event Hubs Programming Guide]: ../event-hubs/event-hubs-programming-guide.md
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Build multi-tier applications with Service Bus]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/



<!--HONumber=Jan17_HO1-->


