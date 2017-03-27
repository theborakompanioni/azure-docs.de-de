---
title: Schreiben eines Programms, das Azure Service Bus-Warteschlangen verwendet | Microsoft-Dokumentation
description: "Gewusst wie: Erstellen einer C#-Konsolenanwendung für Service Bus-Messaging"
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/23/2017
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: 83649bdad1d369cdfe4edf3c2bdaa67180db8668
ms.lasthandoff: 03/01/2017


---
# <a name="get-started-with-service-bus-queues"></a>Erste Schritte mit Service Bus-Warteschlangen
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Ziele
In diesem Tutorial werden folgende Kenntnisse vermittelt:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
2. Erstellen einer Service Bus-Messagingwarteschlange mithilfe des Azure-Portals
3. Erstellen einer Konsolenanwendung zum Senden einer Nachricht
4. Erstellen einer Konsolenanwendung zum Empfangen von Nachrichten

## <a name="prerequisites"></a>Voraussetzungen
1. [Visual Studio 2015 oder höher](http://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2015 verwendet.
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Erstellen eines Namespace mithilfe des Azure-Portals
Falls Sie bereits einen Service Bus-Namespace erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Warteschlange mithilfe des Azure-Portals](#2-create-a-queue-using-the-azure-portal) fort.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Erstellen einer Warteschlange mithilfe des Azure-Portals
Falls Sie bereits eine Service Bus-Warteschlange erstellt haben, fahren Sie mit dem Abschnitt [Senden von Nachrichten an die Warteschlange](#3-send-messages-to-the-queue) fort.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Senden von Nachrichten an die Warteschlange
Um Nachrichten an die Warteschlange senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

- Starten Sie Visual Studio, und erstellen Sie eine neue Konsolenanwendung.

### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des NuGet-Pakets "Service Bus"
1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, und suchen Sie nach „Microsoft Azure Service Bus“. Wählen Sie anschließend das Element **Microsoft Azure Service Bus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
   
    ![Auswählen eines NuGet-Pakets][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Schreiben von Code zum Senden einer Nachricht an die Warteschlange
1. Fügen Sie am Anfang der Datei „Program.cs“ die folgende using-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Fügen Sie der `Main`-Methode den folgenden Code hinzu, und legen Sie die **connectionString**-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace abgerufen haben. Legen Sie außerdem **queueName** auf den Warteschlangennamen fest, der beim Erstellen der Warteschlange verwendet wurde.
   
    ```csharp
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```
   
    „Program.cs“ sollte nun wie folgt aussehen:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";
   
                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");
   
                client.Send(message);
            }
        }
    }
    ```
3. Führen Sie das Programm aus, und überprüfen Sie das Azure-Portal. Klicken Sie auf dem Übersichtsblatt**** des Namespaces auf den Namen Ihrer Warteschlange. Unter **Anzahl aktiver Nachrichten** sollte nun der Wert „1“ angezeigt werden.
   
      ![Nachrichtenanzahl][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Empfangen von Nachrichten aus der Warteschlange
1. Erstellen Sie eine neue Konsolenanwendung, und fügen Sie ähnlich wie bei der vorherigen Sendeanwendung einen Verweis auf das NuGet-Paket „Service Bus“ hinzu.
2. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Fügen Sie der `Main`-Methode den folgenden Code hinzu, und legen Sie die **connectionString**-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace abgerufen haben. Legen Sie außerdem **queueName** auf den Warteschlangennamen fest, den Sie beim Erstellen der Warteschlange verwendet haben.
   
    ```csharp
    var connectionString = "";
    var queueName = "samplequeue";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.ReadLine();
    ```
   
    Die Datei „Program.cs“ sollte nun wie folgt aussehen:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "";
          var queueName = "samplequeue";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
   
          Console.ReadLine();
        }
      }
    }
    ```
4. Führen Sie das Programm aus, und überprüfen Sie das Portal. **Warteschlangenlänge** sollte nun den Wert „0“ haben.
   
    ![Warteschlangenlänge][queue-message-receive]

Glückwunsch! Sie haben eine Warteschlange erstellt, eine Nachricht gesendet und eine Nachricht empfangen.

## <a name="next-steps"></a>Nächste Schritte
Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) an, die einige erweiterte Features von Azure Service Bus Messaging veranschaulichen.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

