---
title: "Unternehmensintegration für B2B: Azure Logic Apps | Microsoft-Dokumentation"
description: "Informationen zum Erstellen von B2B-Workflows und Unterstützen von Unternehmensintegrationsszenarien für Logik-Apps mit dem Enterprise Integration Pack"
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
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 5b1b65e3d1066bea6958fa6461a157ee39fbe7dc
ms.openlocfilehash: 514942b2552564f669d73b997a2d355ddb477b22
ms.contentlocale: de-de
ms.lasthandoff: 02/15/2017


---
# <a name="overview-b2b-scenarios-and-communication-with-the-enterprise-integration-pack"></a>Übersicht: B2B-Szenarien und Kommunikation mit dem Enterprise-Integrationspaket

Für Business-to-Business-Workflows (B2B) und eine nahtlose Kommunikation mit Azure Logic Apps können Sie Unternehmensszenarien mithilfe der Microsoft-Cloudlösung mit dem Namen Enterprise Integration Pack ermöglichen. Organisationen können Nachrichten elektronisch austauschen, auch wenn sie unterschiedliche Protokolle und Formate verwenden. Das Pack wandelt verschiedene Formaten in ein Format um, das die Systeme von Unternehmen interpretieren und verarbeiten können. Unternehmen können Nachrichten über branchenübliche Protokolle wie [AS2](../logic-apps/logic-apps-enterprise-integration-as2.md), [X12](logic-apps-enterprise-integration-x12.md) und [EDIFACT](../logic-apps/logic-apps-enterprise-integration-edifact.md) austauschen. Sie können Nachrichten auch mit Verschlüsselung und digitalen Signaturen schützen.

Wenn Sie mit BizTalk Server oder Microsoft Azure BizTalk Services vertraut sind, wird Ihnen die Nutzung der Enterprise Integration-Features leicht fallen, da die meisten Konzepte ähnlich sind. Ein wesentlicher Unterschied ist, dass Enterprise Integration mit Integrationskonten arbeitet, um die Speicherung und Verwaltung von Artefakten zu vereinfachen, die bei der B2B-Kommunikation verwendet werden. 

Was sie Architektur betrifft, basiert das Enterprise Integration Pack auf „Integrationskonten“. Diese Konten sind cloudbasierte Container, in denen alle Artefakte wie Schemas, Partner, Zertifikate, Zuordnungen und Vereinbarungen gespeichert werden. Sie können diese Artefakte zum Entwerfen, Bereitstellen und Verwalten von B2B-Apps sowie zum Einrichten von B2B-Workflows für Logik-Apps nutzen. Doch bevor Sie diese Artefakte verwenden können, müssen Sie zuerst Ihr Integrationskonto mit Ihrer Logik-App verknüpfen. Anschließend kann Ihre Logik-App auf die Artefakte in Ihrem Integrationskonto zugreifen.

## <a name="why-should-you-use-enterprise-integration"></a>Gründe für die Unternehmensintegration

* Mithilfe der Unternehmensintegration können Sie alle Ihre Artefakte zentral in Ihrem Integrationskonto speichern.
* Sie können B2B-Workflows erstellen und mit SaaS-Apps (Software-as-a-Service) anderer Anbieter, lokalen Apps und benutzerdefinierten Apps integrieren, indem Sie das Azure Logic Apps-Modul und alle seine Connectors nutzen.
* Mit Azure Functions können Sie benutzerdefinierten Code für Ihre Logik-Apps erstellen.

## <a name="how-to-get-started-with-enterprise-integration"></a>Erste Schritte mit der Unternehmensintegration

Mit dem Enterprise Integration Pack und dem Logik-App-Designer im **Azure-Portal** können Sie B2B-Apps erstellen und verwalten. Sie können auch [PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "PowerShell-Themen zu Logik-Apps") verwenden, um Ihre Logik-Apps zu verwalten.

Es folgen die allgemeinen Schritte, die Sie ausführen müssen, ehe Sie im Azure-Portal Apps erstellen können:

![Übersichtsbild](media/logic-apps-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>Gängige Szenarien

Für die Unternehmensintegration werden unterstützt diese Branchenstandards unterstützt:

* EDI (Electronic Data Interchange)
* EAI (Enterprise Application Integration)

## <a name="heres-what-you-need-to-get-started"></a>Voraussetzungen für die ersten Schritte

* Azure-Abonnement mit einem Integrationskonto
* Visual Studio 2015 zum Erstellen von Zuordnungen und Schemas
* [Microsoft Azure Logic Apps Enterprise Integration Tools for Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it-now"></a>Jetzt testen

[Stellen Sie voll funktionsfähige Beispiel-AS2-Logik-Apps mit Sende- und Empfangsfunktionen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive) unter Verwendung der B2B-Features von Azure Logic Apps bereit.

## <a name="learn-more"></a>Weitere Informationen
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


