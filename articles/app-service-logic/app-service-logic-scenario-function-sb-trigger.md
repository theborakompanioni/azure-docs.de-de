<properties
   pageTitle="Logik-App-Szenario: Azure-Funktion als Service Bus-Trigger | Microsoft Azure"
   description="Es wird beschrieben, wie Sie Azure-Funktionen (Azure Functions) als Service Bus-Trigger für Logik-Apps verwenden."
   services="app-service\logic,functions"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="05/23/2016"
   ms.author="jehollan"/>
   
# Logik-App-Szenario: Azure-Funktion als Service Bus-Trigger

Azure-Funktionen (Azure Functions) können als Trigger für Logik-Apps dienen, wenn Listener oder Aufgaben mit langer Ausführungsdauer bereitgestellt werden müssen. Beispielsweise können Sie eine Azure-Funktion erstellen, die eine Warteschlange überwacht und sofort eine Logik-App als Pushtrigger auslöst.

## Erstellen der Logik-App

In diesem Beispiel führen Sie eine Funktion für jede Logik-App aus, die ausgelöst werden muss. Sie müssen also zuerst eine Logik-App mit einem HTTP-Anforderungstrigger erstellen. Die Azure-Funktion ruft diesen Endpunkt jeweils auf, wenn eine Warteschlangennachricht empfangen wird.

1. Öffnen Sie eine neue Logik-App, und wählen Sie den Trigger **Manuell – Wenn eine HTTP-Anforderung empfangen wird** aus.  
    * Sie können optional ein JSON-Schema, über das die Warteschlangennachricht verfügt, mit einem Tool wie [jsonschema.net](http://jsonschema.net) angeben und in den Trigger einfügen. So kann der Designer die Form der Daten verstehen und Eigenschaften leicht über den Workflow weitergeben.
1. Fügen Sie beliebige Schritte hinzu, die nach dem Empfang einer Warteschlangennachricht ausgeführt werden sollen. Beispielsweise können Sie eine E-Mail-Nachricht über Office 365 senden.  
1. Speichern Sie die Logik-App, um die Rückruf-URL für den Trigger dieser Logik-App zu generieren. Die URL wird auf der Triggerkarte angezeigt.

![][1]

## Erstellen der Azure-Funktion

Als Nächstes müssen Sie eine Azure-Funktion erstellen, die als Trigger fungiert und die Warteschlange überwacht.

1. Öffnen Sie das [Azure Functions-Portal](https://functions.azure.com/signin), und wählen Sie **Neue Funktion** mit der Vorlage **ServiceBusQueueTrigger – C#** aus.

    ![][2]

2. Konfigurieren Sie die Verbindung mit der Service Bus-Warteschlange (mit Verwendung des `OnMessageReceive()`-Listeners aus dem Service Bus SDK).
3. Schreiben Sie eine einfache Funktion zum Aufrufen des Logik-App-Endpunkts (oben) mit der Warteschlangennachricht. Unten ist ein vollständiges Beispiel für eine Funktion mit dem Nachrichteninhaltstyp `application/json` angegeben. Dies kann bei Bedarf aber geändert werden.

   ```
   using System;
   using System.Threading.Tasks;
   using System.Net.Http;
   using System.Text;
   
   private static string logicAppUri = @"https://prod-05.westus.logic.azure.com:443/.........";
   
   public static void Run(string myQueueItem, TraceWriter log)
   {
       
       log.Info($"C# ServiceBus queue trigger function processed message: {myQueueItem}");
       using (var client = new HttpClient())
       {
           var response = client.PostAsync(logicAppUri, new StringContent(myQueueItem, Encoding.UTF8, "application/json")).Result;
       }
   }
   ```

Sie können einen Test durchführen, indem Sie eine Warteschlangennachricht mit einem Tool wie [Service Bus-Explorer](https://github.com/paolosalvatori/ServiceBusExplorer) hinzufügen und verfolgen, wie die Logik-App direkt nach dem Eingang der Nachricht bei der Funktion ausgelöst wird.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG

<!---HONumber=AcomDC_0601_2016-->