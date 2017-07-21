---
title: Erste Schritte mit Azure Service Bus-Warteschlangen | Microsoft-Dokumentation
description: "Schreiben Sie eine C#-Konsolenanwendung, für die Service Bus Messaging-Warteschlangen verwendet werden."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 02d0ce093bc42cffa4f3993826c61c8aeca4d033
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017

---
# <a name="get-started-with-service-bus-queues"></a>Erste Schritte mit Service Bus-Warteschlangen
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Ziele
Dieses Tutorial enthält die folgenden Schritte:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
2. Erstellen einer Service Bus-Warteschlange mithilfe des Azure-Portals
3. Erstellen einer Konsolenanwendung zum Senden einer Nachricht
4. Schreiben einer Konsolenanwendung zum Empfangen der im vorherigen Schritt gesendeten Nachrichten

## <a name="prerequisites"></a>Voraussetzungen
1. [Visual Studio 2015 oder höher](http://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2017 verwendet.
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Erstellen eines Namespace mithilfe des Azure-Portals
Falls Sie bereits einen Service Bus Messaging-Namespace erstellt haben, können Sie zum Abschnitt [Erstellen einer Warteschlange mit dem Azure-Portal](#2-create-a-queue-using-the-azure-portal) springen.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Erstellen einer Warteschlange mithilfe des Azure-Portals
Falls Sie bereits eine Service Bus-Warteschlange erstellt haben, fahren Sie mit dem Abschnitt [Senden von Nachrichten an die Warteschlange](#3-send-messages-to-the-queue) fort.

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Senden von Nachrichten an die Warteschlange
Um Nachrichten an die Warteschlange senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung

Starten Sie Visual Studio, und erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**.

### <a name="add-the-service-bus-nuget-package"></a>Hinzufügen des NuGet-Pakets "Service Bus"
1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, suchen Sie nach **Microsoft Azure Service Bus**, und wählen Sie anschließend das Element **WindowsAzure.ServiceBus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
   
    ![Auswählen eines NuGet-Pakets][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Schreiben von Code zum Senden einer Nachricht an die Warteschlange
1. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Fügen Sie der `Main` -Methode den folgenden Code hinzu. Legen Sie die `connectionString`-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und legen Sie `queueName` auf den Warteschlangennamen fest, den Sie beim Erstellen der Warteschlange verwendet haben.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Die Datei „Program.cs“ sollte nun wie folgt aussehen:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Führen Sie das Programm aus, und prüfen Sie im Azure-Portal Folgendes: Klicken Sie auf dem Blatt **Übersicht** auf den Namen Ihrer Warteschlange im Namespace. Das Blatt **Essentials** der Warteschlange wird angezeigt. Unter **Anzahl aktiver Nachrichten** sollte nun der Wert „1“ angezeigt werden. Jedes Mal, wenn Sie die Absenderanwendung ausführen, ohne Nachrichten abzurufen, erhöht sich dieser Wert um 1. Beachten Sie auch, dass die aktuelle Größe der Warteschlange jedes Mal inkrementell zunimmt, wenn die App der Warteschlange eine Nachricht hinzufügt.
   
      ![Nachrichtengröße][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Empfangen von Nachrichten aus der Warteschlange

1. Gehen Sie wie folgt vor, um die gerade gesendeten Nachrichten zu empfangen: Erstellen Sie eine neue Konsolenanwendung, und fügen Sie einen Verweis auf das Service Bus-NuGet-Paket hinzu, ähnlich wie bei der vorherigen Absenderanwendung.
2. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Fügen Sie der `Main` -Methode den folgenden Code hinzu. Legen Sie die `connectionString`-Variable auf die Verbindungszeichenfolge fest, die beim Erstellen des Namespace abgerufen wurde, und legen Sie `queueName` auf den Warteschlangennamen fest, den Sie beim Erstellen der Warteschlange verwendet haben.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
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
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Führen Sie das Programm aus, und überprüfen Sie die Angaben im Portal erneut. Beachten Sie, dass die Werte für **Anzahl aktiver Nachrichten** und **Aktuell** jetzt „0“ lauten.
   
    ![Warteschlangenlänge][queue-message-receive]

Glückwunsch! Sie haben eine Warteschlange erstellt, eine Nachricht gesendet und eine Nachricht empfangen.

## <a name="next-steps"></a>Nächste Schritte

Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure/azure-service-bus/tree/master/samples) an, mit denen einige erweiterte Features des Service Bus Messaging veranschaulicht werden.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

