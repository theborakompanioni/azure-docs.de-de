---
title: "Azure Security Center – Handbuch zur Problembehandlung | Microsoft Docs"
description: In diesem Dokument wird die Problembehandlung in Azure Security Center beschrieben.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 44462de6-2cc5-4672-b1d3-dbb4749a28cd
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: fa70dffc2a4bade44e1dec583bdfcf7b5dae6801
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-troubleshooting-guide"></a>Azure Security Center – Handbuch zur Problembehandlung
Dieses Handbuch ist für IT-Experten, Informationssicherheitsanalysten und Cloudadministratoren konzipiert, in deren Organisation Azure Security Center verwendet wird und die Security Center-Probleme lösen müssen.

>[!NOTE] 
>Ab Anfang Juni 2017 verwendet Security Center den Microsoft Monitoring Agent zum Sammeln und Speichern von Daten. Weitere Informationen finden Sie unter [Azure Security Center – Plattformmigration](security-center-platform-migration.md). Die Informationen in diesem Artikel stellen Security Center-Funktionen nach dem Umstieg auf den Microsoft Monitoring Agent vor.
>

## <a name="troubleshooting-guide"></a>Handbuch zur Problembehandlung
In diesem Handbuch wird beschrieben, wie Sie Probleme mit Security Center beheben. Der Großteil der Problembehandlung in Security Center erfolgt, indem zunächst die Einträge im [Überwachungsprotokoll](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) für die fehlerhafte Komponente geprüft werden. Über Überwachungsprotokolle können Sie Folgendes ermitteln:

* Welche Vorgänge durchgeführt werden
* Wer den Vorgang initiiert hat
* Wann der Vorgang durchgeführt wurde
* Status des Vorgangs
* Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

Das Überwachungsprotokoll enthält alle Schreibvorgänge (PUT, POST, DELETE), die für Ihre Ressourcen durchgeführt werden, aber keine Lesevorgänge (GET).

## <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Security Center verwendet den Microsoft Monitoring Agent, um sicherheitsrelevante Daten von Ihren virtuellen Azure-Computern zu sammeln. Derselbe Agent wird auch von der Operations Management Suite und dem Log Analytics-Dienst verwendet. Nachdem die Datensammlung aktiviert und der Agent auf dem Zielcomputer richtig installiert wurde, sollte dieser Prozess ausgeführt werden:

* HealthService.exe

Wenn Sie die Dienstverwaltungskonsole („services.msc“) öffnen, sehen Sie auch den Microsoft Monitoring Agent als ausgeführten Dienst, wie unten dargestellt:

![Dienste](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig5.png)

Wenn Sie erfahren möchten, welche Version des Agents Sie verwenden, öffnen Sie den **Task-Manager**, und suchen Sie auf der Registerkarte **Prozesse** den **Microsoft Monitoring Agent-Dienst**. Klicken Sie mit der rechten Maustaste darauf, und klicken Sie dann auf **Eigenschaften**. Auf der Registerkarte **Details** wird die Dateiversion wie unten dargestellt angezeigt:

![Datei](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig6.png)
   

## <a name="microsoft-monitoring-agent-installation-scenarios"></a>Installationsszenarien für den Microsoft Monitoring Agent
Es gibt zwei Installationsszenarien, die bei der Installation des Microsoft Monitoring Agents auf Ihrem Computer zu unterschiedlichen Ergebnissen führen können. Folgende Szenarien werden unterstützt:

* **Der Agent wird automatisch von Security Center installiert:** In diesem Szenario können Sie die Warnungen an beiden Orten einsehen – im Security Center und in der Protokollsuche. Sie erhalten E-Mail-Benachrichtigungen an die E-Mail-Adresse, die in der Sicherheitsrichtlinie für das Abonnement, zu der die Ressource gehört, konfiguriert wurde.
zu erstellen und zu verwalten.
* **Der Agent wird manuell auf einem virtuellen Computer in Azure installiert:** Wenn Sie in diesem Szenario Agents verwenden, die vor Februar 2017 manuell heruntergeladen und installiert wurden, können Sie die Warnungen nur dann im Security Center-Portal anzeigen, wenn Sie das Abonnement filtern, zu dem der Arbeitsbereich gehört. Wenn Sie das Abonnement herausfiltern, zu dem die Ressource gehört, können Sie keine Warnungen anzeigen. Sie erhalten E-Mail-Benachrichtigungen an die E-Mail-Adresse, die in der Sicherheitsrichtlinie für das Abonnement, zu der der Arbeitsbereich gehört, konfiguriert wurde.

>[!NOTE]
> Um das Verhalten im zweiten Szenario zu vermeiden, müssen Sie die neueste Version des Agents herunterladen.
> 

## <a name="troubleshooting-monitoring-agent-network-requirements"></a>Beheben von Problemen mit den Netzwerkanforderungen für den Überwachungs-Agent
Damit Agents eine Verbindung mit Security Center herstellen und sich bei diesem registrieren können, müssen sie Zugriff auf Netzwerkressourcen, einschließlich der Portnummern und Domänen-URLs, haben.

- Für Proxy-Server müssen Sie sicherstellen, dass die entsprechenden Proxy-Serverressourcen in Agenteinstellungen konfiguriert sind. Weitere Informationen zum [Ändern der Proxyeinstellungen](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-windows-agents#configure-proxy-settings) finden Sie in diesem Artikel.
- Wenn eine Firewall den Zugriff auf das Internet einschränkt, müssen Sie die Firewall so konfigurieren, dass OMS Zugriff erhält. In den Agenteinstellungen ist keine Aktion erforderlich.

Die folgende Tabelle zeigt die für die Kommunikation erforderlichen Ressourcen.

| Agent-Ressource | Ports | Umgehung der HTTPS-Überprüfung |
|---|---|---|
| *.ods.opinsights.azure.com | 443 | Ja |
| *.oms.opinsights.azure.com | 443 | Ja |
| *.blob.core.windows.net | 443 | Ja |
| *.azure-automation.net | 443 | Ja |

Wenn mit dem Agent Onboardingprobleme auftreten, lesen Sie den Artikel [How to troubleshoot Operations Management Suite onboarding issues](https://support.microsoft.com/en-us/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues) (Behandeln von Onboardingproblemen mit Operations Management Suite).


## <a name="troubleshooting-endpoint-protection-not-working-properly"></a>Schwierigkeiten bei der Endpoint Protection-Problembehandlung

Der Gast-Agent ist der übergeordnete Prozess aller Vorgänge der [Microsoft Antimalware](../security/azure-security-antimalware.md)-Erweiterung. Wenn beim Prozess für den Gast-Agent ein Fehler auftritt, schlägt die Microsoft Antimalware, die als untergeordneter Prozess des Gast-Agents ausgeführt wird, unter Umständen auch fehl.  In diesen Fällen ist es ratsam, die folgenden Optionen zu überprüfen:

- Wenn die Ziel-VM ein benutzerdefiniertes Image ist und der Ersteller der VM den Gast-Agent nicht installiert hat.
- Wenn das Ziel keine Windows-VM ist, sondern eine Linux-VM, tritt bei der Installation der Windows-Version der Antimalware-Erweiterung auf einer Linux-VM ein Fehler auf. Der Linux-Gast-Agent verfügt über bestimmte Anforderungen in Bezug auf die Betriebssystemversion und die erforderlichen Pakete, und wenn diese Anforderungen nicht erfüllt werden, funktioniert der VM-Agent auch dafür nicht. 
- Wenn die VM mit einer alten Version des Gast-Agent erstellt wurde. In diesem Fall sollten Sie beachten, dass einige alte Agents nicht selbst automatisch das Update auf die neuere Version durchführen können, weil dieses Problem dadurch verursacht werden kann. Verwenden Sie immer die aktuelle Version des Gast-Agents, wenn Sie Ihre eigenen Images erstellen.
- Für einige Verwaltungssoftwareanwendungen von Drittanbietern wird der Gast-Agent ggf. deaktiviert oder der Zugriff auf bestimmte Dateispeicherorte blockiert. Stellen Sie sicher, dass der Agent in der Ausschlussliste enthalten ist, wenn auf Ihrer VM Software von Drittanbietern installiert ist.
- Bestimmte Firewalleinstellungen oder Netzwerksicherheitsgruppen (NSGs) können bewirken, dass der Netzwerkdatenverkehr zum und vom Gast-Agent blockiert wird.
- Es kann sein, dass bestimmte Zugriffssteuerungslisten (Access Control Lists, ACLs) den Datenträgerzugriff verhindern.
- Durch einen Mangel an Speicherplatz kann verhindert werden, dass der Gast-Agent richtig funktioniert. 

Die Microsoft Antimalware-Benutzeroberfläche ist standardmäßig deaktiviert. Unter [Enabling Microsoft Antimalware User Interface on ARM VMs Post Deployment](https://blogs.msdn.microsoft.com/azuresecurity/2016/03/09/enabling-microsoft-antimalware-user-interface-post-deployment/) (Aktivieren der Microsoft Antimalware-Benutzeroberfläche auf ARM-VMs nach der Bereitstellung) finden Sie weitere Informationen dazu, wie Sie sie bei Bedarf aktivieren.

## <a name="troubleshooting-problems-loading-the-dashboard"></a>Behandlung von Problemen beim Laden des Dashboards

Wenn beim Laden des Security Center-Dashboards Probleme auftreten, stellen Sie sicher, dass der Benutzer, der das Security Center-Abonnement registriert (d.h. der erste Benutzer, der Security Center mit dem Abonnement geöffnet hat), und der Benutzer, der die Datensammlung aktivieren möchte, entweder *Besitzer* oder *Mitwirkender* des Abonnements sind. Ab diesem Zeitpunkt können auch Benutzer mit der *Leser*-Berechtigung für das Abonnement die Richtlinie unter „dashboard/alerts/recommendation/policy“ anzeigen.

## <a name="contacting-microsoft-support"></a>Kontaktaufnahme mit dem Microsoft-Support
Einige Probleme können mit den Richtlinien in diesem Artikel identifiziert werden, während andere im öffentlichen Security Center- [Forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureSecurityCenter)dokumentiert sind. Falls Sie weitere Hilfe zur Problembehandlung benötigen, können Sie wie unten gezeigt über das **Azure-Portal** eine neue Supportanfrage erstellen: 

![Microsoft-Support](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)

## <a name="see-also"></a>Weitere Informationen
In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

* [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md) : Erfahren Sie, wie Sie Entwurfsüberlegungen zur Einführung von Azure Security Center planen und umsetzen können.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) : Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) : Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
* [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md) : Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
* [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md) : Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Azure-Sicherheit und -Compliance.


