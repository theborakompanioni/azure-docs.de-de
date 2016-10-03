<properties 
	pageTitle="Übersicht über die Unternehmensintegration | Microsoft Azure App Service | Microsoft Azure" 
	description="Nutzen Sie die Enterprise Integration-Features, um die Integration von Geschäftsprozessen mithilfe von Logik-Apps zu ermöglichen." 
	services="logic-apps" 
	documentationCenter=".net,nodejs,java"
	authors="msftman" 
	manager="erikre" 
	editor="cgronlun"/>

<tags 
	ms.service="logic-apps" 
	ms.workload="integration" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/08/2016" 
	ms.author="deonhe"/>

# Übersicht über das Enterprise Integration Pack

## Was ist das Enterprise Integration Pack?
Das Enterprise Integration Pack ist die Cloudlösung von Microsoft für das Ermöglichen einer reibungslosen Kommunikation zwischen Unternehmen (Business-to-Business, B2B). Es verwendet Protokolle gemäß Industriestandard wie [AS2](./app-service-logic-enterprise-integration-as2.md), [X12](./app-service-logic-enterprise-integration-x12.md) und [EDIFACT](./app-service-logic-enterprise-integration-edifact.md) zum Austausch von Nachrichten zwischen Geschäftspartnern. Nachrichten können optional mithilfe einer Verschlüsselung und digitaler Signaturen geschützt werden.

Das Enterprise Integration Pack ermöglicht Organisationen, die unterschiedliche Protokolle und Formate nutzen, den elektronischen Austausch von Nachrichten, indem die unterschiedlichen Formate in ein Format transformiert werden, das die Systeme beider Organisationen interpretieren und verarbeiten können.

Wenn Sie mit BizTalk Server oder Microsoft Azure BizTalk Services vertraut sind, wird Ihnen die Nutzung der Enterprise Integration-Features leicht fallen, da die meisten Konzepte ähnlich sind. Ein wesentlicher Unterschied ist, dass Enterprise Integration mit Integrationskonten arbeitet, um die Speicherung und Verwaltung von Artefakten zu vereinfachen, die bei der B2B-Kommunikation verwendet werden.

Aus Sicht der Systemarchitektur basiert das Enterprise Integration Pack auf **Integrationskonten**, in denen alle Artefakte gespeichert werden, die zum Entwerfen, Bereitstellen und Verwalten Ihrer B2B-Apps verwendet werden können. Ein Integrationskonto ist im Wesentlichen ein Container in der Cloud, in dem Sie Artefakte wie Schemas, Partner, Zertifikate, Zuordnungen und Vereinbarungen speichern können. Diese Artefakte können dann in Logik-Apps verwendet werden, um B2B-Workflows zu erstellen. Bevor Sie die Artefakte in einer Logik-App verwenden können, müssen Sie lediglich Ihr Integrationskonto mit Ihrer Logik-App verknüpfen. Nach dem Verknüpfen hat Ihre Logik-App Zugriff auf die Artefakte im Integrationskonto.

## Gründe für die Unternehmensintegration
- Bei der Unternehmensintegration können Sie alle Ihre Artefakte zentral in Ihrem Integrationskonto speichern.
- Sie können das Logik-Apps-Modul und alle seine Connectors zum Erstellen von B2B-Workflows nutzen und für eine Integration mit SaaS-Anwendungen von Drittanbietern, lokalen Apps sowie benutzerdefinierten Anwendungen sorgen.
- Sie können auch Azure-Funktionen nutzen.

## Erste Schritte mit der Unternehmensintegration
Mit dem Enterprise Integration Pack und dem Logik-App-Designer im **Azure-Portal** können Sie B2B-Apps erstellen und verwalten.

Sie können auch [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logik-Apps-PowerShell-Themen") verwenden, um Ihre Logik-Apps zu verwalten.

Hier ist eine Übersicht über die Schritte, die Sie ausführen müssen, ehe Sie im Azure-Portal Apps erstellen können: ![Übersichtsbild](./media/app-service-logic-enterprise-integration-overview/overview-0.png)

## Gängige Szenarien

Für die Unternehmensintegration werden unterstützt diese Branchenstandards unterstützt:

- EDI (Electronic Data Interchange)
- EAI (Enterprise Application Integration)

## Voraussetzungen für die ersten Schritte
- Azure-Abonnement mit einem Integrationskonto
- Visual Studio 2015 zum Erstellen von Zuordnungen und Schemas
- [Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)

## Ausprobieren
[Probieren Sie es jetzt aus](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive), um voll funktionsfähige Beispiel-AS2-Logik-Apps mit Sende- und Empfangsfunktionen unter Verwendung der B2B-Features von Logik-Apps bereitzustellen.

## Weitere Informationen:
- [Vereinbarungen](./app-service-logic-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")
- [B2B-Szenarien (Business-to-Business)](./app-service-logic-enterprise-integration-b2b.md "Informationen zum Erstellen von Logik-Apps mit B2B-Features")
- [Zertifikate](./app-service-logic-enterprise-integration-certificates.md "Informationen zu Zertifikaten für die Unternehmensintegration")
- [Codierung/Decodierung von Flatfiles](./app-service-logic-enterprise-integration-flatfile.md "Informationen zum Codieren und Decodieren von Flatfile-Inhalten")
- [Integrationskonten](./app-service-logic-enterprise-integration-accounts.md "Informationen zu Integrationskonten")
- [Zuordnungen](./app-service-logic-enterprise-integration-maps.md "Informationen zu Zuordnungen für die Unternehmensintegration")
- [Partner](./app-service-logic-enterprise-integration-partners.md "Informationen zu Unternehmensintegrationspartnern")
- [Schemas](./app-service-logic-enterprise-integration-schemas.md "Informationen zu Schemas für die Unternehmensintegration")
- [Überprüfung von XML-Nachrichten](./app-service-logic-enterprise-integration-xml.md "Informationen zum Überprüfen von XML-Nachrichten mit Logik-Apps")
- [XML-Transformation](./app-service-logic-enterprise-integration-transform.md "Informationen zu Zuordnungen für die Unternehmensintegration")
- [Enterprise-Integrationsconnectors](../connectors/apis-list.md "Informationen zu Enterprise Integration Pack-Connectors")

<!---HONumber=AcomDC_0921_2016-->