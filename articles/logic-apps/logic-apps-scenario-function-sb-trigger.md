---
title: "Szenario – Auslösen von Logik-Apps mit Azure Functions und Azure Service Bus | Microsoft-Dokumentation"
description: "Erstellen einer Funktion zum Auslösen einer Logik-App mithilfe von Azure Functions und Azure Service Bus"
services: logic-apps,functions
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
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
ms.sourcegitcommit: 98c78d84f3a615fae7d6785994f0db20f7a53254
ms.openlocfilehash: 013e3d29694a8daf1481e513c9c4dfc6b5da3384


---
# <a name="scenario-trigger-a-logic-app-with-azure-functions-and-azure-service-bus"></a>Szenario – Auslösen einer Logik-App mit Azure Functions und Azure Service Bus

Sie können Azure Functions verwenden, um einen Trigger für Logik-Apps zu erstellen, wenn Sie einen Listener oder Aufgaben mit langer Ausführungsdauer bereitstellen müssen. Beispielsweise können Sie eine Funktion erstellen, die an einer Warteschlange lauscht und sofort eine Logik-App als Pushtrigger auslöst.

## <a name="build-the-logic-app"></a>Erstellen der Logik-App
In diesem Beispiel führen Sie eine Funktion für jede Logik-App aus, die ausgelöst werden muss. Erstellen Sie zunächst eine Logik-App mit einem HTTP-Anforderungstrigger. Die Funktion ruft jedes Mal, wenn eine Warteschlangennachricht empfangen wird, einen Endpunkt auf.  

1. Erstellen Sie eine Logik-App.
2. Wählen Sie den Auslöser **Manuell – Wenn eine HTTP-Anforderung empfangen wird**.
   Optional können Sie auch mithilfe eines Tools wie z.B. [jsonschema.net](http://jsonschema.net) ein JSON-Schema angeben, das mit der Warteschlangennachricht verwendet werden soll. Fügen Sie das Schema in den Trigger ein. Anhand von Schemas kann der Designer die Form der Daten verstehen und Eigenschaften leichter über den Workflow weitergeben.
2. Fügen Sie ggf. zusätzliche Schritte hinzu, die nach Empfang einer Warteschlangennachricht ausgeführt werden sollen. Beispiel: Senden einer E-Mail über Office 365.  
3. Speichern Sie die Logik-App, um die Rückruf-URL für den Trigger dieser Logik-App zu generieren. Die URL wird auf der Triggerkarte angezeigt.

![Die Rückruf-URL wird auf der Triggerkarte angezeigt.][1]

## <a name="build-the-function"></a>Erstellen der Funktion
Als Nächstes müssen Sie eine Funktion erstellen, die als Trigger fungiert und an der Warteschlange lauscht.

1. Öffnen Sie das [Azure Functions-Portal](https://functions.azure.com/signin), und wählen Sie **Neue Funktion** und die Vorlage **ServiceBusQueueTrigger – C#** aus.
   
    ![Azure Functions-Portal][2]
2. Konfigurieren Sie die Verbindung mit der Service Bus-Warteschlange, die den `OnMessageReceive()`-Listener aus dem Azure Service Bus SDK verwendet.
3. Schreiben Sie eine einfache Funktion zum Aufrufen des (bereits früher erstellten) Logik-App-Endpunkts, bei der die Warteschlangennachricht als Trigger verwendet wird. Hier finden Sie ein vollständiges Beispiel für eine Funktion. Dieses Beispiel verwendet einen `application/json`-Inhaltstyp für Nachrichten, dies können Sie jedoch bei Bedarf ändern.
   
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
[1]: ./media/logic-apps-scenario-function-sb-trigger/manualtrigger.png
[2]: ./media/logic-apps-scenario-function-sb-trigger/newqueuetriggerfunction.png



<!--HONumber=Feb17_HO2-->


