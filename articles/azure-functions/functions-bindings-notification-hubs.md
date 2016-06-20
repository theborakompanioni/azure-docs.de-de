<properties
	pageTitle="Azure Functions Notification Hub-Bindung | Microsoft Azure"
	description="Erfahren Sie, wie Azure Notification Hub-Bindungen in Azure Functions verwendet werden."
	services="functions"
	documentationCenter="na"
	authors="wesmc7777"
	manager="erikre"
	editor=""
	tags=""
	keywords="Azure Functions, Funktionen, Ereignisverarbeitung, dynamisches Compute, serverlose Architektur"/>

<tags
	ms.service="functions"
	ms.devlang="multiple"
	ms.topic="reference"
	ms.tgt_pltfrm="multiple"
	ms.workload="na"
	ms.date="05/16/2016"
	ms.author="wesmc"/>

# Azure Functions Notification Hub-Ausgabebindung

Dieser Artikel erläutert das Konfigurieren und Codieren von Azure Notification Hub-Bindungen in Azure Functions.

[AZURE.INCLUDE [Einführung](../../includes/functions-bindings-intro.md)]

Ihre Funktionen können über einen konfigurierten Azure Notification Hub mit sehr wenigen Codezeilen Pushbenachrichtigungen senden. Allerdings muss der Notification Hub für die Plattformbenachrichtigungsdienste konfiguriert sein, die Sie verwenden möchten. Weitere Informationen zum Konfigurieren eines Azure Notification Hubs und zum Entwickeln von Clientanwendungen, die sich für den Empfang von Benachrichtigungen registrieren, finden Sie unter [Erste Schritte mit Notification Hubs](../notification-hubs/notification-hubs-windows-store-dotnet-get-started.md). Klicken Sie dabei oben auf Ihre Zielclientplattform.

## „function.json“ für Azure Notification Hub-Ausgabebindung

Die Datei „function.json“ stellt die folgenden Eigenschaften bereit:

- `name`: Variablenname, der im Funktionscode für die Notification Hub-Nachricht verwendet wird.
- `type`: muss auf *"notificationHub"* festgelegt werden.
- `tagExpression`: Mit Tagausdrücken können Sie Benachrichtigungen an eine Gruppe von Geräten übermitteln lassen, die sich für den Empfang von Benachrichtigungen entsprechend dem Tagausdruck registriert haben. Weitere Informationen finden Sie unter [Weiterleitung und Tagausdrücke](../notification-hubs/notification-hubs-routing-tag-expressions.md).
- `hubName`: Name der Notification Hub-Ressource im Azure-Portal.
- `connection`: Diese Verbindungszeichenfolge muss eine Verbindungszeichenfolge für die **Anwendungseinstellung** sein, die auf den *DefaultFullSharedAccessSignature*-Wert Ihres Notifications Hubs festgelegt ist.
- `direction`: muss auf *"out"* festgelegt werden. 
 
Beispiel für „function.json“:

	{
	  "bindings": [
	    {
	      "name": "notification",
	      "type": "notificationHub",
	      "tagExpression": "",
	      "hubName": "my-notification-hub",
	      "connection": "MyHubConnectionString",
	      "direction": "out"
	    }
	  ],
	  "disabled": false
	}

## Einrichten der Azure Notification Hub-Verbindungszeichenfolge

Um eine Notification Hub-Ausgabebindung zu verwenden, müssen Sie die Verbindungszeichenfolge für den Hub konfigurieren. Dies kann auf der Registerkarte *Integrieren* erfolgen, indem Sie einfach Ihren Notification Hub auswählen oder einen neuen erstellen.

Sie können eine Verbindungszeichenfolge für einen vorhandenen Hub auch manuell hinzufügen, indem Sie Ihrem Notification Hub eine Verbindungszeichenfolge für *DefaultFullSharedAccessSignature* hinzufügen. Diese Verbindungszeichenfolge stellt Ihrer Funktion die Zugriffsberechtigungen zum Senden von Nachrichten bereit. Der Wert der *DefaultFullSharedAccessSignature*-Verbindungszeichenfolge ist über die Schaltfläche **Schlüssel** auf dem Hauptblatt der Notification Hub-Ressource im Azure-Portal zugänglich. Um eine Verbindungszeichenfolge für den Hub manuell hinzuzufügen, gehen Sie folgendermaßen vor:

1. Klicken Sie im Azure-Portal auf dem Blatt **Funktionscontainer** auf **Function App Settings (Funktionscontainer) > Go to App Service settings (Zu App Service-Einstellungen wechseln)**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Anwendungseinstellungen**.

3. Scrollen Sie nach unten zum Abschnitt **Verbindungszeichenfolgen**, und fügen Sie einen benannten Eintrag für den *DefaultFullSharedAccessSignature*-Wert für Ihren Notification Hub hinzu. Ändern Sie den Typ in **Benutzerdefiniert**.
4. Verweisen Sie in den Ausgabebindungen auf den Namen der Verbindungszeichenfolge, ähnlich wie bei **MyHubConnectionString** im obigen Beispiel.

## Azure Notification Hub-Codebeispiel für einen Node.js-Trigger mit Timer 

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md) gesendet, die `location` und `message` enthält.

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

## Azure Notification Hub-Codebeispiel für einen C#-Warteschlangentrigger

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md) gesendet, die `message` enthält.


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

In diesem Beispiel wird eine Benachrichtigung für eine [Vorlagenregistrierung](../notification-hubs/notification-hubs-templates.md), die `message` enthält, über eine gültige JSON-Zeichenfolge gesendet.

	using System;
	 
	public static void Run(string myQueueItem,  out string notification, TraceWriter log)
	{
		log.Info($"C# Queue trigger function processed: {myQueueItem}");
		notification = "{"message":"Hello from C#. Processed a queue item!"}";
	}

## C#-Codebeispiel für einen Azure Notification Hub-Warteschlangentrigger mit Benachrichtigungstyp

Dieses Beispiel zeigt, wie Sie den in der [Bibliothek für Microsoft Azure Notification Hubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/) definierten `Notification`-Typ verwenden. Um diesen Typ und die Bibliothek zu verwenden, müssen Sie die Datei *project.json* für Ihre Funktionen-App hochladen. Die Datei „project.json“ ist eine JSON-Textdatei, die in etwa folgendermaßen aussieht:

	{
	  "frameworks": {
	    ".NETFramework,Version=v4.6": {
	      "dependencies": {
	        "Microsoft.Azure.NotificationHubs": "1.0.4"
	      }
	    }
	  }
	}

Weitere Informationen zum Hochladen der Datei „project.json“ finden Sie in der [Anleitung zum Hochladen der Datei „project.json“](http://stackoverflow.com/questions/36411536/how-can-i-use-nuget-packages-in-my-azure-functions).

Beispielcode:

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

## Nächste Schritte

[AZURE.INCLUDE [Nächste Schritte](../../includes/functions-bindings-next-steps.md)]

<!---HONumber=AcomDC_0608_2016-->