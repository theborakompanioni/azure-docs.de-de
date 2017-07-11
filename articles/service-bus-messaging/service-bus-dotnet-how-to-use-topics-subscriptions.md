---
title: Erste Schritte mit Azure Service Bus-Themen und -Abonnements | Microsoft-Dokumentation
description: "Schreiben Sie eine C#-Konsolenanwendung, für die Service Bus Messaging-Themen und -Abonnements verwendet werden."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.contentlocale: de-de
ms.lasthandoff: 07/01/2017


---
<a id="get-started-with-service-bus-topics" class="xliff"></a>

# Erste Schritte mit Service Bus-Themen

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

<a id="what-will-be-accomplished" class="xliff"></a>

## Ziele

Dieses Tutorial enthält die folgenden Schritte:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals
2. Erstellen eines Service Bus-Themas mithilfe des Azure-Portals
3. Erstellen eines Service Bus-Abonnements für dieses Thema mit dem Azure-Portal
4. Schreiben einer Konsolenanwendung zum Senden einer Nachricht an das Thema
5. Schreiben einer Konsolenanwendung zum Empfangen dieser Nachricht aus dem Abonnement

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

1. [Visual Studio 2015 oder höher](http://www.visualstudio.com) Für die Beispiele in diesem Tutorial wird Visual Studio 2017 verwendet.
2. Ein Azure-Abonnement.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

<a id="1-create-a-namespace-using-the-azure-portal" class="xliff"></a>

## 1. Erstellen eines Namespace mithilfe des Azure-Portals

Falls Sie bereits einen Service Bus Messaging-Namespace erstellt haben, können Sie zum Abschnitt [Erstellen eines Themas mit dem Azure-Portal](#2-create-a-topic-using-the-azure-portal) springen.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

<a id="2-create-a-topic-using-the-azure-portal" class="xliff"></a>

## 2. Erstellen eines Themas mit dem Azure-Portal

1. Melden Sie sich beim [Azure-Portal][azure-portal] an.
2. Klicken Sie im linken Navigationsbereich des Portals auf **Service Bus**. (Wird **Service Bus** nicht angezeigt, klicken Sie auf **More services** (Weitere Dienste)).
3. Klicken Sie auf den Namespace, in dem das Thema erstellt werden soll. Das Blatt mit der Namespace-Übersicht wird angezeigt:
   
    ![Erstellen eines Themas][createtopic1]
4. Klicken Sie auf dem Blatt **Service Bus-Namespace** auf **Themen** und dann auf **Thema hinzufügen**.
   
    ![Auswählen von Themen][createtopic2]
5. Geben Sie einen Namen für das Thema ein, und deaktivieren Sie die Option **Partitionierung aktivieren**. Behalten Sie bei den anderen Optionen die Standardwerte bei.
   
    ![„Neu“ wählen][createtopic3]
6. Klicken Sie im unteren Bereich des Blatts auf **Erstellen**.

<a id="3-create-a-subscription-to-the-topic" class="xliff"></a>

## 3. Erstellen eines Abonnements für das Thema

1. Klicken Sie im Bereich mit den Portalressourcen auf den Namespace, den Sie in Schritt 1 erstellt haben, und klicken Sie dann auf den Namen des Themas, das Sie in Schritt 2 erstellt haben.
2. Klicken Sie oben im Übersichtsbereich auf das Pluszeichen neben **Abonnement**, um diesem Thema ein Abonnement hinzuzufügen.

    ![Erstellen des Abonnements][createtopic4]

3. Geben Sie einen Namen für das Abonnement ein. Behalten Sie bei den anderen Optionen die Standardwerte bei.

<a id="4-send-messages-to-the-topic" class="xliff"></a>

## 4. Senden von Nachrichten an das Thema

Um Nachrichten an das Thema senden zu können, erstellen wir mit Visual Studio eine C#-Konsolenanwendung.

<a id="create-a-console-application" class="xliff"></a>

### Erstellen einer Konsolenanwendung

Starten Sie Visual Studio, und erstellen Sie ein neues Projekt vom Typ **Konsolen-App (.NET Framework)**.

<a id="add-the-service-bus-nuget-package" class="xliff"></a>

### Hinzufügen des NuGet-Pakets "Service Bus"

1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.
2. Klicken Sie auf die Registerkarte **Durchsuchen**, suchen Sie nach **Microsoft Azure Service Bus**, und wählen Sie anschließend das Element **WindowsAzure.ServiceBus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.
   
    ![Auswählen eines NuGet-Pakets][nuget-pkg]

<a id="write-some-code-to-send-a-message-to-the-topic" class="xliff"></a>

### Schreiben von Code zum Senden einer Nachricht an das Thema

1. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Fügen Sie der `Main` -Methode den folgenden Code hinzu. Legen Sie die `connectionString`-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und legen Sie `topicName` auf den Namen fest, den Sie beim Erstellen des Themas verwendet haben.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
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

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Führen Sie das Programm aus, und prüfen Sie im Azure-Portal Folgendes: Klicken Sie auf dem Blatt **Übersicht** auf den Namen Ihres Themas im Namespace. Das Blatt **Essentials** des Themas wird angezeigt. Beachten Sie in den Abonnements, die unten auf dem Blatt aufgeführt sind, dass der Wert für **Nachrichtenanzahl** für jedes Abonnement jetzt „1“ lauten sollte. Jedes Mal, wenn Sie die Absenderanwendung ausführen, ohne Nachrichten abzurufen (wie im nächsten Abschnitt beschrieben), erhöht sich dieser Wert um 1. Beachten Sie auch, dass bei der aktuellen Größe des Themas der Wert **Aktuell** auf dem Blatt **Essentials** jeweils inkrementiert wird, wenn die App dem Thema bzw. Abonnement eine Nachricht hinzufügt.
   
      ![Nachrichtengröße][topic-message]

<a id="5-receive-messages-from-the-subscription" class="xliff"></a>

## 5. Empfangen von Nachrichten aus dem Abonnement

1. Gehen Sie wie folgt vor, um die gerade gesendeten Nachrichten zu empfangen: Erstellen Sie eine neue Konsolenanwendung, und fügen Sie einen Verweis auf das Service Bus-NuGet-Paket hinzu, ähnlich wie bei der vorherigen Absenderanwendung.
2. Fügen Sie am Anfang der Datei „Program.cs“ die folgende `using`-Anweisung hinzu:
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Fügen Sie der `Main` -Methode den folgenden Code hinzu. Legen Sie die `connectionString`-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace erhalten haben, und legen Sie `topicName` auf den Namen fest, den Sie beim Erstellen des Themas verwendet haben.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Führen Sie das Programm aus, und überprüfen Sie die Angaben im Portal erneut. Beachten Sie, dass die Werte für **Nachrichtenanzahl** und **Aktuell** jetzt „0“ lauten.
   
    ![Länge des Themas][topic-message-receive]

Glückwunsch! Sie haben jetzt ein Thema und ein Abonnement erstellt, eine Nachricht gesendet und diese Nachricht empfangen.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte

Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure/azure-service-bus/tree/master/samples) an, mit denen einige erweiterte Features des Service Bus Messaging veranschaulicht werden.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com

