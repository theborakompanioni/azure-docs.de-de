---
title: Integrieren von Protokollen aus Azure-Ressourcen in Ihre SIEM-Systeme | Microsoft-Dokumentation
description: Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: 9c1346e1-baf8-4975-b2f2-42ae05b2dc0a
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/10/2017
ms.author: TomSh
ms.custom: azlog
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 1aa93ac52d1d5c4efe222c6da505e3639170cf55
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Einführung in die Microsoft Azure-Protokollintegration
Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert.

## <a name="overview"></a>Übersicht

Mit der kostenlosen Azure-Protokollintegration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren.

Die Azure-Protokollintegration erfasst Windows-Ereignisse aus Protokollen der Windows-Ereignisanzeige, aus [Azure-Aktivitätsprotokollen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), aus [Azure Security Center-Warnungen](../security-center/security-center-intro.md) und aus [Azure-Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) von Azure-Ressourcen. Diese Integration unterstützt Ihre SIEM-Lösung beim Bereitstellen eines einheitlichen Dashboards für alle Ihre Assets (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

>[!NOTE]
Zu diesem Zeitpunkt sind die einzigen unterstützten Clouds Azure Commercial und Azure Government. Andere Clouds werden nicht unterstützt.

![Azure-Protokollintegration][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle kann ich integrieren?
Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Diese Protokolle stellen drei Arten von Protokollen dar:

* **Steuerungs-/Verwaltungsprotokolle** ermöglichen Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md). Ein Beispiel für diesen Protokolltyp sind Azure-Aktivitätsprotokolle.
* **Datenebenenprotokolle** ermöglichen Einblicke in die Ereignisse, die bei Verwendung einer Azure-Ressource auftreten. Ein Beispiel für diesen Protokolltyp sind die Kanäle **System**, **Sicherheit** und **Anwendung** der Windows-Ereignisanzeige auf einem virtuellen Windows-Computer. Ein weiteres Beispiel ist das über Azure Monitor konfigurierte Diagnoseprotokoll.
* **Verarbeitete Ereignisse** bieten analysierte Ereignis- und Benachrichtigungsinformationen, die in Ihrem Auftrag verarbeitet werden. Ein Beispiel für diese Art von Ereignis sind Azure Security Center-Warnungen, bei denen Azure Security Center Ihr Abonnement verarbeitet und analysiert hat, um Warnungen bereitzustellen, die für Ihren aktuellen Sicherheitsstatus relevant sind.

Die Azure-Protokollintegration unterstützt ArcSight, QRadar und Splunk. Erkundigen Sie sich in jedem Fall bei Ihrem SIEM-Anbieter nach einem nativen Connector. Manchmal müssen Sie die Azure-Protokollintegration nicht verwenden, wenn native Connectors verfügbar sind. Weitere Informationen zu unterstützten Protokolltypen finden Sie in den häufig gestellten Fragen.

>[!NOTE]
Während die Azure-Protokollintegration eine kostenlose Lösung darstellt, entstehen durch Speichern der Protokolldateiinformationen entsprechende Azure-Speicherkosten.

Unterstützung durch die Community ist über das [MSDN-Forum für die Azure-Protokollintegration](https://social.msdn.microsoft.com/Forums/office/home?forum=AzureLogIntegration) verfügbar. Das Forum bietet der AzLog-Community die Möglichkeit, sich gegenseitig mit Fragen, Antworten, Tipps und Tricks zur optimalen Verwendung der Azure-Protokollintegration zu unterstützen. Darüber hinaus überwacht das Team für die Azure-Protokollintegration dieses Forum und bietet nach Möglichkeit entsprechende Hilfe an.

Sie können zudem eine [Supportanfrage](../azure-supportability/how-to-create-azure-support-request.md) öffnen. Wählen Sie zu diesem Zweck **Protokollintegration** als Dienst aus, für den Sie Support anfordern.

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde die Azure-Protokollintegration vorgestellt. Weitere Informationen zur Azure-Protokollintegration und den unterstützten Protokolltypen finden Sie hier:

* [Microsoft Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Erste Schritte mit der Azure-Protokollintegration:](security-azure-log-integration-get-started.md) In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure-WAD-Speicher sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure Active Directory-Überwachungsprotokolle integrieren.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird. Dieses Blog entspricht unserer aktuellen Position zur Konfiguration der Partnerlösungen. Konsultieren Sie in jedem Fall zunächst die Dokumentation zur Partnerlösung.
* [Activity and ASC alerts over syslog to QRadar (Aktivitäts- und ASC-Warnungen über Syslog an QRadar)](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/): In diesem Blogbeitrag werden die Schritte zum Senden von Aktivitäts- und Azure Security Center-Warnungen über Syslog an QRadar erläutert.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Integrieren von Security Center-Warnungen mit der Azure-Protokollintegration](../security-center/security-center-integrating-alerts-with-log-integration.md): In diesem Dokument wird gezeigt, wie Azure Security Center-Warnungen mit der Azure-Protokollintegration synchronisiert werden.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png

