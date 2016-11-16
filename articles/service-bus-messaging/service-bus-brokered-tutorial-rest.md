---
title: REST-Tutorial zu Service Bus-Brokermessaging | Microsoft Docs
description: REST-Lernprogramm zu Brokermessaging
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9b7a8147-a1b1-42fc-b30e-f52e79a902b5
ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 45b72037e2de01b9201edf3e4ebee7e80d996383


---
# <a name="service-bus-brokered-messaging-rest-tutorial"></a>REST-Lernprogramm zu Service Bus-Brokermessaging 
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Tutorial wird gezeigt, wie Sie eine einfache REST-basierte Azure Service Bus-Warteschlange und ein Thema bzw. ein Abonnement erstellen.

## <a name="create-a-namespace"></a>Erstellen eines Namespace
Im ersten Schritt wird der Dienstnamespace erstellt und ein SAS-Schlüssel ([Shared Access Signature](service-bus-sas-overview.md)) abgerufen. Ein Namespace stellt eine Anwendungsgrenze für jede Anwendung bereit, die über Service Bus zur Verfügung steht. Das System generiert automatisch einen SAS-Schlüssel, wenn ein Dienstnamespace erstellt wird. Dienstnamespace und SAS-Schlüssel bilden gemeinsam die Anmeldeinformationen, mit denen sich der Servicebus gegenüber der Anwendung authentifiziert.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-console-client"></a>Erstellen eines Konsolenclients
Mit Service Bus-Warteschlangen können Sie Nachrichten in einer First-In-First-Out-Warteschlange (FIFO) speichern. Bei Themen und Abonnements wird ein Veröffentlichen/Abonnieren-Muster implementiert. Sie erstellen ein Thema und dann ein oder mehrere Abonnements, die diesem Thema zugeordnet sind. Wenn Nachrichten an das Thema gesendet werden, werden sie sofort an die Abonnenten dieses Themas gesendet.

Der Code in diesem Lernprogramm erfüllt die folgenden Aufgaben:

* Verwenden des Namespaces und [Shared Access Signature](service-bus-sas-overview.md)-Schlüssels (SAS) zum Erlangen des Zugriffs auf Ihre Service Bus-Namespaceressourcen
* Erstellen einer Warteschlange, Senden einer Nachricht an die Warteschlange und Lesen der Nachricht aus der Warteschlange
* Erstellen eines Themas und eines Abonnements für das Thema und Senden und Lesen der Nachricht aus dem Abonnement
* Abrufen aller Warteschlangen-, Themen- und Abonnementinformationen, z.B. Abonnementregeln, aus Service Bus
* Löschen der Warteschlangen-, Themen- und Abonnementressourcen

Da es sich beim Dienst um einen Webdienst im REST-Stil handelt, sind keine speziellen Typen involviert, weil es beim gesamten Austausch um Zeichenfolgen geht. Dies bedeutet, dass im Visual Studio-Projekt nicht auf Service Bus-Bibliotheken verwiesen werden darf.

Nach dem Abrufen des Namespace und der Anmeldeinformationen im ersten Schritt erstellen Sie als Nächstes eine einfache Visual Studio-Konsolenanwendung.

### <a name="create-a-console-application"></a>Erstellen einer Konsolenanwendung
1. Starten Sie Visual Studio als Administrator: Klicken Sie dazu mit der rechten Maustaste im **Startmenü** auf das Programm, und klicken Sie dann auf **Als Administrator ausführen**.
2. Erstellen Sie ein neues Konsolenanwendungsprojekt. Klicken Sie im Menü **Datei** auf **Neu** und anschließend auf **Projekt**. Klicken Sie im Dialogfeld **Neues Projekt** auf **Visual C#** (wenn **Visual C#** nicht angezeigt wird, suchen Sie unter **Andere Sprachen**), wählen Sie die Vorlage **Konsolenanwendung** aus, und nennen Sie sie **Microsoft.ServiceBus.Samples**). Verwenden Sie den standardmäßigen Speicherort. Klicken Sie auf **OK** , um das Projekt zu erstellen.
3. Stellen Sie in „Program.cs“ sicher, dass Ihre `using`-Anweisungen wie folgt angezeigt werden:
   
    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```
4. Benennen Sie den Namespace bei Bedarf für das Programm in `Microsoft.ServiceBus.Samples` um, indem Sie den Standardnamen von Visual Studio ändern.
5. Fügen Sie in der `Program`-Klasse die folgenden globalen Variablen hinzu:
   
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```
6. Fügen Sie in `Main()` den folgenden Code ein:
   
    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
   
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
   
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
   
        string queueName = "Queue" + Guid.NewGuid().ToString();
   
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
   
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
   
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
   
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
   
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
   
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
   
        // Delete the queue we created
        DeleteResource(queueName);
   
        // Delete the topic we created
        DeleteResource(topicName);
   
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
   
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## <a name="create-management-credentials"></a>Erstellen von Verwaltungsanmeldeinformationen
Der nächste Schritt ist das Schreiben einer Methode, mit der der Namespace und der SAS-Schlüssel aus dem vorherigen Schritt verarbeitet werden und ein SAS-Token zurückgegeben wird. In diesem Beispiel wird ein SAS-Token erstellt, das eine Stunde lang gültig ist.

### <a name="create-a-getsastoken-method"></a>Erstellen einer GetSASToken()-Methode
Fügen Sie den folgenden Code nach der `Main()`-Methode in der `Program`-Klasse ein:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## <a name="create-the-queue"></a>Erstellen der Warteschlange
Der nächste Schritt ist das Schreiben einer Methode, die zum Erstellen einer Warteschlange den HTTP PUT-Befehl im REST-Stil verwendet.

Fügen Sie den folgenden Code direkt nach dem `GetSASToken()`-Code ein, den Sie im vorherigen Schritt hinzugefügt haben:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="send-a-message-to-the-queue"></a>Senden einer Nachricht an die Warteschlange
In diesem Schritt fügen Sie eine Methode hinzu, in der der HTTP POST-Befehl im REST-Stil zum Senden einer Nachricht an die Warteschlange verwendet wird, die Sie im vorherigen Schritt erstellt haben.

1. Fügen Sie den folgenden Code direkt nach dem `CreateQueue()`-Code ein, den Sie im vorherigen Schritt hinzugefügt haben:
   
    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
   
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```
2. Standardmäßige Eigenschaften von Brokernachrichten werden in einem `BrokerProperties`-HTTP-Header angeordnet. Die Brokereigenschaften müssen im JSON-Format serialisiert werden. Fügen Sie den folgenden Code wie im vorherigen Beispiel direkt vor dem `webClient.UploadData()`-Aufruf hinzu, um einen **TimeToLive**-Wert von 30 Sekunden anzugeben und der Nachricht die Nachrichtenbezeichnung „M1“ hinzuzufügen:
   
    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```
   
    Beachten Sie, dass Brokernachrichteneigenschaften hinzugefügt wurden und auch weiterhin hinzugefügt werden. Daher muss in der Sendeanforderung eine API-Version angegeben werden, von der alle Brokernachrichteneigenschaften unterstützt werden, die Teil der Anforderung sind. Wenn die angegebene API-Version eine Brokernachrichteneigenschaft nicht unterstützt, wird diese Eigenschaft ignoriert.
3. Benutzerdefinierte Nachrichteneigenschaften werden als Satz von Schlüssel-Wert-Paaren definiert. Jede benutzerdefinierte Eigenschaft wird in ihrem eigenen TPPT-Header gespeichert. Fügen Sie den folgenden Code direkt vor dem `webClient.UploadData()`-Aufruf hinzu, der im vorherigen Beispiel zu sehen ist, um die benutzerdefinierten Eigenschaften „Priority“ und „Customer“ hinzuzufügen:
   
    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## <a name="receive-and-delete-a-message-from-the-queue"></a>Empfangen und Löschen einer Nachricht aus der Warteschlange
Im nächsten Schritt wird eine Methode hinzugefügt, bei der der HTTP DELETE-Befehl im REST-Stil verwendet wird, um eine Nachricht zu empfangen und aus der Warteschlange zu löschen.

Fügen Sie den folgenden Code direkt nach dem `SendMessage()`-Code ein, den Sie im vorherigen Schritt hinzugefügt haben:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## <a name="create-a-topic-and-subscription"></a>Erstellen eines Themas und Abonnements
Der nächste Schritt ist das Schreiben einer Methode, die zum Erstellen eines Themas den HTTP PUT-Befehl im REST-Stil verwendet. Anschließend schreiben Sie eine Methode, mit der ein Abonnement für dieses Thema erstellt wird.

### <a name="create-a-topic"></a>Erstellen eines Themas
Fügen Sie den folgenden Code direkt nach dem `ReceiveAndDeleteMessage()`-Code ein, den Sie im vorherigen Schritt hinzugefügt haben:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### <a name="create-a-subscription"></a>Erstellen eines Abonnements
Mit dem folgenden Code wird ein Abonnement des Themas erstellt, das Sie im vorherigen Schritt erstellt haben. Fügen Sie den folgenden Code direkt nach der `CreateTopic()`-Definition hinzu:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="retrieve-message-resources"></a>Abrufen von Nachrichtenressourcen
In diesem Schritt fügen Sie Code hinzu, mit dem die Nachrichteneigenschaften abgerufen werden und anschließend die im vorherigen Schritt erstellten Nachrichtenressourcen gelöscht werden.

### <a name="retrieve-an-atom-feed-with-the-specified-resources"></a>Abrufen eines Atom-Feeds mit den angegebenen Ressourcen
Fügen Sie den folgenden Code direkt nach der `CreateSubscription()`-Methode hinzu, die Sie im vorherigen Schritt hinzugefügt haben:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### <a name="delete-messaging-entities"></a>Löschen von Nachrichtenentitäten
Fügen Sie den folgenden Code direkt nach dem Code hinzu, den Sie im vorherigen Schritt hinzugefügt haben:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### <a name="format-the-atom-feed"></a>Formatieren des Atom-Feeds
Die `GetResources()`-Methode enthält einen Aufruf von einer `FormatXml()`-Methode, mit der der abgerufene Atom-Feed neu formatiert wird, damit er besser lesbar ist. Unten ist die Definition von `FormatXml()` angegeben. Fügen Sie diesen Code direkt nach dem `DeleteResource()`-Code hinzu, den Sie im vorherigen Abschnitt hinzugefügt haben:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## <a name="build-and-run-the-application"></a>Erstellen und Ausführen der Anwendung
Sie können die Anwendung jetzt erstellen und ausführen. Klicken Sie in Visual Studio im Menü **Erstellen** auf **Projektmappe erstellen**, oder drücken Sie STRG+UMSCHALT+B****.

### <a name="run-the-application"></a>Ausführen der Anwendung
Wenn keine Fehler vorliegen, können Sie F5 drücken, um die Anwendung auszuführen. Geben Sie bei Aufforderung Ihren Namespace, den SAS-Schlüssel und den Wert des SAS-Schlüssels aus dem ersten Schritt ein.

### <a name="example"></a>Beispiel
Das folgende Beispiel enthält den vollständigen Code nach dem Ausführen aller Schritte in diesem Lernprogramm.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie in den folgenden Artikeln:

* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [Azure Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)
* [REST-Tutorial zu Service Bus Relay](../service-bus-relay/service-bus-relay-rest-tutorial.md)




<!--HONumber=Nov16_HO2-->


