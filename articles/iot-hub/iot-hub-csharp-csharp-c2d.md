---
title: Senden von Cloud-zu-Gerät-Nachrichten mit IoT Hub | Microsoft Docs
description: In diesem Lernprogramm erfahren Sie, wie Sie mithilfe von Azure IoT Hub mit C# Cloud-zu-Gerät-Nachrichten senden.
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2016
ms.author: elioda

---
# Tutorial: Senden von Cloud-zu-Gerät-Nachrichten mithilfe von IoT Hub und .NET
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## Einführung
Azure IoT Hub ist ein vollständig verwalteter Dienst, der eine zuverlässige und sichere bidirektionale Kommunikation zwischen Millionen von IoT-Geräten und einem Anwendungs-Back-End ermöglicht. Im Lernprogramm [Erste Schritte mit IoT Hub] erfahren Sie, wie ein IoT Hub erstellt, eine Geräteidentität im Hub bereitgestellt und ein simuliertes Gerät programmiert wird, das Gerät-zu-Cloud-Nachrichten sendet.

Dieses Tutorial baut auf [Erste Schritte mit IoT Hub] auf. Es beschreibt Folgendes:

* Senden von C2D-Nachrichten aus Ihrem Anwendungs-Cloud-Back-End an ein einzelnes Gerät über IoT Hub.
* Empfangen von C2D-Nachrichten auf einem Gerät.
* Anfordern einer Übermittlungsbestätigung (*Feedback*) von Ihrem Anwendungs-Cloud-Back-End für Nachrichten, die von IoT Hub an ein einzelnes Gerät gesendet wurden.

Weitere Informationen zu Cloud-zu-Gerät-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

Am Ende dieses Tutorials führen Sie zwei Windows-Konsolenanwendungen aus:

* **SimulatedDevice**, eine abgewandelte Version der in [Erste Schritte mit IoT Hub] erstellten App, die eine Verbindung mit IoT Hub herstellt und Cloud-zu-Gerät-Nachrichten empfängt.
* **SendCloudToDevice**, die über IoT Hub eine C2D-Nachricht an das simulierte Gerät sendet und dann die zugehörige Übermittlungsbestätigung empfängt.

> [!NOTE]
> IoT Hub bietet durch Azure IoT-Geräte-SDKs Unterstützung für zahlreiche Geräteplattformen und Sprachen (u.a. C, Java und JavaScript). Im [Azure IoT Developer Center] finden Sie Schritt-für-Schritt-Anweisungen zum Verbinden eines Geräts mit dem Code in diesem Tutorial sowie allgemeine Informationen zum Verbinden mit Azure IoT Hub.
> 
> 

Für dieses Tutorial benötigen Sie Folgendes:

* Microsoft Visual Studio 2015
* Ein aktives Azure-Konto. (Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk-free-trial].)

## Empfangen von Nachrichten auf dem simulierten Gerät
In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Erste Schritte mit IoT Hub] erstellt haben, um C2D-Nachrichten von IoT Hub zu empfangen.

1. Fügen Sie in Visual Studio im **SimulatedDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.
   
        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();
   
                await deviceClient.CompleteAsync(receivedMessage);
            }
        }
   
    Die `ReceiveAsync`-Methode gibt die empfangene Nachricht asynchron zurück, sobald sie vom Gerät empfangen wird. Nach Ablauf eines festlegbaren Timeoutzeitraums (hier wird der Standardwert eine Minute verwendet) gibt sie *NULL* zurück. Falls dies geschieht, soll der Code weiter auf neue Nachrichten warten, was durch die Zeile `if (receivedMessage == null) continue` gewährleistet ist.
   
    Der Aufruf von `CompleteAsync()` teilt IoT Hub mit, dass die Nachricht erfolgreich verarbeitet wurde. Die Nachricht kann sicher aus der Gerätewarteschlange entfernt werden. Wenn ein Fehler aufgetreten ist, der den Abschluss der Verarbeitung der Nachricht durch die Geräte-App verhindert hat, übermittelt IoT Hub sie erneut. Dann ist es wichtig, dass die Nachrichtenverarbeitungslogik in der Geräte-App *idempotent* ist, sodass der mehrmalige Empfang der gleichen Nachricht das gleiche Ergebnis erzeugt. Eine Anwendung kann eine Nachricht auch vorübergehend verwerfen, sodass IoT Hub die Nachricht für die zukünftige Verwendung in der Warteschlange beibehält. Oder die Anwendung kann eine Nachricht ablehnen, wodurch die Nachricht dauerhaft aus der Warteschlange entfernt wird. Weitere Informationen zum Lebenszyklus von C2D-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].
   
   > [!NOTE]
   > Wird HTTP/1 anstelle von AMQP als Transport verwendet wird, erfolgt von der `ReceiveAsync`-Methode sofort eine Rückgabe. Das unterstützte Muster für C2D-Nachrichten mit HTTP/1 entspricht Geräten, die nicht permanent verbunden sind und weniger häufig Nachrichten abrufen (d.h. seltener als alle 25 Minuten). Die Ausgabe von mehr HTTP/1-Empfangsvorgängen führt dazu, dass die Anforderungen von IoT Hub gedrosselt werden. Weitere Informationen zu den Unterschieden zwischen der AMQP- und HTTP/1-Unterstützung sowie zur IoT Hub-Drosselung finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].
   > 
   > 
2. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` die folgende Methode hinzu:
   
        ReceiveC2dAsync();

> [!NOTE]
> Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.
> 
> 

## Senden einer C2D-Nachricht
In diesem Abschnitt schreiben Sie eine Windows-Konsolen-App, die Cloud-zu-Gerät-Nachrichten an die simulierte Geräte-App sendet.

1. Erstellen Sie in der aktuellen Visual Studio-Projektmappe mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Desktop-App-Projekt. Nennen Sie das Projekt **SendCloudToDevice**.
   
    ![Neues Projekt in Visual Studio][20]
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf die Projektmappe, und klicken Sie dann auf **NuGet-Pakete verwalten...**
   
    Daraufhin wird das Fenster **NuGet-Pakete verwalten** geöffnet.
3. Suchen Sie nach `Microsoft Azure Devices`, klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen.
   
    Daraufhin wird das [NuGet-Paket mit dem Dienst-SDK für Azure IoT] heruntergeladen, installiert und mit einem Verweis versehen.
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
        using Microsoft.Azure.Devices;
5. Fügen Sie der **Program**-Klasse die folgenden Felder hinzu. Ersetzen Sie den Platzhalterwert durch die IoT Hub-Verbindungszeichenfolge aus [Erste Schritte mit IoT Hub]\:
   
        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
6. Fügen Sie der **Program**-Klasse die folgende Methode hinzu:
   
        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }
   
    Diese Methode sendet eine neue C2D-Nachricht an das Gerät mit der ID `myFirstDevice`. Ändern Sie diesen Parameter entsprechend, falls Sie den in [Erste Schritte mit IoT Hub] verwendeten Parameter geändert haben.
7. Fügen Sie abschließend der **Main**-Methode die folgenden Zeilen hinzu:
   
        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
   
        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();
8. Klicken Sie in Visual Studio mit der rechten Maustaste auf Ihre Projektmappe, und wählen Sie **Startprojekte festlegen** aus. Wählen Sie **Mehrere Startprojekte** aus, und wählen Sie dann die Aktion **Starten** für **ProcessDeviceToCloudMessages**, **SimulatedDevice** und **SendCloudToDevice** aus.
9. Drücken Sie **F5**. Alle drei Anwendungen sollten beginnen. Wählen Sie das Fenster **SendCloudToDevice** aus, und drücken Sie die **EINGABETASTE**. Die von der simulierten Anwendung empfangene Meldung sollte angezeigt werden.
   
   ![Nachrichtenempfangs-App][21]

## Empfangen von Übermittlungsfeedback
Es ist möglich, für jede C2D-Nachricht Übermittlungsbestätigungen (oder Ablaufbestätigungen) von IoT Hub anzufordern. Auf diese Weise kann die Wiederholungs- oder Kompensierungslogik einfach vom Cloud-Back-End informiert werden. Weitere Informationen zu C2D-Feedback finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

In diesem Abschnitt ändern Sie die **SendCloudToDevice**-App so, dass sie Feedback von IoT Hub anfordert und empfängt.

1. Fügen Sie in Visual Studio im **SendCloudToDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();
   
            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;
   
                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();
   
                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }
   
    Beachten Sie, dass das Empfangsmuster mit dem Muster zum Empfangen von Cloud-zu-Gerät-Nachrichten von der Geräte-App identisch ist.
2. Fügen Sie in der **Main**-Methode unmittelbar nach der Zeile `serviceClient = ServiceClient.CreateFromConnectionString(connectionString)` folgende Methode hinzu:
   
        ReceiveFeedbackAsync();
3. Um für die Übermittlung der C2D-Nachricht Feedback anzufordern, müssen Sie in der **SendCloudToDeviceMessageAsync**-Methode eine Eigenschaft angeben. Fügen Sie direkt nach der Zeile `var commandMessage = new Message(...);` folgende Zeile hinzu:
   
        commandMessage.Ack = DeliveryAcknowledgement.Full;
4. Drücken Sie **F5**, um die Apps auszuführen. Alle drei Anwendungen sollten gestartet werden. Wählen Sie das Fenster **SendCloudToDevice** aus, und drücken Sie die **EINGABETASTE**. Die Nachricht sollte von der simulierten App empfangen werden, und nach einigen Sekunden sollte die Feedbacknachricht von der **SendCloudToDevice**-Anwendung empfangen werden.
   
   ![Nachrichtenempfangs-App][22]

> [!NOTE]
> Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (etwa einen exponentiellen Backoff), wie im MSDN-Artikel zum [Behandeln vorübergehender Fehler] beschrieben.
> 
> 

## Nächste Schritte
In diesem Lernprogramm haben Sie gelernt, wie Cloud-zu-Gerät-Nachrichten gesendet und empfangen werden.

Beispiele vollständiger Lösungen, die IoT Hub nutzen, finden Sie unter [Azure IoT Suite].

Weitere Informationen zum Entwickeln von Lösungen mit IoT Hub finden Sie im [IoT Hub-Entwicklerhandbuch].

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[NuGet-Paket mit dem Dienst-SDK für Azure IoT]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[Behandeln vorübergehender Fehler]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide.md#c2d

[IoT Hub-Entwicklerhandbuch]: iot-hub-devguide.md
[Erste Schritte mit IoT Hub]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/

<!---HONumber=AcomDC_0727_2016-->