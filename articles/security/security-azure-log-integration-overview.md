<properties
   pageTitle="Einführung in die Microsoft Azure-Protokollintegration | Microsoft Azure"
   description="Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# Einführung in die Microsoft Azure-Protokollintegration (Vorschau)

Sie erhalten Informationen zur Azure-Protokollintegration, zu den wichtigsten Funktionen und dazu, wie dieses Feature funktioniert.

## Übersicht

PaaS- (Platform-as-a-Service) als auch IaaS-Lösungen (Infrastructure-as-a-Service), die in Azure gehostet werden, generieren in Sicherheitsprotokollen eine große Menge von Daten. Diese Protokolle enthalten wichtige Informationen, die wertvolle Einblicke in Richtlinienverstöße, interne und externe Bedrohungen, Probleme bei der Einhaltung gesetzlicher Compliancevorgaben sowie Anomalien bei Netzwerk-, Host- und Benutzeraktivität liefern.

Mit der Azure-Protokollintegration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management) integrieren. Bei der Azure-Protokollintegration werden Azure-Diagnosen aus Ihren virtuellen Windows-Computern *(WAD)* sowie Diagnosedaten aus Partnerlösungen wie Web Application Firewall (WAF) erfasst. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Assets (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

![Azure-Protokollintegration][1]

## Welche Protokolle kann ich integrieren?

Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Diese Protokolle gliedern sich in die beiden folgenden Haupttypen:

- **Steuerungs-/Verwaltungsprotokolle**, die Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager ermöglichen. Ein Beispiel für diesen Protokolltyp sind Azure-Überwachungsprotokolle.
- **Datenebenenprotokolle**, die Einblicke in die Ereignisse ermöglichen, die bei Verwendung einer Azure-Ressource auftreten. Beispiele für diesen Protokolltyp sind System-, Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer.

Die Azure-Protokollintegration bietet aktuell Unterstützung für die Integration von Azure-Überwachungsprotokollen, VM-Protokollen und Azure Security Center-Warnungen.

Wenn Sie Fragen zur Azure-Protokollintegration haben, senden Sie eine E-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## Nächste Schritte

In diesem Dokument wurde die Azure-Protokollintegration vorgestellt. Weitere Informationen zur Azure-Protokollintegration und den unterstützten Protokolltypen finden Sie hier:

- [Microsoft Azure Log Integration for Azure logs (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) (Microsoft Azure-Protokollintegration für Azure-Protokolle [Vorschau]): Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration im Download Center.
- [Get started with Azure log integration](security-azure-log-integration-get-started.md) (Erste Schritte mit der Azure-Protokollintegration): In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure Storage sowie Azure-Überwachungsprotokolle und Azure Security Center-Warnungen integrieren.
- [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
- [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
- [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](../security-center/security-center-integrating-alerts-with-log-integration.md): In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
- [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Vorgänge Ihrer Azure-Ressourcen verschaffen können.

<!--Image references-->
[1]: ./media/security-azure-log-integration-overview/azure-log-integration.png

<!---HONumber=AcomDC_0921_2016-->