---
title: "Azure Event Grid – Übersicht"
description: "Beschreibt Azure Event Grid und die zugehörigen Konzepte."
services: event-grid
author: banisadr
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 09/11/2017
ms.author: babanisa
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 20c22bdbefe238781242ee26e648a77da02a21d2
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="an-introduction-to-azure-event-grid"></a>Einführung in Azure Event Grid

Mit Azure Event Grid können Sie mühelos Anwendungen mit ereignisbasierten Architekturen erstellen. Sie wählen die Azure-Ressource aus, die Sie abonnieren möchten, und geben den Ereignishandler oder WebHook-Endpunkt an, an den das Ereignis gesendet werden soll. Event Grid verfügt über integrierte Unterstützung für Ereignisse, die aus Azure-Diensten stammen, wie Speicherblobs und Ressourcengruppen. Event Grid bietet auch mithilfe benutzerdefinierter Themen und Webhooks benutzerdefinierte Unterstützung für Anwendungs- und Drittanbieterereignisse. 

Sie können Filter zum Weiterleiten bestimmter Ereignisse an verschiedene Endpunkte verwenden, Multicasts zu mehreren Endpunkten ausführen und sicherstellen, dass Ihre Ereignisse zuverlässig übermittelt werden. Event Grid bietet auch integrierte Unterstützung für benutzerdefinierte und Drittanbieterereignisse.

In der Vorschauversion unterstützt Event Grid die Standorte **westus2** und **westcentralus**. Andere Regionen werden hinzugefügt.

Dieser Artikel enthält eine Übersicht zu Azure Event Grid. Wenn Sie in die Verwendung von Event Grid einsteigen möchten, lesen Sie [Erstellen und Weiterleiten benutzerdefinierter Ereignisse mit Azure Event Grid](custom-event-quickstart.md).

![Event Grid – funktionales Modell](./media/overview/event-grid-functional-model.png)

Blob Storage ist derzeit nicht als Herausgeber öffentlich verfügbar. Sie müssen sich für die Vorschauversion registrieren, um auf Speicherblobereignisse reagieren zu können. Weitere Informationen finden Sie unter [Weiterleiten von Blob Storage-Ereignissen an einen benutzerdefinierten Webendpunkt (Vorschauversion)](../storage/blobs/storage-blob-event-quickstart.md?toc=%2fazure%2fevent-grid%2ftoc.json).

## <a name="concepts"></a>Konzepte

Es gibt fünf Konzepte in Azure Event Grid, die Sie nutzen können:

* **Ereignisse**: Was ist passiert?
* **Ereignisquellen/-herausgeber**: Wo das Ereignis stattgefunden hat.
* **Themen**: Der Endpunkt, an den Herausgeber Ereignisse senden.
* **Ereignisabonnements**: Der Endpunkt oder integrierte Mechanismus zum Weiterleiten von Ereignissen, und zwar mitunter zu mehreren Handlern. Abonnements werden auch vom Handler verwendet, um eingehende Ereignisse intelligent zu filtern.
* **Ereignishandler**: Die App oder der Dienst, die/der auf das Ereignis reagiert.

Weitere Informationen zu diesen Konzepten finden Sie unter [Concepts in Azure Event Grid](concepts.md) (Konzepte in Azure Event Grid).

## <a name="capabilities"></a>Funktionen

Azure Event Grid bietet unter anderem folgende zentrale Features:

* **Einfachheit**: Leiten Sie Ereignisse mit Point-and-Click aus Ihrer Azure-Ressource zu einem Ereignishandler oder Endpunkt.
* **Erweiterte Filterung**: Filtern Sie nach Ereignistyp oder Ereignisveröffentlichungspfad, um sicherzustellen, dass Ereignishandler nur relevante Ereignisse empfangen.
* **Auffächern**: Abonnieren Sie mehrere Endpunkte für das gleiche Ereignis, um Kopien des Ereignisses an so viele Stellen wie erforderlich zu senden.
* **Zuverlässigkeit**: Nutzen Sie die 24-Stunden-Wiederholung mit exponentiellem Backoff, um sicherzustellen, dass Ereignisse übermittelt werden.
* **Bezahlen pro Ereignis**: Zahlen Sie nur für den Umfang, in dem Sie Event Grid nutzen.
* **Hoher Durchsatz**: Erstellen Sie Workloads mit hohem Volumen auf Event Grid mit Unterstützung für Millionen Ereignisse pro Sekunde.
* **Integrierte Ereignisse**: Mit ressourcendefinierten integrierten Ereignissen sind Sie schnell einsatzbereit.
* **Benutzerdefinierte Ereignisse**: Nutzen Sie die Weiterleitungs- und Filterfunktionen von Event Grid, um zuverlässig benutzerdefinierte Ereignisse in Ihre App zu übertragen.

## <a name="built-in-publisher-and-handler-integration"></a>Integrierte Herausgeber- und Handlerintegration

Azure bietet integrierte Ereignisunterstützung mit zahlreichen Diensten einschließlich Herausgebern und Handlern.

### <a name="publishers"></a>Herausgeber

Derzeit bieten die folgenden Azure-Dienste integrierte Herausgeberunterstützung für Event Grid:

* Ressourcengruppen (Verwaltungsvorgänge)
* Azure-Abonnements (Verwaltungsvorgänge)
* Event Hubs
* Benutzerdefinierte Themen

Weitere Azure-Dienste werden dieses Jahr hinzugefügt.

### <a name="handlers"></a>Handler

Derzeit bieten die folgenden Azure-Dienste integrierte Handlerunterstützung für Event Grid: 

* Azure-Funktionen
* Logik-Apps
* Azure-Automatisierung
* WebHooks

Weitere Azure-Dienste werden dieses Jahr hinzugefügt.

## <a name="what-can-i-do-with-event-grid"></a>Welche Möglichkeiten bietet Event Grid?

Azure Event Grid bietet verschiedene Funktionen, die die serverlose, Betriebsautomatisierungs- und Integrationsarbeit erheblich verbessern: 

### <a name="serverless-application-architectures"></a>Serverlose Anwendungsarchitekturen

![Serverlose Anwendung](./media/overview/serverless_web_app.png)

Event Grid verknüpft Datenquellen und Ereignishandler. Verwenden Sie Event Grid z.B. zum automatischen Auslösen einer serverlosen Funktion, um jedes Mal, wenn ein neues Foto einem Blob Storage-Container hinzugefügt wird, eine Bildanalyse durchzuführen. 

### <a name="ops-automation"></a>Betriebsautomatisierung

![Betriebsautomatisierung](./media/overview/Ops_automation.png)

Mit Event Grid können Sie die Automatisierung beschleunigen und die Richtlinienerzwingung vereinfachen. Event Grid kann z.B. Azure Automation benachrichtigen, wenn ein virtueller Computer erstellt oder eine SQL-Datenbank eingerichtet wird. Diese Ereignisse können verwendet werden, um automatisch die Kompatibilität von Dienstkonfigurationen zu überprüfen, Metadaten in Betriebstools einzulesen, virtuelle Computer zu kennzeichnen oder Arbeitselemente abzulegen.

### <a name="application-integration"></a>Anwendungsintegration

![Anwendungsintegration](./media/overview/app_integration.png)

Event Grid verknüpft Ihre App mit anderen Diensten. Sie können z.B. ein benutzerdefiniertes Thema erstellen, um die Ereignisdaten Ihrer App an Event Grid zu senden und von der zuverlässigen Übermittlung, den erweiterten Routingfunktionen und der direkten Integration in Azure zu profitieren. Alternativ dazu können Sie Event Grid mit Logic Apps verwenden, um Daten überall verarbeiten zu können, ohne Code schreiben zu müssen. 

## <a name="how-is-event-grid-different-from-other-azure-integration-services"></a>Wie unterscheidet sich Event Grid von anderen Azure-Integrationsdiensten?

Event Grid ist ein Ereignisbackplane, das ereignisgesteuerte, reaktive Programmierung ermöglicht. Es ist tief in Azure-Diensten integriert und kann in Drittanbieterdienste integriert werden. Die Ereignismeldung enthält die Informationen, die Sie benötigen, um auf Änderungen in Diensten und Anwendungen zu reagieren. Event Grid ist keine Datenpipeline und übermittelt nicht das eigentliche Objekt, das aktualisiert wurde.

Service Bus eignet sich gut für herkömmliche Unternehmensanwendungen, die Transaktionen, Sortieren, Duplikaterkennung und sofortige Konsistenz erfordern. Event Grid ist für Geschwindigkeit, Skalierung, Bandbreite und niedrige Kosten in einem reaktiven Modell konzipiert. Es eignet sich gut für serverlose Architektur.

Event Grid ergänzt andere Azure-Dienste wie Logic Apps und Event Hubs. Event Grid löst die Logik-App aus, um seinen Workflow zu starten. Event Hubs und Event Grid ermöglichen Ihnen gemeinsam, auf Ereignisse von Event Hubs Capture zu reagieren und Datenerfassungs- und -transformationspipelines zu erstellen.

## <a name="how-much-does-event-grid-cost"></a>Was kostet Event Grid?

Das Preismodell von Azure Event Grid orientiert sich an einer Zahlung pro Ereignis, sodass Sie nur für die tatsächliche Verwendung zahlen.

Event Grid kostet 0,60 $ pro Million Vorgänge ($0,30 während der Vorschau), und die ersten 100.000 Vorgänge pro Monat sind kostenlos. Vorgänge sind definiert als Ereigniseingänge, erweiterte Übereinstimmungen, Übermittlungsversuche und Verwaltungsaufrufe.  Weitere Einzelheiten finden Sie auf der [Preisseite](https://azure.microsoft.com/pricing/details/event-grid/).

## <a name="next-steps"></a>Nächste Schritte

* [Erstellen und Abonnieren von benutzerdefinierten Ereignissen](custom-event-quickstart.md)  
  Starten Sie sofort, und senden Sie Ihre eigenen benutzerdefinierten Ereignisse mithilfe des Azure Event Grid-Schnellstarts an einen beliebigen Endpunkt.
* [Verwenden von Logic Apps als Ereignishandler](monitor-virtual-machine-changes-event-grid-logic-app.md)  
  Ein Tutorial zum Erstellen einer App mit Logic Apps, um auf Ereignisse zu reagieren, die von Event Grid mithilfe von Push übertragen werden.
* [Streamen von Big Data in ein Data Warehouse](event-grid-event-hubs-integration.md)  
  Ein Tutorial, das mithilfe von Azure Functions Daten von Event Hubs in SQL Data Warehouse streamt.
* [Event Grid-REST-API-Referenz](/rest/api/eventgrid)  
  Bietet weitere technische Informationen zu Event Grid Azure und einen Verweis zum Verwalten von Ereignisabonnements, zu Routing und Filterung.
