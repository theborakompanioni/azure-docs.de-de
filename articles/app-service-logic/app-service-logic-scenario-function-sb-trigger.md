---
title: 'Logik-App-Szenario: Erstellen eines Service Bus-Triggers mithilfe von Azure Functions | Microsoft-Dokumentation'
description: "Verwenden Sie Azure Functions, um einen Service Bus-Trigger für eine Logik-App zu erstellen."
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: dwrede
editor: 
ms.assetid: 19cbd921-7071-4221-ab86-b44d0fc0ecef
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/23/2016
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3f8031d31d1291972026430e67005863579ae911


---
# <a name="logic-app-scenario-create-an-azure-service-bus-trigger-by-using-azure-functions"></a>Logik-App-Szenario: Erstellen eines Azure Service Bus-Triggers mithilfe von Azure Functions
Sie können Azure Functions verwenden, um einen Trigger für Logik-Apps zu erstellen, wenn Sie einen Listener oder Aufgaben mit langer Ausführungsdauer bereitstellen müssen. Beispielsweise können Sie eine Funktion erstellen, die an einer Warteschlange lauscht und sofort eine Logik-App als Pushtrigger auslöst.

## <a name="build-the-logic-app"></a>Erstellen der Logik-App
In diesem Beispiel führen Sie eine Funktion für jede Logik-App aus, die ausgelöst werden muss. Erstellen Sie zunächst eine Logik-App mit einem HTTP-Anforderungstrigger. Die Funktion ruft jedes Mal, wenn eine Warteschlangennachricht empfangen wird, einen Endpunkt auf.  

1. Erstellen Sie eine neue Logik-App, und wählen Sie den Trigger **Manuell – Wenn eine HTTP-Anforderung empfangen wird** aus.  
   Optional können Sie auch mithilfe eines Tools wie z.B. [jsonschema.net](http://jsonschema.net) ein JSON-Schema angeben, das mit der Warteschlangennachricht verwendet werden soll. Fügen Sie das Schema in den Trigger ein. So kann der Designer die Form der Daten verstehen und Eigenschaften leichter über den Workflow weitergeben.
2. Fügen Sie ggf. zusätzliche Schritte hinzu, die nach Empfang einer Warteschlangennachricht ausgeführt werden sollen. Beispiel: Senden einer E-Mail über Office 365.  
3. Speichern Sie die Logik-App, um die Rückruf-URL für den Trigger dieser Logik-App zu generieren. Die URL wird auf der Triggerkarte angezeigt.

![Die Rückruf-URL wird auf der Triggerkarte angezeigt.][1]

## <a name="build-the-function"></a>Erstellen der Funktion
Als Nächstes müssen Sie eine Funktion erstellen, die als Trigger fungiert und an der Warteschlange lauscht.

1. Öffnen Sie das [Azure Functions-Portal](https://functions.azure.com/signin), und wählen Sie **Neue Funktion** und die Vorlage **ServiceBusQueueTrigger – C#** aus.
   
    ![Azure Functions-Portal][2]
2. Konfigurieren Sie die Verbindung mit der Service Bus-Warteschlange (mit Verwendung des `OnMessageReceive()` -Listeners aus dem Azure Service Bus SDK).
3. Schreiben Sie eine einfache Funktion zum Aufrufen des (bereits früher erstellten) Logik-App-Endpunkts, bei der die Warteschlangennachricht als Trigger verwendet wird. Hier finden Sie ein vollständiges Beispiel für eine Funktion. Dieses Beispiel verwendet einen `application/json` -Inhaltstyp für Nachrichten, dies können Sie jedoch ändern.
   
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

Fügen Sie zum Testen mithilfe eines Tools wie [Service Bus Explorer](https://github.com/paolosalvatori/ServiceBusExplorer)eine Warteschlangennachricht hinzu. Sie sehen, dass die Logik-App ausgelöst wird, unmittelbar nachdem die Funktion die Nachricht empfängt.

<!-- Image References -->
[1]: ./media/app-service-logic-scenario-function-sb-trigger/manualTrigger.PNG
[2]: ./media/app-service-logic-scenario-function-sb-trigger/newQueueTriggerFunction.PNG



<!--HONumber=Nov16_HO3-->


