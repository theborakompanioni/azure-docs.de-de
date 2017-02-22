---
title: "Übersicht über Enterprise Integration | Microsoft-Dokumentation"
description: "Nutzen Sie die Enterprise Integration-Features, um die Integration von Geschäftsprozessen mithilfe von Logik-Apps zu ermöglichen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: dd517c4d-1701-4247-b83c-183c4d8d8aae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/08/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: dc8c9eac941f133bcb3a9807334075bfba15de46
ms.openlocfilehash: d0508a40cd4ccea1d7476b366b8e1d17a0b78847


---
# <a name="overview-of-the-enterprise-integration-pack"></a>Übersicht über das Enterprise Integration Pack
## <a name="what-is-the-enterprise-integration-pack"></a>Was ist das Enterprise Integration Pack?
Das Enterprise Integration Pack ist die Cloudlösung von Microsoft für das Ermöglichen einer reibungslosen Kommunikation zwischen Unternehmen (Business-to-Business, B2B). Es verwendet Protokolle gemäß Industriestandard wie [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md) und [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md) zum Austausch von Nachrichten zwischen Geschäftspartnern. Nachrichten können optional mithilfe einer Verschlüsselung und digitaler Signaturen geschützt werden. 

Das Enterprise Integration Pack ermöglicht Organisationen, die unterschiedliche Protokolle und Formate nutzen, den elektronischen Austausch von Nachrichten, indem die unterschiedlichen Formate in ein Format transformiert werden, das die Systeme beider Organisationen interpretieren und verarbeiten können. 

Wenn Sie mit BizTalk Server oder Microsoft Azure BizTalk Services vertraut sind, wird Ihnen die Nutzung der Enterprise Integration-Features leicht fallen, da die meisten Konzepte ähnlich sind. Ein wesentlicher Unterschied ist, dass Enterprise Integration mit Integrationskonten arbeitet, um die Speicherung und Verwaltung von Artefakten zu vereinfachen, die bei der B2B-Kommunikation verwendet werden. 

Aus Sicht der Systemarchitektur basiert das Enterprise Integration Pack auf **Integrationskonten** , in denen alle Artefakte gespeichert werden, die zum Entwerfen, Bereitstellen und Verwalten Ihrer B2B-Apps verwendet werden können. Ein Integrationskonto ist im Wesentlichen ein Container in der Cloud, in dem Sie Artefakte wie Schemas, Partner, Zertifikate, Zuordnungen und Vereinbarungen speichern können. Diese Artefakte können dann in Logik-Apps verwendet werden, um B2B-Workflows zu erstellen. Bevor Sie die Artefakte in einer Logik-App verwenden können, müssen Sie lediglich Ihr Integrationskonto mit Ihrer Logik-App verknüpfen. Nach dem Verknüpfen hat Ihre Logik-App Zugriff auf die Artefakte im Integrationskonto.  

## <a name="why-should-you-use-enterprise-integration"></a>Gründe für die Unternehmensintegration
* Bei der Unternehmensintegration können Sie alle Ihre Artefakte zentral in Ihrem Integrationskonto speichern. 
* Sie können das Logik-Apps-Modul und alle seine Connectors zum Erstellen von B2B-Workflows nutzen und für eine Integration mit SaaS-Anwendungen von Drittanbietern, lokalen Apps sowie benutzerdefinierten Anwendungen sorgen.
* Sie können auch Azure-Funktionen nutzen.

## <a name="how-to-get-started-with-enterprise-integration"></a>Erste Schritte mit der Unternehmensintegration
Mit dem Enterprise Integration Pack und dem Logik-App-Designer im **Azure-Portal**können Sie B2B-Apps erstellen und verwalten.  

Sie können auch [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "Logik-Apps-PowerShell-Themen") verwenden, um Ihre Logik-Apps zu verwalten. 

Hier ist eine Übersicht über die Schritte, die Sie ausführen müssen, ehe Sie im Azure-Portal Apps erstellen können: ![Übersichtsbild](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Gängige Szenarien
Für die Unternehmensintegration werden unterstützt diese Branchenstandards unterstützt:   

* EDI (Electronic Data Interchange)  
* EAI (Enterprise Application Integration)  

## <a name="heres-what-you-need-to-get-started"></a>Voraussetzungen für die ersten Schritte
* Azure-Abonnement mit einem Integrationskonto
* Visual Studio 2015 zum Erstellen von Zuordnungen und Schemas
* [Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>Ausprobieren
[Probieren Sie es jetzt aus](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive), um voll funktionsfähige Beispiel-AS2-Logik-Apps mit Sende- und Empfangsfunktionen unter Verwendung der B2B-Features von Logic Apps bereitzustellen.

## <a name="learn-more-about"></a>Weitere Informationen:
* [Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")
* [B2B-Szenarien (Business-to-Business)](../logic-apps/logic-apps-enterprise-integration-b2b.md "Informationen zum Erstellen von Logik-Apps mit B2B-Features")  
* [Zertifikate](logic-apps-enterprise-integration-certificates.md "Informationen zu Zertifikaten für die Unternehmensintegration")
* [Codierung/Decodierung von Flatfiles](logic-apps-enterprise-integration-flatfile.md "Informationen zum Codieren und Decodieren von Flatfile-Inhalten")  
* [Integrationskonten](../logic-apps/logic-apps-enterprise-integration-accounts.md "Informationen zu Integrationskonten")
* [Zuordnungen](../logic-apps/logic-apps-enterprise-integration-maps.md "Informationen zu Zuordnungen für die Unternehmensintegration")
* [Partner](logic-apps-enterprise-integration-partners.md "Informationen zu Unternehmensintegrationspartnern")
* [Schemas](logic-apps-enterprise-integration-schemas.md "Informationen zu Schemas für die Unternehmensintegration")
* [Überprüfung von XML-Nachrichten](logic-apps-enterprise-integration-xml.md "Informationen zum Überprüfen von XML-Nachrichten mit Logik-Apps")
* [XML-Transformation](logic-apps-enterprise-integration-transform.md "Informationen zu Zuordnungen für die Unternehmensintegration")
* [Enterprise-Integrationsconnectors](../connectors/apis-list.md "Informationen zu Enterprise Integration Pack-Connectors")
* [Integrationskontometadaten](../logic-apps/logic-apps-enterprise-integration-metadata.md "Erfahren Sie mehr über Integrationskontometadaten")
* [Überwachen von B2B-Nachrichten](logic-apps-monitor-b2b-message.md "Weitere Informationen zum Überwachen von B2B-Nachrichten")
* [Nachverfolgen von B2B-Nachrichten im OMS-Portal](logic-apps-track-b2b-messages-omsportal.md "Weitere Informationen zum Nachverfolgen von B2B-Nachrichten im OMS-Portal")




<!--HONumber=Jan17_HO3-->


