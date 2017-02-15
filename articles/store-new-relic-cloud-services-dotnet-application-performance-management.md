---
title: Verwenden von New Relic mit Azure | Microsoft Docs
description: "Erfahren Sie, wie Sie den New Relic-Dienst zum Verwalten und Überwachen Ihrer Azure-Anwendung verwenden."
services: 
documentationcenter: .net
author: nickfloyd
manager: timlt
editor: 
ms.assetid: b01011be-c344-4e33-987d-c93dac1971fb
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 08/23/2016
ms.author: nickfloyd@newrelic.com
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f4f13c909a6ff640d403f5264004176c087925dd


---
# <a name="new-relic-application-performance-management-on-azure"></a>Verwaltung der Anwendungsleistung mit New Relic auf Azure
Sie können Ihren mit Azure gehosteten Anwendungen die erstklassige Leistungsüberwachung von New Relic hinzufügen. Neben umfassenden Überwachungs-, Fehlerbehandlungs- und Optimierungsfunktionen für Ihre Azure-Apps profitieren Sie durch die Verwendung von Azure zusätzlich von einem reduzierten Preis für New Relic-Produkte.

## <a name="what-is-new-relic"></a>Was ist New Relic?
Mit [New Relic-Produkten](https://newrelic.com/products)können Sie App-Fehler beheben, potenziellen Problemen vorbeugen und die Leistung Ihrer gesamten Umgebung nachvollziehen. Es beschleunigt die Identifizierung und Diagnostizierung von Leistungsproblemen und gibt Ihnen die benötigten Informationen an die Hand, um diese Probleme zu lösen.

New Relic verfolgt die Ladezeit und den Durchsatz Ihrer Webtransaktionen – sowohl vom Server als auch von den Browsern der Benutzer. Das Tool zeigt, wie viel Zeit Sie in der Datenbank verbracht haben, analysiert langsame Abfragen und Webanforderungen, bietet Verfügbarkeitsüberwachung und Warnfunktionen, verfolgt Anwendungsausnahmen und vieles mehr. 

## <a name="special-pricing"></a>Sonderpreise
New Relic Standard steht Azure-Benutzern kostenlos zur Verfügung. New Relic Pro wird basierend auf der Instanzgröße für Azure Cloud Services angeboten. Preisinformationen finden Sie im Azure Marketplace auf der [New Relic-Seite](https://azure.microsoft.com/marketplace/partners/newrelic/newrelic/). Alternativ können Sie sich auch direkt bei New Relic (sales@newrelic.com) nach Preisen für Unternehmen erkundigen.

Azure-Kunden erhalten bei Bereitstellung des New Relic-Agents ein zweiwöchiges Testabonnement von New Relic Pro.

## <a name="sign-up-for-new-relic-using-the-azure-store"></a>Anmelden bei New Relic über den Azure Store
New Relic kann nahtlos in Azure-Web- und -Workerrollen integriert werden. Sie können sich direkt über den Azure Store schnell und einfach bei New Relic registrieren. Eine entsprechende Anleitung finden Sie in den [Anweisungen für die Azure Store-Registrierung](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services#signup) von New Relic.

## <a name="view-your-data"></a>Prüfen der Daten
Nach der Registrierung können Sie die praktische Anwendungsüberwachung und die datenbasierte Analyse von New Relic nutzen. So überprüfen Sie die Leistung Ihrer App in New Relic:

1. Wählen Sie im Azure-Portal die Option „Verwalten“ aus.
2. Melden Sie sich mit der E-Mail-Adresse und dem Kennwort Ihres New Relic-Kontos an.
3. Wählen Sie im Anwendungsindex Ihre App aus, um sämtliche Daten Ihrer App auf der [APM-Übersichtsseite](https://docs.newrelic.com/docs/apm/applications-menu/monitoring/apm-overview-page)anzuzeigen.

## <a name="using-new-relic-with-azure"></a>Verwenden von New Relic mit Azure
Weitere Informationen zur Verwendung von New Relic und Azure finden Sie auf der [Dokumentationswebsite von New Relic](https://docs.newrelic.com/docs/agents/net-agent/azure-installation): 

* [New Relic for .NET](https://docs.newrelic.com/docs/agents/net-agent/getting-started/new-relic-net)
* [Instrument a .NET Worker Role on Azure Cloud service](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/instrument-net-worker-role-azure-cloud-service)
* [New Relic for the Microsoft Azure Cloud platform](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-cloud-services)
* [New Relic for the Microsoft Azure App Services](https://docs.newrelic.com/docs/agents/net-agent/azure-installation/azure-portal)
* [New Relic/Azure troubleshooting](https://docs.newrelic.com/docs/agents/net-agent/azure-troubleshooting)




<!--HONumber=Nov16_HO3-->


