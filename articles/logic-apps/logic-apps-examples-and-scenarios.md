---
title: "Beispiele und Szenarien – Azure Logic Apps | Microsoft-Dokumentation"
description: "Durcharbeiten der Logic Apps-Beispiele für häufige Szenarien"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: .net,nodejs,java
ms.assetid: e06311bc-29eb-49df-9273-1f05bbb2395c
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/14/2017
ms.author: jehollan
translationtype: Human Translation
ms.sourcegitcommit: 432752c895fca3721e78fb6eb17b5a3e5c4ca495
ms.openlocfilehash: dcf089d680249d0a2f9d748b315076d91c8c78e8
ms.lasthandoff: 03/30/2017


---
# <a name="examples-and-common-scenarios-for-azure-logic-apps"></a>Beispiele und häufige Szenarien für Logic Apps

Damit Sie mehr über die vielen Muster und Funktionen in Azure Logic Apps erfahren, finden Sie hier einige häufige Beispiele und Szenarien.

## <a name="key-scenarios-for-logic-apps"></a>Wichtige Szenarien für Logic Apps

Azure Logic Apps bietet robuste Orchestrierung und Integration für verschiedene Dienste. Der Logic Apps-Dienst arbeitet „serverlos“, daher müssen Sie sich über Skalierung oder Instanzen keine Gedanken machen – Sie brauchen lediglich den Workflow (Trigger und Aktionen) zu definieren. Die zugrundeliegende Plattform kümmert sich um Skalierung, Verfügbarkeit und Leistung. Jedes Szenario, in dem Sie mehrere Aktionen koordinieren müssen, insbesondere über mehrere Systeme hinweg, stellt einen bestens geeigneten Anwendungsfall für Azure Logic Apps dar. Hier sind einige Muster und Beispiele.

## <a name="respond-to-triggers-and-extend-actions"></a>Reagieren auf Trigger und Erweitern von Aktionen

Jede Logic App beginnt mit einem Trigger. Beispielsweise kann Ihr Workflow durch ein geplantes Ereignis, einen manuellen Aufruf oder ein Ereignis von einem externen System gestartet werden, etwa mit dem Trigger „wenn einem FTP-Server eine Datei hinzugefügt wird“. Azure Logic Apps unterstützen aktuell mehr als 100 gebrauchsfertige Connectors, die von lokalem SAP bis zu Azure Cognitive Services reichen. Für Systeme und Dienste, für die möglicherweise keine veröffentlichten Connectors verfügbar sind, können Sie Logic Apps darüber hinaus erweitern.

* [Tutorial: Erstellen eines KI-gestützten sozialen Dashboards innerhalb von Minuten mithilfe von Logic Apps und Power BI](http://aka.ms/logicappsdemo)
* [Erstellen von benutzerdefinierten Triggern oder Aktionen](../logic-apps/logic-apps-create-api-app.md)
* [Einrichten von Aktionen mit langer Ausführungszeit für Workflowausführungen](../logic-apps/logic-apps-create-api-app.md)
* [Reagieren auf externe Ereignisse und Aktionen mit Webhooks](../logic-apps/logic-apps-create-api-app.md)
* [Aufrufen, Auslösen oder Schachteln von Workflows mithilfe von synchronen Antworten auf HTTP-Anforderungen](logic-apps-http-endpoint.md)
* [Tutorial: Reagieren auf Twilio SMS-Webhooks und Senden einer Textantwort](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-logging-and-control-flow-capabilities"></a>Fehlerbehandlung, Protokollierung und Funktionen zur Ablaufsteuerung

Logic Apps enthalten umfangreiche Funktionen für die erweiterte Ablaufsteuerung, wie Bedingungen, Schalter, Schleifen und Bereiche. Um robuste Lösungen sicherzustellen, können Sie darüber hinaus in Ihren Workflows Fehler- und Ausnahmebehandlung implementieren. Für Benachrichtigungs- und Diagnoseprotokolle für den Status von Workflowausführungen stellt Azure Logic Apps ferner Überwachung und Warnungen zur Verfügung.

* [Ausführen von verschiedenen Aktionen mit switch-Anweisungen](logic-apps-switch-case.md)
* [Verarbeiten von Elementen in Arrays und Sammlungen mit Schleifen und Batches in Logic Apps](logic-apps-loops-and-scopes.md)
* [Erstellen von Fehler- und Ausnahmebehandlung in einem Workflow](logic-apps-exception-handling.md)
* [Konfigurieren von Azure-Warnungen und Diagnose](logic-apps-monitor-your-logic-apps.md)
* [Anwendungsfall: Wie ein Unternehmen im Gesundheitswesen Logic Apps-Ausnahmebehandlung für HL7 FHIR-Workflows einsetzt](logic-apps-scenario-error-and-exception-handling.md)

## <a name="deploy-and-manage-logic-apps"></a>Bereitstellen und Verwalten von Logic Apps

Sie können Logic Apps mithilfe von Visual Studio, Visual Studio Team Services oder beliebigen anderen Tools für Quellcodeverwaltung und automatische Builderstellung vollständig entwickeln und bereitstellen. Um die Entwicklung für Workflows und abhängige Verbindungen in einer Ressourcenvorlage zu unterstützen, verwendet Logic Apps Azure-Vorlagen für die Ressourcenbereitstellung. Visual Studio-Tools generieren diese Vorlagen automatisch, die Sie zur Versionsverwaltung in die Quellcodeverwaltung einchecken können.

* [Erstellen einer Vorlage zur automatisierten Bereitstellung](../logic-apps/logic-apps-create-deploy-template.md)
* [Erstellen und Bereitstellen von Logik-Apps von Visual Studio aus](logic-apps-deploy-from-vs.md)
* [Überwachen der Integrität Ihrer Logic Apps](logic-apps-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations-within-a-run"></a>Inhaltstypen, Konvertierungen und Transformationen während der Ausführung

Mithilfe der vielen Funktionen in der [Workflowdefinitionssprache](http://aka.ms/logicappsdocs) von Azure Logic Apps können Sie auf mehrere Inhaltstypen zugreifen, sie konvertieren und transformieren. Beispielsweise können Sie mit den Workflowausdrücken `@json()` und `@xml()` zwischen einer Zeichenfolge, JSON und XML konvertieren. Das Logic Apps-Modul behält Inhaltstypen bei, um die verlustfreie Übertragung von Inhalten zwischen Diensten zu unterstützen.

* [Verarbeitung von nicht als JSON vorliegenden Inhaltstypen](../logic-apps/logic-apps-content-type.md), wie `application/xml`, `application/octet-stream` und `multipart/formdata`
* [Funktionsweise von Workflowausdrücken in Logic Apps](../logic-apps/logic-apps-author-definitions.md)
* [Referenz: Azure Logic Apps-Workflowdefinitionssprache](http://aka.ms/logicappsdocs)

## <a name="other-integrations-and-capabilities"></a>Weitere Integrationen und Funktionen

Logic Apps bieten außerdem Integration mit vielen Diensten, wie etwa Azure Functions, Azure API Management, Azure App Services und benutzerdefinierten HTTP-Endpunkten, z.B. REST und SOAP.

* [Erstellen eines sozialen Dashboards in Echtzeit mit Azure Serverless](logic-apps-scenario-social-serverless.md)
* [Aufrufen von Azure Functions aus Logic-Apps](../logic-apps/logic-apps-azure-functions.md)
* [Szenario: Auslösen von Logic Apps mit Azure Functions](logic-apps-scenario-function-sb-trigger.md)
* [Blog: Call SOAP endpoints from logic apps (Aufrufen von SOAP-Endpunkten aus Logic Apps)](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)

## <a name="next-steps"></a>Nächste Schritte

- [Behandeln von Fehlern und Ausnahmen in Logic Apps](logic-apps-exception-handling.md)
- [Erstellen von Workflowdefinitionen mit der Workflowdefinitionssprache ](logic-apps-author-definitions.md)
- [Senden Sie Ihre Kommentare, Fragen, Feedback und Vorschläge für Verbesserungen an Azure-Logic Apps](https://feedback.azure.com/forums/287593-logic-apps)
