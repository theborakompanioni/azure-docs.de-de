---
title: Azure Functions Notification Hub-Bindung | Microsoft Docs
description: Erfahren Sie, wie Azure Notification Hub-Bindungen in Azure Functions verwendet werden.
services: functions
documentationcenter: na
author: wesmc7777
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur
ms.assetid: 0ff0c949-20bf-430b-8dd5-d72b7b6ee6f7
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/27/2016
ms.author: wesmc
translationtype: Human Translation
ms.sourcegitcommit: 7e9534afa8ecd224b4e3c1df2f4465b70d961d2c
ms.openlocfilehash: 1e03a54956a9fd035cf40aa953a2c368d91b766b


---
# <a name="azure-functions-notification-hub-output-binding"></a>Azure Functions Notification Hub-Ausgabebindung
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Notification Hub-Bindungen in Azure Functions. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Ihre Funktionen können über einen konfigurierten Azure Notification Hub mit wenigen Codezeilen Pushbenachrichtigungen senden. Allerdings muss der Azure Notification Hub für die Plattformbenachrichtigungsdienste konfiguriert sein, die Sie verwenden möchten. Weitere Informationen zum Konfigurieren eines Azure Notification Hubs und zum Entwickeln von Clientanwendungen, die sich für den Empfang von Benachrichtigungen registrieren, finden Sie unter [Erste Schritte mit Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md). Hier können Sie im oberen Bereich die gewünschte Zielclientplattform auswählen.

Die von Ihnen gesendeten Benachrichtigungen können native Benachrichtigungen oder Vorlagenbenachrichtigungen sein. Native Benachrichtigungen sind für eine spezifische Benachrichtigungsplattform bestimmt. Dies wird in der `platform`-Eigenschaft der Ausgabebindung konfiguriert. Eine Vorlagenbenachrichtigung kann für mehrere Plattformen verwendet werden.   

## <a name="notification-hub-output-binding-properties"></a>Eigenschaften der Notification Hub-Ausgabebindung
Die Datei „function.json“ stellt die folgenden Eigenschaften bereit:

* `name` : Variablenname, der im Funktionscode für die Notification Hub-Nachricht verwendet wird.
* `type` : Muss auf *"notificationHub"*festgelegt werden.
* `tagExpression` : Mit Tagausdrücken können Sie Benachrichtigungen an eine Gruppe von Geräten übermitteln, die sich für den Empfang entsprechender Benachrichtigungen registriert haben.  Weitere Informationen finden Sie unter [Weiterleitung und Tagausdrücke](../notification-hubs/notification-hubs-tags-segment-push-message.md).
* `hubName` : Name der Notification Hub-Ressource im Azure-Portal.
* `connection` : Diese Verbindungszeichenfolge muss eine Verbindungszeichenfolge für die **Anwendungseinstellung** sein, die auf den *DefaultFullSharedAccessSignature* -Wert Ihres Notifications Hubs festgelegt ist.
* `direction` : muss auf *"out"*festgelegt werden. 
* `platform`: Mit der platform-Eigenschaft wird die Benachrichtigungsplattform Ihrer Benachrichtigungsziele angegeben. Dies muss einer der folgenden Werte sein:
  * `template`: Dies ist die Standardplattform, wenn die platform-Eigenschaft in der Ausgabebindung weggelassen wird. Vorlagenbenachrichtigungen können für alle Plattformen verwendet werden, die auf dem Azure Notification Hub konfiguriert sind. Weitere Informationen zur allgemeinen Verwendung von Vorlagen zum Senden von plattformübergreifenden Benachrichtigungen mit einem Azure Notification Hub finden Sie unter [Vorlagen](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md).
  * `apns`: Apple Push Notification Service. Weitere Informationen zur Konfiguration des Notification Hub für APNS und zum Empfang der Benachrichtigung in einer Client-App finden Sie unter [Senden von Pushbenachrichtigungen an iOS mit Azure Notification Hubs](../notification-hubs/notification-hubs-ios-apple-push-notification-apns-get-started.md). 
  * `adm`: [Amazon Device Messaging](https://developer.amazon.com/device-messaging). Weitere Informationen zur Konfiguration des Notification Hub für ADM und zum Empfang der Benachrichtigung in einer Kindle-App finden Sie unter [Erste Schritte mit Notification Hubs für Kindle-Apps](../notification-hubs/notification-hubs-kindle-amazon-adm-push-notification.md). 
  * `gcm`: [Google Cloud Messaging](https://developers.google.com/cloud-messaging/). Firebase Cloud Messaging, die neue Version von GCM, wird ebenfalls unterstützt. Weitere Informationen zur Konfiguration des Notification Hub für GCM/FCM und zum Empfang der Benachrichtigung in einer Android-Client-App finden Sie unter [Senden von Pushbenachrichtigungen an Android mit Azure Notification Hubs](../notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started.md).
  * `wns`: [Windows-Pushbenachrichtigungsdienst](https://msdn.microsoft.com/en-us/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) für Windows-Plattformen. Windows Phone 8.1 und höher wird vom Windows-Pushbenachrichtigungsdienst (WNS) ebenfalls unterstützt. Weitere Informationen zur Konfiguration des Notification Hub für WNS und zum Empfang der Benachrichtigung in einer UWP-App (universelle Windows-Plattform) finden Sie unter [Erste Schritte mit Notification Hubs für Apps für die universelle Windows-Plattform](../notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
  * `mpns`: [Microsoft-Pushbenachrichtigungsdienst](https://msdn.microsoft.com/en-us/library/windows/apps/ff402558.aspx). Diese Plattform unterstützt Windows Phone 8 und frühere Windows Phone-Plattformen. Weitere Informationen zur Konfiguration des Notification Hub für MPNS und zum Empfang der Benachrichtigung in einer Windows Phone-App finden Sie unter [Senden von Pushbenachrichtigungen mit Azure Notification Hubs unter Windows Phone](../notification-hubs/notification-hubs-windows-mobile-push-notifications-mpns.md).

Beispiel für „function.json“:

```json
{
  "bindings": [
    {
      "name": "notification",
      "type": "notificationHub",
      "tagExpression": "",
      "hubName": "my-notification-hub",
      "connection": "MyHubConnectionString",
      "platform": "gcm",
      "direction": "out"
    }
  ],
  "disabled": false
}
```

## <a name="notification-hub-connection-string-setup"></a>Einrichten der Notification Hub-Verbindungszeichenfolge
Um eine Notification Hub-Ausgabebindung zu verwenden, müssen Sie die Verbindungszeichenfolge für den Hub konfigurieren. Hierzu können Sie auf der Registerkarte *Integrieren* Ihren Notification Hub auswählen oder einen neuen erstellen. 

Sie können eine Verbindungszeichenfolge für einen vorhandenen Hub auch manuell hinzufügen, indem Sie Ihrem Notification Hub eine Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* hinzufügen. Diese Verbindungszeichenfolge stellt Ihrer Funktion die Zugriffsberechtigungen zum Senden von Nachrichten bereit. Zum Wert der *DefaultFullSharedAccessSignature* -Verbindungszeichenfolge gelangen Sie über die Schaltfläche **Schlüssel** auf dem Hauptblatt der Notification Hub-Ressource im Azure-Portal. Um eine Verbindungszeichenfolge für den Hub manuell hinzuzufügen, gehen Sie folgendermaßen vor: 

1. Klicken Sie im Azure-Portal auf dem Blatt **Funktionen-App** auf **Funktionen-App-Einstellungen > Zu App Service-Einstellungen wechseln**.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Anwendungseinstellungen**.
3. Scrollen Sie nach unten zum Abschnitt **Verbindungszeichenfolgen**, und fügen Sie einen benannten Eintrag für den *DefaultFullSharedAccessSignature*-Wert für Ihren Notification Hub hinzu. Ändern Sie den Typ in **Benutzerdefiniert**.
4. Verweisen Sie in den Ausgabebindungen auf den Namen der Verbindungszeichenfolge, ähnlich wie bei **MyHubConnectionString** im obigen Beispiel.

## <a name="apns-native-notifications-with-c-queue-triggers"></a>Native APNS-Benachrichtigungen mit C#-Warteschlangentriggern
In diesem Beispiel wird gezeigt, wie Sie in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen zum Senden einer nativen APNS-Benachrichtigung verwenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native APNS notification is ...
    // { "aps": { "alert": "notification message" }}  

    log.Info($"Sending APNS notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string apnsNotificationPayload = "{\"aps\": {\"alert\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{apnsNotificationPayload}");
    await notification.AddAsync(new AppleNotification(apnsNotificationPayload));        
}
```

## <a name="gcm-native-notifications-with-c-queue-triggers"></a>Native GCM-Benachrichtigungen mit C#-Warteschlangentriggern
In diesem Beispiel wird gezeigt, wie Sie in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen zum Senden einer nativen GCM-Benachrichtigung verwenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The JSON format for a native GCM notification is ...
    // { "data": { "message": "notification message" }}  

    log.Info($"Sending GCM notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string gcmNotificationPayload = "{\"data\": {\"message\": \"A new user wants to be added (" + 
                                        user.name + ")\" }}";
    log.Info($"{gcmNotificationPayload}");
    await notification.AddAsync(new GcmNotification(gcmNotificationPayload));        
}
```

## <a name="wns-native-notifications-with-c-queue-triggers"></a>Native WNS-Benachrichtigungen mit C#-Warteschlangentriggern
In diesem Beispiel wird gezeigt, wie Sie in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen zum Senden einer nativen WNS-Popupbenachrichtigung verwenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.NotificationHubs;
using Newtonsoft.Json;

public static async Task Run(string myQueueItem, IAsyncCollector<Notification> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a new user to be processed in the form of a JSON string with 
    // a "name" value.
    //
    // The XML format for a native WNS toast notification is ...
    // <?xml version="1.0" encoding="utf-8"?>
    // <toast>
    //      <visual>
    //     <binding template="ToastText01">
    //       <text id="1">notification message</text>
    //     </binding>
    //   </visual>
    // </toast>

    log.Info($"Sending WNS toast notification of a new user");    
    dynamic user = JsonConvert.DeserializeObject(myQueueItem);    
    string wnsNotificationPayload = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                                    "<toast><visual><binding template=\"ToastText01\">" +
                                        "<text id=\"1\">" + 
                                            "A new user wants to be added (" + user.name + ")" + 
                                        "</text>" +
                                    "</binding></visual></toast>";

    log.Info($"{wnsNotificationPayload}");
    await notification.AddAsync(new WindowsNotification(wnsNotificationPayload));        
}
```

## <a name="template-example-for-nodejs-timer-triggers"></a>Vorlagenbeispiel für Node.js-Trigger mit Timer
In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) gesendet, die `location` und `message` enthält.

```javascript
module.exports = function (context, myTimer) {
    var timeStamp = new Date().toISOString();

    if(myTimer.isPastDue)
    {
        context.log('Node.js is running late!');
    }
    context.log('Node.js timer trigger function ran!', timeStamp);  
    context.bindings.notification = {
        location: "Redmond",
        message: "Hello from Node!"
    };
    context.done();
};
```

## <a name="template-example-for-f-timer-triggers"></a>Vorlagenbeispiel für F#-Trigger mit Timer
In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) gesendet, die `location` und `message` enthält.

```fsharp
let Run(myTimer: TimerInfo, notification: byref<IDictionary<string, string>>) =
    notification = dict [("location", "Redmond"); ("message", "Hello from F#!")]
```

## <a name="template-example-using-an-out-parameter"></a>Vorlagenbeispiel mit einem out-Parameter
In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md) gesendet, bei der in der Vorlage ein `message`-Platzhalter enthalten ist.

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static void Run(string myQueueItem,  out IDictionary<string, string> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = GetTemplateProperties(myQueueItem);
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return templateProperties;
}
```

## <a name="template-example-with-asynchronous-function"></a>Vorlagenbeispiel mit asynchroner Funktion
Wenn Sie asynchronen Code verwenden, sind out-Parameter nicht zulässig. Verwenden Sie in diesem Fall `IAsyncCollector`, um die Vorlagenbenachrichtigung zurückzugeben. Der folgende Code ist ein asynchrones Beispiel des obigen Codes. 

```cs
using System;
using System.Threading.Tasks;
using System.Collections.Generic;

public static async Task Run(string myQueueItem, IAsyncCollector<IDictionary<string,string>> notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    log.Info($"Sending Template Notification to Notification Hub");
    await notification.AddAsync(GetTemplateProperties(myQueueItem));    
}

private static IDictionary<string, string> GetTemplateProperties(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["user"] = "A new user wants to be added : " + message;
    return templateProperties;
}
```

## <a name="template-example-using-json"></a>Vorlagenbeispiel mit JSON
In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates-cross-platform-push-messages.md), die den Platzhalter `message` in der Vorlage enthält, über eine gültige JSON-Zeichenfolge gesendet.

```cs
using System;

public static void Run(string myQueueItem,  out string notification, TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");
    notification = "{\"message\":\"Hello from C#. Processed a queue item!\"}";
}
```

## <a name="template-example-using-notification-hubs-library-types"></a>Vorlagenbeispiel mit Notification Hubs-Bibliothekstypen
Dieses Beispiel zeigt, wie Sie in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierte Typen verwenden. 

```cs
#r "Microsoft.Azure.NotificationHubs"

using System;
using System.Threading.Tasks;
using Microsoft.Azure.NotificationHubs;

public static void Run(string myQueueItem,  out Notification notification, TraceWriter log)
{
   log.Info($"C# Queue trigger function processed: {myQueueItem}");
   notification = GetTemplateNotification(myQueueItem);
}

private static TemplateNotification GetTemplateNotification(string message)
{
    Dictionary<string, string> templateProperties = new Dictionary<string, string>();
    templateProperties["message"] = message;
    return new TemplateNotification(templateProperties);
}
```

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]




<!--HONumber=Nov16_HO3-->


