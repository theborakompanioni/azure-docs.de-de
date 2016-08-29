<properties
   pageTitle="Beispiele und Szenarios für Logik-Apps | Microsoft Azure"
   description="Hier finden Sie Informationen zu häufig verwendeten Logik-Apps und zum Implementieren verschiedener Szenarios."
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="08/11/2016"
   ms.author="jehollan"/>

# Beispiele und häufige Szenarios für Logik-Apps

In diesem Artikel werden häufige Szenarios und Beispiele für die Verwendung von Logik-Apps zum Automatisieren von Geschäftsprozessen erläutert.

## Benutzerdefinierte Trigger und Aktionen

Sie können eine Logik-App auf verschiedene Weise über eine andere App auslösen. Hier sehen Sie ein paar allgemeine Beispiele:

- [Erstellen eines benutzerdefinierten Triggers oder einer benutzerdefinierten Aktion](app-service-logic-create-api-app.md)
- [Aktionen mit langer Ausführungszeit](app-service-logic-create-api-app.md)
- [HTTP-Anforderungstrigger (POST)](app-service-logic-http-endpoint.md)
- [Webhooktrigger und -aktionen](app-service-logic-create-api-app.md)
- [Abfragetrigger](app-service-logic-create-api-app.md)

### Szenarios

- [Anfordern synchroner Antworten](app-service-logic-http-endpoint.md)
- [Anfordern von Antworten mit SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## Fehlerbehandlung und Protokollierung

- [Ausnahme- und Fehlerbehandlung](app-service-logic-exception-handling.md)
- [Konfigurieren von Azure-Warnungen und Diagnose](app-service-logic-monitor-your-logic-apps.md)

### Szenarios

- [Anwendungsfall: Fehler- und Ausnahmebehandlung](app-service-logic-scenario-error-and-exception-handling.md)

## Bereitstellen und Verwalten

- [Erstellen einer automatisierten Bereitstellung](app-service-logic-create-deploy-template.md)
- [Erstellen und Bereitstellen von Logik-Apps von Visual Studio aus](app-service-logic-deploy-from-vs.md)
- [Überwachen von Logik-Apps](app-service-logic-monitor-your-logic-apps.md)

## Inhaltstypen, Konvertierungen und Transformationen

Die [Definitionssprache für Workflows](http://aka.ms/logicappsdocs) in Logic Apps enthält viele Funktionen zum Konvertieren von und Arbeiten mit unterschiedlichen Inhaltstypen. Darüber hinaus versucht das Modul, möglichst alle Inhaltstypen während des Workflows als Datenflüsse beizubehalten.

- [Behandeln von Inhaltstypen](app-service-logic-content-type.md) wie „application/json“, „application/xml“ und „plain/text“
- [Erstellen von Logik-App-Definitionen](app-service-logic-author-definitions.md)
- [Workflow Definition Language Reference (Referenz zur Definitionssprache für Workflows)](http://aka.ms/logicappsdocs)

## Batches und Schleifen

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Until](app-service-logic-loops-and-scopes.md)

## Integration von Azure Functions

- [Integrieren von Azure Functions](app-service-logic-azure-functions.md)

### Szenarios

- [Azure Function as a Service Bus Trigger (Azure-Funktion als Service Bus-Trigger)](app-service-logic-scenario-function-sb-trigger.md)

## HTTP, REST und SOAP

 - [Calling SOAP (Aufrufen von SOAP)](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Wir werden diesem Artikel weitere Beispiele und Szenarios hinzufügen. Im Bereich „Kommentare“ unten können Sie uns Wünsche zu Beispielen oder Szenarios mitteilen.

<!---HONumber=AcomDC_0817_2016-->