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
ms.date: 03/03/2017
ms.author: TomSh
translationtype: Human Translation
ms.sourcegitcommit: 2c9877f84873c825f96b62b492f49d1733e6c64e
ms.openlocfilehash: 434961f7d25195eee3310461a3a97e03ffdd8917
ms.lasthandoff: 03/15/2017


---
# <a name="introduction-to-microsoft-azure-log-integration"></a>Einführung in die Microsoft Azure-Protokollintegration
Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert.

## <a name="overview"></a>Übersicht
PaaS- (Platform-as-a-Service) als auch IaaS-Lösungen (Infrastructure-as-a-Service), die in Azure gehostet werden, generieren in Sicherheitsprotokollen eine große Menge von Daten. Diese Protokolle enthalten wichtige Informationen, die wertvolle Einblicke in Richtlinienverstöße, interne und externe Bedrohungen, Probleme bei der Einhaltung gesetzlicher Compliancevorgaben sowie Anomalien bei Netzwerk-, Host- und Benutzeraktivität liefern.

Mit der kostenlosen Azure-Protokollintegration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren. Die Azure-Protokollintegration erfasst Azure-Diagnosen von Ihren virtuellen Windows-Computern *(WAD)*, Azure-Aktivitätsprotokollen, Azure Security Center-Warnungen und Azure-Ressourcenanbieterprotokollen. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Assets (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

![Azure-Protokollintegration][1]

## <a name="what-logs-can-i-integrate"></a>Welche Protokolle kann ich integrieren?
Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Diese Protokolle gliedern sich in diese Haupttypen:

* **Steuerungs-/Verwaltungsprotokolle**, die Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager ermöglichen. Ein Beispiel für diesen Protokolltyp sind Azure-Aktivitätsprotokolle.
* **Datenebenenprotokolle**, die Einblicke in die Ereignisse ermöglichen, die bei Verwendung einer Azure-Ressource auftreten. Beispiele für diesen Protokolltyp sind System-, Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer sowie die über Azure Monitor konfigurierten Diagnoseprotokolle.
* **Verarbeitet Ereignisse**, die Einblicke in analysierte Ereignisse/Warnungen geben, die in Ihrem Auftrag verarbeitet werden. Beispiele für diesen Typ sind Azure Security Center-Warnungen, bei denen Azure Security Center Ihr Abonnement verarbeitet und analysiert hat und sehr prägnante Sicherheitswarnungen ausgibt.

Die Azure-Protokollintegration unterstützt derzeit die Integration von Azure-Aktivitätsprotokollen, Windows-Ereignisprotokollen vom virtuellen Computer unter Windows in Ihr Azure-Abonnement, Azure Security Center-Warnungen, Azure-Diagnoseprotokolle sowie Azure Active Directory-Überwachungsprotokolle.

In der folgenden Tabelle werden die Protokollkategorie und Details der SIEM-Integration erläutert.

| Protokolltyp  |Log Analytics unterstützt JSON (Splunk, ELK)| ArcSight  | QRadar  |   
|---|---|---|---|
|  AAD-Überwachungsprotokolle |  Ja | Erfordert die Erstellung einer FlexConnector JSON-Parserdatei. Weitere Informationen finden Sie in der ArcSight-Dokumentation.  |  Sie müssen eine Protokollquellenerweiterung erstellen. Weitere Informationen finden Sie in der QRadar-Dokumentation. |  
| Aktivitätsprotokolle  | Ja  |  FlexConnector-JSON-Parserdatei, verfügbar im Download Center zusammen mit dem Download der Azure-Protokollintegration |  [QRadar DSM](https://www.ibm.com/support/knowledgecenter/SSKMKU/com.ibm.dsm.doc/c_dsm_guide_microsoft_azure_overview.html) (über Syslog senden) |  
| ASC-Warnungen  | Ja  |  Erfordert die Erstellung einer FlexConnector JSON-Parserdatei. Weitere Informationen finden Sie in der ArcSight-Dokumentation. | [QRadar DSM](https://www.ibm.com/support/knowledgecenter/SSKMKU/com.ibm.dsm.doc/c_dsm_guide_microsoft_azure_overview.html) (über Syslog senden)   |   
| Diagnoseprotokolle (Ressourcenprotokolle) | Ja | Der Endbenutzer muss eine FlexConnector-JSON-Parserdatei erstellen. Weitere Informationen zur Vorgehensweise finden Sie in der ArcSight-Dokumentation. | Sie müssen eine Protokollquellenerweiterung erstellen. Weitere Informationen finden Sie in der QRadar-Dokumentation. |
| VM-Protokolle | Ja, über weitergeleitete Ereignisse und nicht über JSON | Ja, über die weitergeleiteten Ereignisse | Ja, über die weitergeleiteten Ereignisse |

Die Azure-Protokollintegration ist eine kostenlose Lösung – Sie müssen für die Binärdateien der Azure-Protokollintegration nichts bezahlen. Es fallen jedoch Kosten im Zusammenhang mit Azure Storage für die Informationen in den Protokolldateien an.

Wenn Sie Fragen zur Azure-Protokollintegration haben, senden Sie eine E-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## <a name="next-steps"></a>Nächste Schritte
In diesem Dokument wurde die Azure-Protokollintegration vorgestellt. Weitere Informationen zur Azure-Protokollintegration und den unterstützten Protokolltypen finden Sie hier:

* [Microsoft Azure Log Integration for Azure logs (Microsoft Azure-Protokoll-Integration für Azure-Protokolle)](https://www.microsoft.com/download/details.aspx?id=53324): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
* [Erste Schritte mit der Azure-Protokollintegration:](security-azure-log-integration-get-started.md) In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure-WAD-Speicher sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure Active Directory-Überwachungsprotokolle integrieren.
* [Integrate Diagnostics Logs (Integrieren von Diagnoseprotokollen)](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/25/integrate-azure-logs-streamed-to-event-hubs-to-siem/): Dieser Blogbeitrag stellt die Schritte zum Integrieren der Diagnoseprotokolle mithilfe der Azure-Protokollintegration bereit.
* [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
* [Activity and ASC alerts over syslog to QRadar (Aktivitäts- und ASC-Warnungen über Syslog an QRadar)](https://blogs.msdn.microsoft.com/azuresecurity/2016/09/24/integrate-azure-logs-to-qradar/): In diesem Blogbeitrag werden die Schritte zum Senden von Aktivitäts- und Azure Security Center-Warnungen über Syslog an QRadar erläutert.
* [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
* [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](../security-center/security-center-integrating-alerts-with-log-integration.md) : In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png

