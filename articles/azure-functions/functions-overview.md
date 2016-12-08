---
title: "Übersicht zu Azure Functions | Microsoft Docs"
description: Hier erfahren Sie, wie Sie mithilfe von Azure Functions in wenigen Minuten asynchrone Workloads optimieren.
services: functions
documentationcenter: na
author: mattchenderson
manager: erikre
editor: 
tags: 
keywords: Azure Functions, Functions, Ereignisverarbeitung, Webhooks, dynamisches Compute, serverlose Architektur
ms.assetid: 01d6ca9f-ca3f-44fa-b0b9-7ffee115acd4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/20/2016
ms.author: cfowler;mahender;glenga
translationtype: Human Translation
ms.sourcegitcommit: 31d7a8d1abb59e096a657c4a74743adfc0d137b6
ms.openlocfilehash: 01c1e7b58a7dce3c640ac151b3ac708ce2b7d913


---
# <a name="azure-functions-overview"></a>Übersicht zu Azure Functions
Azure Functions ist eine Lösung, mit der Sie ganz einfach kleinere Codeelemente (Funktionen) in der Cloud ausführen können. Sie können sich auf das Schreiben des Codes beschränken, der für das jeweilige Problem erforderlich ist, ohne sich über eine gesamte Anwendung oder die Infrastruktur für ihre Ausführung Gedanken machen zu müssen. Mit Functions können Entwickler noch produktiver arbeiten und ihre bevorzugte Programmiersprache verwenden (z.B. C#, F#, Node.js, Python oder PHP). Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird, und setzen Sie auf die flexiblen Möglichkeiten zur Skalierung von Azure.

Dieses Thema bietet einen allgemeinen Überblick über Azure Functions. Unter [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)können Sie sich direkt mit der Verwendung von Azure Functions vertraut machen. Technische Informationen zu Functions finden Sie in der [Entwicklerreferenz](functions-reference.md).

## <a name="features"></a>Features
Azure Functions bietet unter anderem folgende zentrale Features:

* **Freie Sprachwahl** : Schreiben Sie Funktionen mit C#, F#, Node.js, Python, PHP, Batch, Bash oder einem beliebigen ausführbaren Programm.
* **Preismodell mit nutzungsbasierter Bezahlung** : Bezahlen Sie nur für die Zeit, in der Ihr Code ausgeführt wird. Informationen hierzu finden Sie unter der Hostingoption „Verbrauchstarif“ im [Preisabschnitt](#pricing).  
* **Eigene Abhängigkeiten** : Functions unterstützt NuGet und NPM, sodass Sie Ihre bevorzugten Bibliotheken verwenden können.  
* **Integrierte Sicherheit** : Schützen Sie per HTTP ausgelöste Funktionen mit OAuth-Anbietern wie Azure Active Directory, Facebook, Google, Twitter und Microsoft-Konto.  
* **Vereinfachte Integration** : Profitieren Sie von der einfachen Nutzung von Azure-Diensten und SaaS-Angeboten (Software-as-a-Service). Beispiele finden Sie im Abschnitt [Integrationen](#integrations).  
* **Flexible Entwicklung** : Programmieren Sie Ihre Funktionen direkt im Portal, oder richten Sie Continuous Integration ein, und stellen Sie Ihren Code über GitHub, Visual Studio Team Services und andere [unterstützte Entwicklungstools](../app-service-web/web-sites-deploy.md#deploy-using-an-ide)bereit.  
* **Open Source** : Die Laufzeit von Functions ist eine Open-Source-Anwendung und [auf GitHub verfügbar](https://github.com/azure/azure-webjobs-sdk-script).  

## <a name="what-can-i-do-with-functions"></a>Welche Möglichkeiten bestehen mit Functions?
Azure Functions ist eine hervorragende Lösung zum Verarbeiten von Daten, Integrieren von Systemen, Arbeiten mit dem Internet der Dinge (Internet of Things, IoT) und Erstellen einfacher APIs und Microservices. Functions eignet sich für Aufgaben wie Bild- oder Auftragsverarbeitung, Dateiwartung, Aufgaben mit langer Ausführungsdauer, die in einem Hintergrundthread ausgeführt werden sollen, sowie für jegliche zeitplanbasierte Aufgabe. 

In Functions stehen unter anderem folgende Vorlagen für zentrale Szenarien zur Verfügung:

* **BlobTrigger** : Dient zum Verarbeiten von Azure Storage-Blobs beim Hinzufügen zu Containern. Sie können diese Funktion beispielsweise zum Anpassen der Größe von Bildern verwenden.
* **EventHubTrigger**: Reagiert auf Ereignisse, die an einen Azure Event Hub übermittelt werden. Dies ist besonders für Anwendungsinstrumentierung, Benutzeroberflächen oder Workflowverarbeitung sowie für Internet of Things (IoT)-Szenarios geeignet.
* **Generic webhook** (Generischer Webhook): Dient zum Verarbeiten von Webhook-HTTP-Anforderungen von einem beliebigen Dienst mit Webhook-Unterstützung.
* **GitHub webhook** (GitHub-Webhook): Dient zum Reagieren auf Ereignisse in Ihren GitHub-Repositorys. Ein Beispiel finden Sie unter [Erstellen eines Webhooks oder einer API Azure Functions-Funktion](functions-create-a-web-hook-or-api-function.md).
* **HTTPTrigger** : Dient zum Auslösen der Ausführung Ihres Codes mithilfe einer HTTP-Anforderung.
* **QueueTrigger** : Dient zum Reagieren auf eingehende Meldungen der Azure Storage-Warteschlange. Ein Beispiel finden Sie unter [Erstellen einer Azure Functions-Funktion zum Erstellen einer Bindung an einen Azure-Dienst](functions-create-an-azure-connected-function.md).
* **ServiceBusQueueTrigger** : Dient zum Verknüpfen Ihres Codes mit anderen Azure-Diensten oder lokalen Diensten durch Lauschen auf Meldungswarteschlangen. 
* **ServiceBusTopicTrigger** : Dient zum Verknüpfen Ihres Codes mit anderen Azure-Diensten oder lokalen Diensten durch Abonnieren von Themen. 
* **TimerTrigger** : Dient zum Ausführen einer Bereinigung oder anderer Batch-Tasks nach einem vordefinierten Zeitplan. Ein Beispiel finden Sie unter [Erstellen einer Ereignisverarbeitungsfunktion](functions-create-an-event-processing-function.md).

Azure Functions unterstützt *Trigger* zum Starten der Codeausführung sowie *Bindungen* zur Vereinfachung der Programmierung für Eingabe- und Ausgabedaten. Eine ausführliche Beschreibung der Trigger und Bindungen von Azure Functions finden Sie in der [Entwicklerreferenz zu Triggern und Bindungen in Azure Functions](functions-triggers-bindings.md).

## <a name="a-nameintegrationsaintegrations"></a><a name="integrations"></a>Integrationen
Azure Functions kann in eine Reihe von Azure- und Drittanbieterdiensten integriert werden. Diese können Sie zum Auslösen Ihrer Funktion und zum Starten der Ausführung oder als Ein- und Ausgabe für Ihren Code verwenden. Azure Functions unterstützt folgende Dienstintegrationen: 

* Azure DocumentDB
* Azure Event Hubs 
* Azure Mobile Apps (Tabellen)
* Azure Notification Hubs
* Azure Service Bus (Warteschlangen und Themen)
* Azure Storage (Blob, Warteschlangen und Tabellen) 
* GitHub (Webhooks)
* Lokal (mit Service Bus)

## <a name="a-namepricingahow-much-does-functions-cost"></a><a name="pricing"></a>Was kostet Functions?
Bei Azure Functions haben Sie die Wahl zwischen zwei Tarifplänen: 

* **Verbrauchstarif für**: Wenn Ihre Funktion ausgeführt wird, stellt Azure die erforderlichen Verarbeitungsressourcen bereit. Sie müssen sich nicht um die Ressourcenverwaltung kümmern und bezahlen nur für die Zeit, in der Ihr Code ausgeführt wird. Ausführliche Preisinformationen finden Sie auf der Seite [Functions – Preise](/pricing/details/functions). 
* **App Service-Tarif** : Funktionen werden auf die gleiche Weise ausgeführt wie mobile Apps und Web-/API-Apps. Wenn Sie App Service bereits für Ihre anderen Anwendungen verwenden, können Sie Ihre Funktionen ohne zusätzliche Kosten unter dem gleichen Tarif ausführen. Ausführliche Informationen finden Sie auf der Seite mit der [App Service-Preisübersicht](/pricing/details/app-service/).

Weitere Informationen zum Skalieren Ihrer Funktionen finden Sie unter [Skalieren von Azure Functions](functions-scale.md).

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen Sie Ihre erste Funktion in Azure Functions](functions-create-first-azure-function.md)  
   Legen Sie direkt los, und erstellen Sie Ihre erste Funktion mithilfe der Azure Functions-Schnellstartanleitung. 
* [Entwicklerreferenz zu Azure Functions](functions-reference.md)  
   Enthält weitere technische Informationen zur Azure Functions-Laufzeit sowie eine Referenz für das Programmieren von Funktionen sowie für das Festlegen von Triggern und Bindungen.
* [Testen von Azure Functions](functions-test-a-function.md)  
   Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
* [How to scale Azure Functions (Skalieren von Azure Functions) (Skalieren von Azure Functions)](functions-scale.md)  
  Beschreibt die für Azure Functions verfügbaren Servicepläne (einschließlich des Hostingplans „Verbrauchstarif“) und enthält Informationen zur Wahl des geeigneten Plans. 
* [Was ist Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
   Azure Functions nutzt die Azure App Service-Plattform für Kernfunktionen wie Bereitstellungen, Umgebungsvariablen und Diagnosen. 




<!--HONumber=Nov16_HO4-->


