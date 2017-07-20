---
title: Verschieben von Apps aus BizTalk Services nach Azure Logic Apps | Microsoft-Dokumentation
description: Verschieben oder Migrieren von Azure BizTalk Services MABS zu Logic Apps
services: logic-apps
documentationcenter: 
author: jonfancey
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: ladocs; jonfan; mandia
ms.translationtype: Human Translation
ms.sourcegitcommit: 5edc47e03ca9319ba2e3285600703d759963e1f3
ms.openlocfilehash: e58c6950d1d9420f32fc98ca917216dc5fae4fc3
ms.contentlocale: de-de
ms.lasthandoff: 05/31/2017


---

# <a name="move-from-biztalk-services-to-logic-apps"></a>Verschieben von BizTalk Services nach Logic Apps

Microsoft Azure BizTalk Services (MABS) läuft demnächst aus. Verwenden Sie die Informationen in diesem Thema, um Ihre MABS-Integrationslösungen nach Azure Logic Apps zu verschieben. 

## <a name="overview"></a>Übersicht

BizTalk Services besteht aus zwei untergeordneten Diensten:

1.    Microsoft BizTalk Services Hybrid Connections
2.    Auf EAI- und EDI-Bridge basierende Integration

Wenn Sie Hybridverbindungen verschieben möchten, finden Sie in [Azure App Service-Hybridverbindungen](../app-service/app-service-hybrid-connections.md) Informationen zu den Änderungen und Features dieses Diensts. Azure Hybrid Connections ersetzt BizTalk Services Hybrid Connections. Azure Hybrid Connections ist über Azure App Service verfügbar und wird im Azure-Portal angeboten. Azure Hybrid Connections bietet auch einen neuen Hybrid Connection Manager zum Verwalten vorhandener BizTalk Services-Hybridverbindungen und neuer Hybridverbindungen, die Sie im Portal erstellen. Azure App Service Hybrid Connections ist allgemein verfügbar (GA).

Für die EAI- und EDI-Bridge-basierte Integration stellt Logic Apps den Ersatz dar. Logic Apps bietet die gleichen Funktionen wie BizTalk Services und noch viele mehr. Logic Apps bietet Features für verbrauchsbasierte Workflows auf Cloudebene und die Orchestrierung, mit denen Sie schnell und einfach komplexe Integrationslösungen mithilfe eines Browsers oder von Tools in Visual Studio erstellen können.

Die folgende Tabelle enthält eine Gegenüberstellung der BizTalk Services-Funktionen zu Logic Apps.

| BizTalk Services   | Logik-Apps            | Zweck                  |
| ------------------ | --------------------- | ---------------------------- |
| Connector          | Connector             | Senden und Empfangen von Daten   |
| Bridge             | Logik-App             | Pipelineprozessor           |
| Überprüfungsphase     | XML-Überprüfungsaktion      | Überprüfen eines XML-Dokuments anhand eines Schemas             |
| Erweiterungsphase       | Datentoken      | Hochstufen von Eigenschaften in Nachrichten oder für Routingentscheidungen             |
| Transformationsphase    | Transformationsaktion      | Konvertieren von XML-Nachrichten von einem Format in ein anderes             |
| Decodierungsphase       | Flatfile-Decodierungsaktion      | Konvertieren von Flatfile-Dateien nach XML             |
| Codierungsphase       |  Flatfile-Codierungsaktion      | Konvertieren von XML nach Flatfile             |
| Nachrichteninspektor       |  Azure Functions oder API-Apps      | Ausführen von benutzerdefiniertem Code in Ihren Integrationen             |
| Routingaktion      |  Bedingung oder Switch      | Weiterleiten von Nachrichten an einen der angegebenen Connectors             |

Es gibt eine Reihe verschiedener Typen von Artefakten in BizTalk Services.

## <a name="connectors"></a>Connectors
Connectors in BizTalk Services erlauben das Senden und Empfangen von Daten über Bridges. Dazu gehören auch bidirektionale Bridges, die HTTP-basierte Anforderungs-/Antwort-Interaktionen ermöglichen. In Logic Apps wird dieselbe Terminologie verwendet. Connectors in Logic Apps dienen demselben Zweck und umfassen ebenfalls mehr als 140, die eine Verbindung mit einer umfangreichen Zahl von Technologien und Diensten herstellen können – lokal mithilfe eines lokalen Datengateways (als Ersatz für den BizTalk Adapter Service von BizTalk Services) und auch als SaaS- und PaaS-Dienste in der Cloud wie OneDrive, Office365, Dynamics CRM und vieles mehr.

Die Quellen in BizTalk Services sind auf FTP, SFTP und das Abonnement für Service Bus-Warteschlangen oder -Themen beschränkt.

![](media/logic-apps-move-from-mabs/sources.png)

Jede Bridge verfügt standardmäßig über einen HTTP-Endpunkt, der mit der Laufzeitadresse und den relativen Adresseigenschaften der Bridge konfiguriert ist. Um dasselbe in Logic Apps zu erreichen, verwenden Sie die Aktionen [Anforderung und Antwort](../connectors/connectors-native-reqres.md).

## <a name="xml-processing-and-bridges"></a>XML-Verarbeitung und Bridges
Eine Bridge in BizTalk Services entspricht einer Verarbeitungspipeline. Eine Bridge kann die von einem Connector empfangenen Daten verarbeiten und dann an ein anderes System senden. Logic Apps ermöglicht dasselbe durch die Unterstützung derselben pipelinebasierten Interaktionsmuster wie BizTalk Services. Darüber hinaus wird noch eine Reihe anderer Integrationsmuster bereitgestellt. Die [XML-Anforderungs-/Antwort-Bridge](https://msdn.microsoft.com/library/azure/hh689781.aspx) in BizTalk Services wird als VETER-Pipeline bezeichnet und umfasst die folgenden Phasen:

* Überprüfen (Validate – V)
* Erweitern (E)
* Transformieren (T)
* Erweitern (E)
* Routing (R)

Wie Sie in der folgenden Abbildung sehen können, wird die Verarbeitung zwischen Anforderung und Antwort aufgeteilt. Dies ermöglicht die getrennte Steuerung der Anforderungs- und Antwortpfade (z.B. mit unterschiedlichen Zuordnungen):

![](media/logic-apps-move-from-mabs/xml-request-reply.png)

Darüber hinaus fügt eine unidirektionale XML-Bridge am Anfang und Ende der Verarbeitung Decodierungs- und Codierungsphasen hinzu, und die Pass-Through-Bridge umfasst einer einzelne Erweiterungsphase.

### <a name="message-processing-and-decodingencoding"></a>Nachrichtenverarbeitung und Codierung/Decodierung
In BizTalk Services empfangen Sie XML-Nachrichten verschiedener Typen und bestimmen das passende Schema für die empfangene Nachricht. Dies erfolgt in der Phase **Nachrichtentypen** der Empfangsverarbeitungs-Pipeline. Anschließend verwendet die Decodierungsphase den ermittelten Nachrichtentyp, um die Nachricht mithilfe des bereitgestellten Schemas zu decodieren. Wenn das Schema ein Flatfile-Schema ist, wird die eingehende Flatfile nach XML konvertiert. 

Logic Apps bietet ähnliche Funktionen. Sie erhalten eine Flatfile über eine Vielzahl von verschiedenen Protokollen mit unterschiedlichen Connectortriggern (Dateisystem, FTP, HTTP usw.) und verwenden die [Flatfile-Decodierungsaktion](../logic-apps/logic-apps-enterprise-integration-flatfile.md), um die eingehenden Daten nach XML zu konvertieren. Sie können Ihre vorhandenen Flatfile-Schemas direkt nach Logic Apps verschieben, ohne dass Änderungen erforderlich werden, und dann Schemas in Ihr Integrationskonto hochladen.

### <a name="validation"></a>Überprüfen
Nachdem die eingehenden Daten nach XML konvertiert wurden (oder wenn XML das Eingangsnachrichtenformat war), wird eine Überprüfung durchgeführt, um festzustellen, ob die Nachricht dem XSD-Schema entspricht. Verwenden Sie hierzu in Logic Apps die Aktion [XML-Überprüfung](../logic-apps/logic-apps-enterprise-integration-xml-validation.md). Sie können auch hier dieselben Schemas wie in BizTalk Services ohne Änderungen anwenden.

### <a name="transform-messages"></a>Transformieren von Nachrichten
In BizTalk Services werden während der Transformationsphase XML-basierte Nachrichtenformate in andere konvertiert. Dies erfolgt durch Anwendung einer Zuordnung mithilfe der TRFM-basierten Zuordnung. In Logic Apps ist der Prozess ähnlich. Die Transformationsaktion führt eine Zuordnung aus Ihrem Integrationskonto durch. Der Hauptunterschied besteht darin, dass die Zuordnungen in Logic Apps im XSLT-Format vorliegen. XSLT bietet die Möglichkeit, vorhandenen XSLT-Code wiederzuverwenden. Dazu gehören auch die Zuordnungen, die für BizTalk Server erstellt wurden und Funktoide enthalten. 

### <a name="routing-rules"></a>Routingregeln
BizTalk Services trifft Routingentscheidungen anhand des Endpunkts/Connectors, an den eingehende Nachrichten/Daten gesendet werden sollen. Die Auswahl eines Endpunkts aus einer Reihe von vorkonfigurierten Endpunkten ist mit der Filteroption für das Routing möglich:

![](media/logic-apps-move-from-mabs/route-filter.png)

Logic Apps bietet eine komplexere Logik per [Bedingung](../logic-apps/logic-apps-use-logic-app-features.md) und [Switch](../logic-apps/logic-apps-switch-case.md) und damit erweiterte Möglichkeiten für Ablaufsteuerung und Routing. Das Konvertieren von Routingfiltern in BizTalk Services erfolgt am besten mithilfe einer **Bedingung**, *sofern* nur zwei Optionen zur Verfügung stehen. Wenn es mehr als zwei sind, verwenden Sie einen **Switch**.

### <a name="enrich"></a>Erweitern
Bei der Verarbeitung während der Erweiterungsphase in BizTalk Services werden dem empfangenen Nachrichtenkontext Eigenschaften hinzugefügt, die den empfangenden Daten zugeordnet sind. Dazu gehört z.B. das Höherstufen einer Eigenschaft, die für das Routing (weiter unten erläutert) verwendet wird, aus einer Datenbanksuche oder das Extrahieren eines Werts unter Verwendung eines XPath-Ausdrucks. Logic Apps ermöglicht den Zugriff auf alle Ausgaben von kontextbezogenen Daten aus vorhergehenden Aktionen, sodass dasselbe Verhalten einfach repliziert werden kann. Wenn Sie z.B. die SQL-Verbindungsaktion `Get Row` verwenden, geben Sie Daten aus einer SQL Server-Datenbank zurück und verwenden diese Daten in einer Entscheidungsaktion für das Routing. Auf dieselbe Weise sind Eigenschaften von eingehenden Service Bus-Nachrichten in der Warteschlange über einen Trigger sowie per XPath mit dem Ausdruck aus der XPath-Workflow Definition Language adressierbar.

### <a name="use-custom-code"></a>Verwenden von benutzerdefiniertem Code
BizTalk Services bietet die Möglichkeit der [Ausführung von benutzerdefiniertem Code](https://msdn.microsoft.com/library/azure/dn232389.aspx), der in Ihre eigenen Assemblys hochgeladen wurde. Dies wird über die [IMessageInspector](https://msdn.microsoft.com/library/microsoft.biztalk.services.imessageinspector.aspx)-Schnittstelle implementiert. Jede Phase in der Bridge enthält zwei Eigenschaften („On Enter Inspector“ und „On Exit Inspector“), die den .NET-Typ bereitstellen, den Sie erstellt haben und der diese Schnittstelle implementiert. Benutzerdefinierter Code ermöglicht die Ausführung komplexerer Verarbeitungsaufgaben mit den Daten sowie das Wiederverwenden von vorhandenem Codes in Assemblys, mit denen allgemeine Geschäftslogik ausgeführt wird. 

Logic Apps bietet zwei bevorzugte Möglichkeiten für das Ausführen von benutzerdefiniertem Code: Azure Functions und API-Apps. Mit Azure Functions können Funktionen erstellt und von Logic Apps aus aufgerufen werden. Weitere Informationen finden Sie unter [Hinzufügen und Ausführen von benutzerdefiniertem Code für Logik-Apps über Azure Functions](../logic-apps/logic-apps-azure-functions.md). Verwenden Sie API-Apps (eine Komponente von Azure App Service), um eigene Trigger und Aktionen zu erstellen. Weitere Informationen finden Sie unter [Erstellen einer benutzerdefinierten API zur Verwendung mit Logic Apps](../logic-apps/logic-apps-create-api-app.md). 

Wenn Sie über benutzerdefinierten Code in Assemblys verfügen, den Sie aus BizTalk Services aufrufen, können Sie diesen Code entweder nach Azure Functions verschieben oder benutzerdefinierte APIs mit API-Apps erstellen. Die Vorgehensweise hängt von Ihrer Implementierung ab. Wenn Sie z.B. Code verwenden, der einen anderen Dienst umschließt, für den Logic Apps über keinen Connector verfügt, erstellen Sie eine API-App und verwenden die Aktionen, die die API-App in Ihrer Logik-App bereitstellt. Wenn Sie über Hilfsfunktionen oder Bibliotheken verfügen, ist Azure Functions wahrscheinlich am besten geeignet.

### <a name="edi-processing-and-trading-partner-management"></a>EDI-Verarbeitung und Handelspartnerverwaltung
BizTalk Services bietet EDI- und B2B-Verarbeitung mit Unterstützung für AS2 (Applicability Statement 2), X12 und EDIFACT. Gleiches gilt für Logic Apps. In BizTalk Services erstellen Sie EDI-Bridges und erstellen/verwalten Handelspartner und Vereinbarungen im dedizierten Nachverfolgungs- und Verwaltungsportal.

In Logic Apps ist diese Funktion im [Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md) enthalten. Es umfasst das Integrationskonto und B2B-Aktionen für die EDI- und B2B-Verarbeitung. Das [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) dient dem Erstellen und Verwalten von [Handelspartnern](../logic-apps/logic-apps-enterprise-integration-partners.md) und [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md). Nachdem Sie ein Integrationskonto erstellt haben, können Sie dem Konto Logik-Apps zuordnen. Nach der Zuordnung können Sie die B2B-Aktionen für den Zugriff auf Handelspartnerinformationen innerhalb Ihrer Logik-App verwenden. Die folgenden Aktionen werden bereitgestellt:

* AS2-Codierung
* AS2-Decodierung
* X12-Codierung
* X12-Decodierung
* Codieren von EDIFACT-Nachrichten
* Decodieren von EDIFACT-Nachrichten

Diese Aktionen werden im Gegensatz zu BizTalk Services von den Transportprotokollen entkoppelt. Wenn Sie Ihre Logik-Apps erstellen, haben Sie daher mehr Flexibilität bei der Connectorauswahl für das Senden und Empfangen von Daten. Es ist z.B. möglich, X12-Dateien als Anlagen von E-Mail-Nachrichten zu empfangen und diese Dateien dann in einer Logik-App weiterzuverarbeiten. 

## <a name="manage-and-monitor"></a>Verwalten und Überwachen
Neben der Handelspartnerverwaltung bot das dedizierte Portal für BizTalk Services auch Nachverfolgungsfunktionen für die Überwachung und Problembehandlung. 

Logic Apps bietet umfangreichere Nachverfolgungs- und Überwachungsfunktionen im [Azure-Portal](../logic-apps/logic-apps-monitor-your-logic-apps.md) und über die [B2B-Lösung der Operations Management Suite](../logic-apps/logic-apps-monitor-b2b-message.md). Dies umfasst auch eine mobile App für die Überwachung von unterwegs.

## <a name="high-availability"></a>Hohe Verfügbarkeit
Um in BizTalk Services eine hohe Verfügbarkeit zu erzielen, verwenden Sie mehrere Instanzen in einer bestimmten Region, um die Verarbeitungslast aufzuteilen. Bei Logic Apps ist eine regionsweise hohe Verfügbarkeit bereits integriert – ohne dass dafür zusätzliche Kosten anfallen. Für die regionsübergreifende Notfallwiederherstellung für die B2B-Verarbeitung in BizTalk Services ist ein Prozess für die Sicherung und Wiederherstellung erforderlich. In Logic Apps wird eine regionsübergreifende, aktive/passive [Notfallwiederherstellungsfunktion](../logic-apps/logic-apps-enterprise-integration-b2b-business-continuity.md) bereitgestellt. Diese erlaubt die Synchronisierung von B2B-Daten über Integrationskonten hinweg in unterschiedlichen Regionen und damit eine hohe Geschäftskontinuität.

## <a name="next"></a>Next (Weiter)
* [Was sind Logik-Apps?](logic-apps-what-are-logic-apps.md)
* [Erstellen Ihres ersten Logik-App-Workflows zur Automatisierung von Prozessen zwischen Cloud-Apps und Cloud-Diensten](logic-apps-create-a-logic-app.md) oder [Konfigurieren eines Workflows mithilfe einer vorgefertigten Vorlage oder eines Musters für den schnellen Einstieg](logic-apps-use-logic-app-templates.md)  
* [Liste mit den Connectors](../connectors/apis-list.md), die Sie in einer Logik-App verwenden können

