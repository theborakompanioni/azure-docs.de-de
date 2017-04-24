---
title: Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Framework | Microsoft-Dokumentation
description: Erste Schritte beim Senden von Ereignissen an Event Hubs mithilfe von .NET Framework
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/08/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 306c9c5cb06caa186bc0b7f431a5412dfe810722
ms.lasthandoff: 04/18/2017


---
# <a name="send-events-to-azure-event-hubs-using-the-net-framework"></a>Senden von Ereignissen an Azure Event Hubs mithilfe von .NET Framework

## <a name="introduction"></a>Einführung
Event Hubs ist ein Dienst, der große Mengen von Ereignisdaten (Telemetriedaten) von verbundenen Geräten und Anwendungen verarbeiten kann. Nach dem Sammeln von Daten auf Ereignis-Hubs können die Daten mithilfe eines Speicherclusters gespeichert oder mit einem Echtzeitanalyse-Anbieter transformiert werden. Diese umfangreiche Ereignissammlung und -verarbeitung ist eine wichtige Komponente moderner Anwendungsarchitekturen. Hierzu zählt auch das Internet der Dinge (Internet of Things, IoT).

In diesem Tutorial erfahren Sie, wie Sie über das [Azure-Portal](https://portal.azure.com) einen Event Hub erstellen. Außerdem wird gezeigt, wie Sie mithilfe von .NET Framework Ereignisse über eine in C# geschriebene Konsolenanwendung an einen Event Hub senden. Informationen zum Empfangen von Ereignissen mithilfe von .NET Framework finden Sie im Artikel [Receive events using the .NET Framework](event-hubs-dotnet-framework-getstarted-receive-eph.md) (Empfangen von Ereignissen mithilfe von .NET Framework). Alternativ können Sie auch links im Inhaltsverzeichnis auf die entsprechende Empfangssprache klicken.

Für dieses Tutorial benötigen Sie Folgendes:

* [Microsoft Visual Studio 2015 oder höher](http://visualstudio.com) Die Screenshots in diesem Tutorial zeigen Visual Studio 2017.
* Ein aktives Azure-Konto. Falls Sie nicht über ein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Konto erstellen. Weitere Informationen finden Sie unter [Kostenloses Azure-Testkonto](https://azure.microsoft.com/free/).

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Erstellen eines Event Hubs-Namespace und eines Event Hubs

Verwenden Sie zunächst das [Azure-Portal](https://portal.azure.com), um einen Namespace vom Typ „Event Hubs“ zu erstellen, und beschaffen Sie die Verwaltungsanmeldeinformationen, die Ihre Anwendung für die Kommunikation mit dem Event Hub benötigt. Folgen Sie dem Ablauf in [diesem Artikel](event-hubs-create.md), um einen Namespace und einen Event Hub zu erstellen, und fahren Sie dann mit den folgenden Schritten fort.

## <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung
In diesem Abschnitt schreiben wir eine Windows-Konsolenanwendung, die Ereignisse an den Event Hub sendet.

1. Erstellen Sie in Visual Studio mithilfe der Projektvorlage **Konsolenanwendung** ein neues Visual C#-Desktopanwendungsprojekt. Geben Sie dem Projekt den Namen **Sender**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp1.png)
2. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das **Sender**-Projekt, und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**. 
3. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach `Microsoft Azure Service Bus`. Klicken Sie auf **Installieren**, und akzeptieren Sie die Nutzungsbedingungen. 
   
    ![](./media/event-hubs-dotnet-framework-getstarted-send/create-sender-csharp2.png)
   
    Visual Studio lädt das [NuGet-Paket mit der Azure Service Bus-Bibliothek](https://www.nuget.org/packages/WindowsAzure.ServiceBus)herunter, installiert es und fügt dem Projekt einen Verweis auf das Paket hinzu.
4. Fügen Sie am Anfang der Datei **Program.cs** die folgenden `using`-Anweisungen hinzu:
   
    ```csharp
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
5. Fügen Sie der **Program**-Klasse die folgenden Felder hinzu, und ersetzen Sie dabei die Platzhalterwerte durch den Namen des im vorigen Abschnitt erstellten Event Hubs und die zuvor gespeicherte Verbindungszeichenfolge auf Namespace-Ebene.
   
    ```csharp
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```
6. Fügen Sie der **Program** -Klasse die folgende Methode hinzu:
   
    ```csharp
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }
   
            Thread.Sleep(200);
        }
    }
    ```
   
    Diese Methode sendet kontinuierlich Ereignisse mit einer Verzögerung von 200 ms an den Event Hub.
7. Fügen Sie abschließend der **Main** -Methode die folgenden Zeilen hinzu:
   
    ```csharp
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```
8. Führen Sie das Programm aus, und stellen Sie sicher, dass keine Fehler auftreten.
  
Glückwunsch! Sie haben jetzt Nachrichten an einen Event Hub gesendet.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie eine funktionierende Anwendung erstellt haben, die einen Event Hub erstellt und Daten sendet, können Sie mit den folgenden Szenarien fortfahren:

* [Receive events using the Event Processor Host](event-hubs-dotnet-framework-getstarted-receive-eph.md) (Empfangen von Ereignissen mithilfe des Ereignisprozessorhosts)
* [Referenz für den Ereignisprozessorhost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Übersicht über Event Hubs](event-hubs-what-is-event-hubs.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png


