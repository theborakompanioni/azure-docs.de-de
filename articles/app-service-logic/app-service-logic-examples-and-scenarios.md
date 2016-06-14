<properties
   pageTitle="Beispiele und Szenarios für Logik-Apps | Microsoft Azure"
   description="Hier finden Sie Informationen zu häufig verwendeten Logik-Apps und zum Implementieren verschiedener Szenarios."
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="msftman"
   manager="erikre"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="04/25/2016"
   ms.author="deonhe"/>

# Beispiele und häufige Szenarios für Logik-Apps

In diesem Artikel werden häufige Szenarios und Beispiele für die Verwendung von Logik-Apps zum Automatisieren von Geschäftsprozessen erläutert.

## Auslösen einer Logik-App

Sie können eine Logik-App auf verschiedene Weise über eine andere App auslösen. Hier einige Beispiele:

- [HTTP-Anforderung (POST)](app-service-logic-http-endpoint.md)
- [Empfangen eines Webhooks](app-service-logic-create-api-app.md)
- [Abrufen eines Endpunkts](app-service-logic-create-api-app.md)

### Szenarios

- [Logik-Apps als aufrufbare Endpunkte](app-service-logic-http-endpoint.md)

## Benutzerdefinierte und lang ausgeführte Aktionen

- [Erstellen einer benutzerdefinierten Aktion](app-service-logic-create-api-app.md)

## Bereitstellen und Verwalten von Logik-Apps

- [Create a Logic App Deployment Template](app-service-logic-create-deploy-template.md) (Erstellen einer Bereitstellungsvorlage für Logik-Apps)
- [Diagnosing Issues with a Logic App](app-service-logic-diagnosing-failures.md) (Diagnostizieren von Problemen mit Logik-Apps)
- [Bereitstellen einer Logik-App von Visual Studio aus](app-service-logic-deploy-from-vs.md)
- [Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)

## Inhaltstypen, Konvertierungen und Transformationen

Die [Definitionssprache für Workflows](http://aka.ms/logicappsdocs) in Logik-Apps enthält viele Funktionen zum Konvertieren von und Arbeiten mit unterschiedlichen Inhaltstypen. Darüber hinaus versucht das Modul, möglichst alle Inhaltstypen während des Workflows als Datenflüsse beizubehalten.

- [Behandeln von Inhaltstypen](app-service-logic-content-type.md) wie „application/json“, „application/xml“ und „plain/text“
- [Erstellen von Logik-App-Definitionen](app-service-logic-author-definitions.md)
- [Workflow Definition Language Reference](http://aka.ms/logicappsdocs) (Referenz zur Definitionssprache für Workflows)

## Batches und Schleifen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Until](app-service-logic-loops-and-scopes.md)

## Integration von Azure Functions

- [Integrieren von Azure Functions](app-service-logic-azure-functions.md)

### Szenarios

- [Azure Function as a Service Bus Trigger](app-service-logic-scenario-function-sb-trigger.md) (Azure-Funktion als Service Bus-Trigger)

## HTTP, REST und SOAP

 - [Calling SOAP (Aufrufen von SOAP)](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Wir werden diesem Artikel weitere Beispiele und Szenarios hinzufügen. Im Bereich „Kommentare“ unten können Sie uns Wünsche zu Beispielen oder Szenarios mitteilen.

<!---HONumber=AcomDC_0601_2016-->