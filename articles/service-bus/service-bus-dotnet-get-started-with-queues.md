<properties
   pageTitle="Erste Schritte mit Service Bus-Warteschlangen | Microsoft Azure"
   description="Gewusst wie: Erstellen einer C#-Konsolenanwendung für Service Bus-Messaging"
   services="service-bus"
   documentationCenter=".net"
   authors="jtaubensee"
   manager="timlt"
   editor="sethmanheim"/>

<tags
   ms.service="service-bus"
   ms.devlang="tbd"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="dotnet"
   ms.workload="na"
   ms.date="06/07/2016"
   ms.author="jotaub@microsoft.com"/>

#Erste Schritte mit Service Bus-Warteschlangen
[AZURE.INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

##Ziele
In diesem Tutorial werden folgende Kenntnisse vermittelt:

1. Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals

2. Erstellen einer Service Bus-Messagingwarteschlange mithilfe des Azure-Portals

3. Erstellen einer Konsolenanwendung zum Senden einer Nachricht

4. Erstellen einer Konsolenanwendung zum Empfangen von Nachrichten

##Voraussetzungen
1. [Visual Studio 2013/Visual Studio 2015](http://www.visualstudio.com)

2. Ein Azure-Abonnement

##1\. Erstellen eines Namespace mithilfe des Azure-Portals
Falls Sie bereits einen Service Bus-Namespace erstellt haben, fahren Sie mit dem Abschnitt [Erstellen einer Warteschlange mithilfe des Azure-Portals](#2-create-a-queue-using-the-azure-portal) fort.

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

##2\. Erstellen einer Warteschlange mithilfe des Azure-Portals
Falls Sie bereits eine Service Bus-Warteschlange erstellt haben, fahren Sie mit dem Abschnitt [Senden von Nachrichten an die Warteschlange](#3-sending-messages-to-the-queue) fort.

[AZURE.INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

##3\. Senden von Nachrichten an die Warteschlange
Um Nachrichten an die Warteschlange senden zu können, erstellen wir mithilfe von Visual Studio eine C#-Konsolenanwendung.

###Erstellen einer Konsolenanwendung
1. Starten Sie Visual Studio, und erstellen Sie eine neue Konsolenanwendung.

###Hinzufügen des NuGet-Pakets "Service Bus"
1. Klicken Sie mit der rechten Maustaste auf das neu erstellte Projekt, und wählen Sie **NuGet-Pakete verwalten** aus.

2. Suchen Sie nach „Microsoft Azure Service Bus“, und wählen Sie das Element **Microsoft Azure Service Bus** aus. Klicken Sie auf **Installieren**, um die Installation abzuschließen. Schließen Sie danach dieses Dialogfeld.

    ![Auswählen eines NuGet-Pakets][1]

###Schreiben von Code zum Senden einer Nachricht an die Warteschlange
1. Fügen Sie am Anfang der Datei „Program.cs“ die folgende using-Anweisung hinzu:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```
    
2. Fügen Sie der Main-Methode den folgenden Code hinzu, und legen Sie die **connectionString**-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace abgerufen haben. Legen Sie außerdem **queueName** auf den Warteschlangennamen fest, der beim Erstellen der Warteschlange verwendet wurde.

    ```
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
  
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);

    ```

    „Program.cs“ sollte nun wie folgt aussehen:

    ```
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
  
3. Führen Sie das Programm aus, und überprüfen Sie das Azure-Portal. **Warteschlangenlänge** müsste nun den Wert „1“ besitzen.
    
      ![Warteschlangenlänge][2]
    
##4\. Empfangen von Nachrichten aus der Warteschlange
1. Erstellen Sie eine neue Konsolenanwendung, und fügen Sie wie bei der obigen Sendeanwendung einen Verweis auf das NuGet-Paket „Service Bus“ hinzu.

2. Fügen Sie am Anfang der Datei „Program.cs“ die folgende using-Anweisung hinzu:
  
    ```
    using Microsoft.ServiceBus.Messaging;
    ```
  
3. Fügen Sie der Main-Methode den folgenden Code hinzu, und legen Sie die **connectionString**-Variable auf die Verbindungszeichenfolge fest, die Sie beim Erstellen des Namespace abgerufen haben. Legen Sie außerdem **queueName** auf den Warteschlangennamen fest, der beim Erstellen der Warteschlange verwendet wurde.

    ```
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

    ```
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
  
4. Führen Sie das Programm aus, und überprüfen Sie das Azure-Portal. **Warteschlangenlänge** müsste nun den Wert „0“ besitzen.

    ![Warteschlangenlänge][3]
  
Glückwunsch! Sie haben eine Warteschlange erstellt, eine Nachricht gesendet und eine Nachricht empfangen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
##Nächste Schritte

Sehen Sie sich im GitHub-Repository Beispiele an, die einige fortgeschrittenere Features von Azure Service Bus-Messaging veranschaulichen. [https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples]

<!--Image references-->

[1]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[2]: ./media/service-bus-dotnet-get-started-with-queues/queue-length-send.png
[3]: ./media/service-bus-dotnet-get-started-with-queues/queue-length-receive.png


<!--Reference style links - using these makes the source content way more readable than using inline links-->
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

<!---HONumber=AcomDC_0608_2016-->