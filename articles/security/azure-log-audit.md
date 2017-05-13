---

title: "Azure-Protokollierung und -Überwachung | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie auf der Grundlage von Protokollierungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen."
services: security
documentationcenter: na
author: UnifyCloud
manager: swadhwa
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2017
ms.author: TomSh
ms.translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: df1778b6a3e74d79e55dcc18c4faff7944063a2f
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017


---
# <a name="azure-logging-and-auditing"></a>Azure-Protokollierung und -Überwachung
## <a name="10-introduction"></a>1.0 Einführung
### <a name="11-overview"></a>1.1 Übersicht
Microsoft hat eine Reihe von Whitepapers, Sicherheitsübersichten, bewährten Methoden und Prüflisten entwickelt, um aktuelle und zukünftige Azure-Kunden dabei zu unterstützen, die verschiedenen sicherheitsbezogenen Funktionen zu verstehen und zu verwenden, die in und um die Azure Platform herum verfügbar sind. Die Themen variieren in Bezug auf Umfang und Reichweite und werden regelmäßig aktualisiert. Dieses Dokument ist Teil dieser Serie, wie im folgenden Abschnitt zusammengefasst.
### <a name="12-azure-platform"></a>1.2 Azure Platform
Azure ist eine offene und flexible Clouddienstplattform, die eine sehr breite Palette an Betriebssystemen, Programmiersprachen, Frameworks, Tools, Datenbanken und Geräten unterstützt.

Dazu zählen z. B.:
-    Linux-Container mit Docker-Integration ausführen.

-    Apps mit JavaScript, Python, .NET, PHP, Java und Node.js erstellen.

-    Back-Ends für iOS-, Android- und Windows-Geräte erstellen.

Die öffentlichen Azure-Clouddienste unterstützen dieselben Technologien, die bereits von Millionen von Entwicklern und IT-Profis zuverlässig eingesetzt werden.

Wenn Sie IT-Ressourcen erstellen oder zu einem Cloudanbieter migrieren, verlassen Sie sich darauf, dass die jeweilige Organisation Ihre Anwendungen und Daten mit den Diensten und Steuerungsmöglichkeiten schützen kann, die sie zum Verwalten der Sicherheit Ihrer cloudbasierten Ressourcen bereitstellt.

Die Infrastruktur von Azure ist von den Hardwareressourcen bis zu den Anwendungen vollständig auf das gleichzeitige Hosten von Millionen von Kunden ausgelegt und stellt für Unternehmen eine vertrauenswürdige Grundlage zur Erfüllung ihrer Sicherheitsanforderungen dar. Darüber hinaus bietet Azure Ihnen ein breites Spektrum an konfigurierbaren Sicherheitsoptionen, die Sie selbst steuern können, um die Sicherheit an die individuellen Anforderungen Ihrer Bereitstellungen anzupassen. Dieses Dokument unterstützt Sie dabei, die folgenden Anforderungen zu erfüllen.

### <a name="13-abstract"></a>1.3 Zusammenfassung
Die Überwachung und Protokollierung von sicherheitsbezogenen Ereignissen und zugehörigen Warnungen sind wichtige Komponenten einer effektiven Datenschutzstrategie. Sicherheitsprotokolle und Berichte bieten Ihnen einen elektronischen Datensatz zu verdächtigen Aktivitäten und helfen Ihnen bei der Erkennung von Mustern, die möglicherweise auf erfolglose oder erfolgreiche externe Eindringversuche in das Netzwerk sowie auf interne Angriffe hinweisen. Sie können im Rahmen der Überwachung entsprechende Benutzeraktivitäten überwachen, die Einhaltung gesetzlicher Bestimmungen dokumentieren, forensische Analyse durchführen und vieles mehr. Warnungen stellen eine sofortige Benachrichtigung beim Auftreten von Sicherheitsereignissen dar.

Microsoft Azure-Dienste und -Produkte bieten Ihnen konfigurierbare Optionen für die Überwachung und Protokollierung der Sicherheit, damit Sie Lücken in Ihren Sicherheitsrichtlinien und -mechanismen identifizieren und behandeln können, um Verstöße zu vermeiden. Microsoft-Dienste bieten einige (und in gewissen Fällen auch alle) der folgenden Optionen: zentrale Überwachungs-, Protokollierungs- und Analysesysteme für eine kontinuierliche Sichtbarkeit, zeitnahe Warnungen und Berichte, die Sie beim Verwalten großer Informationsmengen unterstützen, die von Geräten und Diensten generiert werden.

Microsoft Azure-Protokolldaten können zur Analyse in Systeme zur Verwaltung von Sicherheitsvorfällen und -ereignissen (Security Incident and Event Management, SIEM) exportiert und in Überwachungslösungen von Drittanbietern integriert werden.

Dieses Whitepaper dient als Einführung in das Generieren, Erfassen und Analysieren von Sicherheitsprotokollen von in Azure gehosteten Diensten, mit der Sie Einblicke in die Sicherheit ihrer Azure-Bereitstellungen erhalten können. Dieses Whitepaper ist auf Anwendungen und Dienste beschränkt, die in Azure erstellt und bereitgestellt werden.

> [!Note]
> Einige Empfehlungen in diesem Artikel können unter Umständen zu einer erhöhten Daten-, Netzwerk- oder Serverressourcenauslastung führen, was mit zusätzlichen Lizenz- oder Abonnementkosten verbunden sein kann.

## <a name="20-types-of-logs-in-azure"></a>2.0 Protokolltypen in Azure
Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Protokolle stellen Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme. Darüber hinaus können Sie auf der Grundlage von Protokolldaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

Azure führt für jeden Azure-Dienst eine umfassende Protokollierung durch. Diese Protokolle gliedern sich in diese Haupttypen:
-    **Steuerungs-/Verwaltungsprotokolle** ermöglichen Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager. Ein Beispiel für diesen Protokolltyp sind [Azure-Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs).

-    **Datenebenenprotokolle** ermöglichen Einblicke in die Ereignisse, die bei Verwendung einer Azure-Ressource auftreten. Beispiele für diesen Protokolltyp sind System-, Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer sowie die über Azure Monitor konfigurierten [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).


-    **Verarbeitete Ereignisse** enthalten Informationen zu analysierten Ereignisse/Warnungen, die in Ihrem Auftrag verarbeitet wurden. Beispiele für diesen Typ sind [Azure Security Center-Warnungen](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts), bei denen [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) Ihr Abonnement verarbeitet und analysiert hat und prägnante Sicherheitswarnungen ausgibt.

Die folgende Tabelle enthält die wichtigsten Protokolltypen, die in Azure verfügbar sind.

| Protokollkategorie | Protokolltyp | Verwendungen | Integration |
| ------------ | -------- | ------ | ----------- |
|[Aktivitätsprotokolle](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|Ereignisse der Steuerungsebene für Azure Resource Manager-Ressourcen|    Sie bieten Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden.|    REST-API und [Azure Monitor](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)|
|[Azure-Diagnoseprotokolle](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|Häufige Daten zum Betrieb der Azure Resource Manager-Ressourcen im Abonnement|    Sie bieten einen Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.| Azure Monitor, [Stream](https://docs.microsoft.com/en-us/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs)|
|[AAD-Berichterstellung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-azure-portal)|Protokolle und Berichte|Aktivitäten zur Benutzeranmeldung und Systemaktivitätsinformationen zur Benutzer- und Gruppenverwaltung|[Graph-API](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-graph-api-quickstart)|
|[Virtual Machine und Cloud Services](https://docs.microsoft.com/en-us/azure/cloud-services/cloud-services-dotnet-diagnostics-storage)|Windows-Ereignisprotokoll und Linux-Syslog|    Erfasst Systemdaten und Protokollierungsdaten auf den virtuellen Computern und überträgt die Daten in ein Speicherkonto Ihrer Wahl.|    Windows mit [WAD](https://docs.microsoft.com/en-us/azure/azure-diagnostics) (Microsoft Azure-Diagnosespeicher) und Linux in Azure Monitor|
|[Speicheranalyse](https://docs.microsoft.com/en-us/rest/api/storageservices/fileservices/storage-analytics)|Speicherprotokollierung und Bereitstellung von Metrikdaten für ein Speicherkonto|Bietet einen Einblick in Nachverfolgungsanforderungen und ermöglicht die Analyse von Verwendungstrends sowie die Diagnose von Problemen mit dem Speicherkonto.|    REST-API oder [Clientbibliothek](https://msdn.microsoft.com/en-us/library/azure/mt347887.aspx)|
|[Datenflussprotokolle für Netzwerksicherheitsgruppen](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-nsg-flow-logging-overview)|Verwendet das JSON-Format und zeigt eingehende und ausgehende Datenflüsse auf Regelbasis|Anzeigen von Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe|[Network Watcher](https://docs.microsoft.com/en-us/azure/network-watcher/network-watcher-monitoring-overview)|
|[Application Insight](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-overview)|Protokolle, Ausnahmen und benutzerdefinierte Diagnosen|    Erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen.|    REST-API, [Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-azure-and-power-bi/)|
|Prozessdaten/Sicherheitswarnung|    Azure Security Center-Warnung, OMS-Warnung|    Sicherheitsinformationen und Warnungen|     REST-APIs, JSON|

### <a name="21-activity-log"></a>2.1 Aktivitätsprotokoll
Das [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Das Aktivitätsprotokoll wurde bisher als „Überwachungsprotokoll“ oder „Vorgangsprotokoll“ bezeichnet, da es [Ereignisse der Steuerungsebene](https://driftboatdave.com/2016/10/13/azure-auditing-options-for-your-custom-reporting-needs/) für Ihre Abonnements enthält. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Abonnements durchgeführt wurden. Sie können auch den Status des Vorgangs und andere relevante Eigenschaften verstehen. Das Aktivitätsprotokoll umfasst keine Lesevorgänge (GET).

Hier beziehen sich PUT, POST, DELETE auf alle Schreibvorgänge, die das Aktivitätsprotokoll für die Ressourcen enthält. Sie können z. B. bei der Problembehandlung mithilfe der Aktivitätsprotokolle einen Fehler ermitteln oder nachverfolgen, welche Änderungen an einer Ressource ein Benutzer in Ihrer Organisation vorgenommen hat.

![Aktivitätsprotokoll](./media/azure-log-audit/azure-log-audit-fig1.png)


Sie können Ereignisse per Azure-Portal, [Befehlszeilenschnittstelle](https://docs.microsoft.com/azure/storage/storage-azure-cli), PowerShell-Cmdlets und [Azure Monitor-REST-API](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-rest-api-walkthrough) aus dem Aktivitätsprotokoll abrufen. Aktivitätsprotokolle weisen eine Datenaufbewahrungsdauer von 19 Tagen auf.

Integrationsszenarien
-    [Erstellen einer E-Mail- oder Webhookwarnung, die ein Aktivitätsprotokoll auslöst](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-auditlog-to-webhook-email)

-    Streamen an einen [Event Hub](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs) zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI

-    Analysieren in Power BI mit dem [Power BI-Inhaltspaket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/)

-    [Speichern unter einem Speicherkonto zur Archivierung oder manuellen Untersuchung](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-activity-log) Sie können die Aufbewahrungsdauer (in Tagen) mithilfe von Protokollprofilen angeben.

-    Abfragen und Anzeigen des Protokolls im Azure-Portal

-    Fragen Sie es per PowerShell-Cmdlet, CLI oder REST-API ab.

-    Exportieren des Aktivitätsprotokolls mit Protokollprofilen in [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

Sie können ein Speicherkonto oder [Event Hub-Namespace](https://docs.microsoft.com/azure/event-hubs/event-hubs-resource-manager-namespace-event-hub-enable-archive) verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden [RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure)-Zugriff auf beide Abonnements.
### <a name="22-azure-diagnostic-logs"></a>2.2 Azure-Diagnoseprotokolle
Azure-Diagnoseprotokolle werden von einer Ressource ausgegeben und stellen umfangreiche, in kurzen Abständen erfasste Betriebsdaten der Ressource bereit. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. ([Windows-Systemereignisprotokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events) sind beispielsweise eine Diagnoseprotokollkategorie für virtuelle Computer; [Blob-, Tabellen- und Warteschlangenprotokolle](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account) sind Diagnoseprotokollkategorien für Speicherkonten.) Darüber hinaus unterscheiden sich Diagnoseprotokolle vom Aktivitätsprotokoll, das Informationen zu den Vorgängen liefert, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden.

![Azure-Diagnoseprotokolle](./media/azure-log-audit/azure-log-audit-fig2.png)

Azure-Diagnoseprotokolle bieten mehrere Konfigurationsoptionen: Azure-Portal, die Verwendung von PowerShell, der Befehlszeilenschnittstelle und der REST-API.

**Integrationsszenarien**
-    Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [Speicherkonto](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs) gespeichert werden. Mithilfe der Diagnoseeinstellungen können Sie eine Aufbewahrungsdauer (in Tagen) angeben.

-    Sie können [Diagnoseprotokolle zur Erfassung](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs) durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie [Power BI](https://powerbi.microsoft.com/documentation/powerbi-azure-and-power-bi/) an Event Hubs streamen.

-    Sie können Diagnoseprotokolle mit [OMS Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) analysieren.

**Unterstützten Dienste, Schema für Diagnoseprotokolle und unterstützte Protokollkategorien pro Ressourcentyp**


| Dienst | Schema und Dokumente | Ressourcentyp | Kategorie |
| ------- | ------------- | ------------- | -------- |
|Lastenausgleichsmodul| [Protokollanalysen für den Azure-Lastenausgleich (Vorschau)](https://docs.microsoft.com/en-us/azure/load-balancer/load-balancer-monitor-log)|Microsoft.Network/loadBalancers|    LoadBalancerAlertEvent|
|||Microsoft.Network/loadBalancers|    LoadBalancerProbeHealthStatus
|Netzwerksicherheitsgruppen|[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](https://docs.microsoft.com/en-us/azure/virtual-network/virtual-network-nsg-manage-log)|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|
|||Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|
|Anwendungsgateways|[Diagnoseprotokollierung für Application Gateway](https://docs.microsoft.com/en-us/azure/application-gateway/application-gateway-diagnostics)|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|
|||Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|
|Key Vault|[Azure-Schlüsseltresor-Protokollierung](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-logging)|Microsoft.KeyVault/vaults|AuditEvent|
|Azure Search|[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](https://docs.microsoft.com/en-us/azure/search/search-traffic-analytics)|Microsoft.Search/searchServices|OperationLogs|
|Data Lake Store|[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-diagnostic-logs)|Microsoft.DataLakeStore/accounts|Audit|
|Data Lake Analytics|[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](https://docs.microsoft.com/en-us/azure/data-lake-analytics/data-lake-analytics-diagnostic-logs)|Microsoft.DataLakeAnalytics/accounts|Audit|
|||Microsoft.DataLakeAnalytics/accounts|Anforderungen|
|||Microsoft.DataLakeStore/accounts|Anforderungen|
|Logik-Apps|[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](https://docs.microsoft.com/en-us/azure/logic-apps/logic-apps-track-integration-account-custom-tracking-schema)|Microsoft.Logic/workflows|WorkflowRuntime|
|||Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|
|Azure Batch|[Diagnoseprotokolle für Azure Batch](https://docs.microsoft.com/en-us/azure/batch/batch-diagnostics)|Microsoft.Batch/batchAccounts|ServiceLog|
|Azure-Automatisierung|[Protokollanalysen für Azure Automation](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|Microsoft.Automation/automationAccounts|JobLogs|
|||Microsoft.Automation/automationAccounts|JobStreams|
|Event Hubs|[Azure Event Hubs-Diagnoseprotokolle](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-diagnostic-logs)|Microsoft.EventHub/namespaces|ArchiveLogs|
|||Microsoft.EventHub/namespaces|OperationalLogs|
|Stream Analytics|[Auftragsdiagnoseprotokolle](https://docs.microsoft.com/en-us/azure/stream-analytics/stream-analytics-job-diagnostic-logs)|Microsoft.StreamAnalytics/streamingjobs|Ausführung|
|||Microsoft.StreamAnalytics/streamingjobs|Erstellen|
|SERVICE BUS|[Azure Service Bus Diagnoseprotokolle](https://docs.microsoft.com/en-us/azure/service-bus-messaging/service-bus-diagnostic-logs)|Microsoft.ServiceBus/namespaces|OperationalLogs|

### <a name="23-azure-active-directory-reporting"></a>2.3 Azure Active Directory-Berichterstellung
Azure Active Directory (Azure AD) umfasst Sicherheits-, Aktivitäts- und Prüfberichte für Ihr Verzeichnis. Der [Azure Active Directory-Überwachungsbericht](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-guide) hilft Kunden, privilegierte Aktionen zu bestimmen, die in ihrem Azure Active Directory aufgetreten sind. Privilegierte Aktionen umfassen Änderungen zur Rechteerweiterung (z. B. das Erstellen von Rollen oder Zurücksetzen von Kennwörtern), das Ändern von Richtlinienkonfigurationen (z. B. Kennwortrichtlinien) oder Änderungen an der Verzeichniskonfiguration (z. B. Änderungen an Domänenverbundeinstellungen).

Die Berichte enthalten den Überwachungsdatensatz für den Ereignisnamen, den Akteur, der die Aktion ausgeführt hat, die von der Änderung betroffene Zielressource sowie Datum und Uhrzeit (in UTC). Kunden können die Liste mit den Überwachungsereignissen für ihre Azure Active Directory-Instanz über das [Azure-Portal](https://portal.azure.com/) abrufen, wie in [Anzeigen von Überwachungsprotokollen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal) beschrieben. Hier sehen Sie eine Liste der enthaltenen Berichte:

| Sicherheitsberichte | Aktivitätsberichte | Prüfberichte |
| :--------------- | :--------------- | :------------ |
|Anmeldungen von unbekannten Quellen|    Anwendungsnutzung: Zusammenfassung|    Verzeichnisprüfbericht|
|Anmeldungen nach mehreren Fehlern|    Anwendungsnutzung: detailliert||
|Anmeldungen aus mehreren geografischen Regionen|    Anwendungsdashboard||
|Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten|    Kontobereitstellungsfehler||
|Irreguläre Anmeldeaktivitäten|    Geräte einzelner Benutzer||
|Anmeldungen von möglicherweise infizierten Geräten|    Aktivität einzelner Benutzer||
|Benutzer mit anomalen Anmeldeaktivitäten|    Gruppenaktivitätsbericht||
||Bericht zur Registrierung für die Kennwortzurücksetzung||
||Kennwortzurücksetzungsaktivität|||

Die Daten dieser Berichte können für Ihre Anwendungen – beispielsweise SIEM-Systeme, Überwachungs- und Business Intelligence-Tools – nützlich sein. Die Azure AD-Berichterstellungs-APIs bieten über einen Satz von REST-basierten APIs programmgesteuerten Zugriff auf die Daten. Sie können diese [APIs](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started) über verschiedene Programmiersprachen und Tools aufrufen.

Ereignisse im Azure AD-Überwachungsbericht werden für 180 Tage beibehalten.

> [!Note]
> Weitere Informationen zur Aufbewahrung von Berichten finden Sie unter [Aufbewahrungsrichtlinien für Azure Active Directory-Berichte](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-retention).

Kunden, die ihre Überwachungsereignisse für längere Zeit speichern möchten, können mithilfe der Reporting-API regelmäßig [Überwachungsereignisse](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-audit-events) in einen separaten Datenspeicher abrufen.

### <a name="24-virtual-machine-logs-using-azure-diagnostics"></a>2.4 Virtual Machine-Protokolle mit der Azure-Diagnose
Die [Azure-Diagnose](https://docs.microsoft.com/azure/azure-diagnostics) ist eine Funktion in Azure, mit der Diagnosedaten für eine bereitgestellte Anwendung erfasst werden können. Sie können die Diagnoseerweiterung von einer Reihe verschiedener Quellen aus verwenden. Derzeit werden [Web- und Workerrollen von Azure Cloud Services](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) unterstützt.

![Virtual Machine-Protokolle mit der Azure-Diagnose](./media/azure-log-audit/azure-log-audit-fig3.png)

[Azure Virtual Machines](https://azure.microsoft.com/documentation/learning-paths/virtual-machines/) unter Microsoft Windows und [Service Fabric](https://docs.microsoft.com/azure/service-fabric/service-fabric-overview)

Sie können die Azure-Diagnose wie folgt auf einem virtuellen Computer aktivieren:

-    Mithilfe von Visual Studio (weitere Informationen finden Sie unter [Verfolgen von Azure Virtual Machines mit Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-debug-cloud-services-virtual-machines))

-    [Remoteeinrichtung der Azure-Diagnose auf einem virtuellen Azure-Computer](https://docs.microsoft.com/azure/virtual-machines-dotnet-diagnostics)

-    [Verwenden von PowerShell zum Einrichten der Diagnose für Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

-    [Erstellen eines virtuellen Windows-Computers mit Überwachung und Diagnose mithilfe von Azure Resource Manager-Vorlagen](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="25-storage-analytics"></a>2.5 Speicheranalyse
Die [Azure-Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics) führt die Protokollierung aus und stellt Metrikdaten für ein Speicherkonto bereit. Mit diesen Daten können Sie Anforderungen verfolgen, Verwendungstrends analysieren und Probleme mit dem Speicherkonto diagnostizieren. Die Protokollierung der Speicheranalyse ist für [Blob-, Warteschlangen- und Tabellenspeicherdienste](https://docs.microsoft.com/azure/storage/storage-introduction) nicht verfügbar. Die Speicheranalyse protokolliert ausführliche Informationen zu erfolgreichen und nicht erfolgreichen Anforderungen an einen Speicherdienst.

Anhand dieser Informationen können einzelne Anforderungen überwacht und Probleme mit einem Speicherdienst untersucht werden. Anforderungen werden auf Grundlage der besten Leistung protokolliert. Protokolleinträge werden nur erstellt, wenn Anforderungen für den Dienstendpunkt gestellt wurden. Wenn beispielsweise ein Speicherkonto Aktivität im Blob-Endpunkt, jedoch nicht im Tabellen- oder Warteschlangenendpunkt aufweist, werden nur Protokolle für den Blob-Dienst erstellt.

Zum Verwenden der Speicheranalyse müssen Sie sie einzeln für jeden zu überwachenden Dienst aktivieren. Sie können sie im [Azure-Portal](https://portal.azure.com/) aktivieren. Details finden Sie unter [Überwachen eines Speicherkontos im Azure-Portal](https://docs.microsoft.com/azure/storage/storage-monitor-storage-account). Sie können die Speicheranalyse auch programmgesteuert über die REST-API oder die Clientbibliothek aktivieren. Verwenden Sie den Vorgang „Diensteigenschaften festlegen“, um die Speicheranalyse für jeden Dienst einzeln zu aktivieren.

Die aggregierten Daten werden in einem bekannten BLOB (zur Protokollierung) und in bekannten Tabellen (als Metrik) gespeichert. Der Zugriff erfolgt über APIs für den BLOB-Dienst und Tabellendienst.

Bei der Speicheranalyse ist die Menge der gespeicherten Daten auf 20 TB beschränkt. Diese Beschränkung gilt unabhängig vom Gesamtlimit für Ihr Speicherkonto. Alle Protokolle werden in [Block-Blobs](https://docs.microsoft.com/azure/storage/storage-analytics) in einem Container namens „$logs“ gespeichert, der automatisch erstellt wird, wenn die Speicheranalyse für ein Speicherkonto aktiviert ist.

> [!Note]
> Weitere Informationen zu Abrechnungs- und Datenaufbewahrungsrichtlinien finden Sie unter [Speicheranalyse und Speicheranalysekosten](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-and-billing).
>
> [!Note]
> Weitere Informationen zu Speicherkontobegrenzungen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](https://docs.microsoft.com/azure/storage/storage-scalability-targets).

Die folgenden Typen authentifizierter und anonymer Anforderungen werden protokolliert.



| Authentifiziert  | Anonym|
| :------------- | :-------------|
| Erfolgreiche Anforderungen | Erfolgreiche Anforderungen |
|Fehlerhafte Anforderungen, einschließlich Timeout-, Drosselungs-, Netzwerk- und Autorisierungsfehler sowie anderer Fehler | Anforderungen mithilfe einer SAS (Shared Access Signature), einschließlich fehlerhafter und erfolgreicher Anforderungen |
| Anforderungen mithilfe einer SAS (Shared Access Signature), einschließlich fehlerhafter und erfolgreicher Anforderungen |Timeoutfehler für Client und Server |
|     Anforderungen von Analysedaten |     Mit Fehlercode 304 (Nicht geändert) misslungene GET-Anforderungen |
| Anforderungen, die durch die Speicheranalyse selbst erfolgen, z. B. Protokollerstellung oder -löschung, werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Speicheranalysevorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Protokollformat der Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. | Alle anderen misslungenen anonymen Anforderungen werden nicht protokolliert. Eine vollständige Liste der protokollierten Daten ist in den Themen [Protokollierte Speicheranalysevorgänge und Statusmeldungen](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-logged-operations-and-status-messages) und [Protokollformat der Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics-log-format) dokumentiert. |

### <a name="26-azure-networking-logs"></a>2.6 Azure-Netzwerkprotokolle
Die Netzwerkprotokollierung und -überwachung in Azure ist umfassend und unterteilt sich in zwei übergeordnete Kategorien:

-    [Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-watcher): Die szenariobasierte Netzwerküberwachung wird mit den Features in Network Watcher bereitgestellt. Dieser Dienst umfasst die Paketerfassung, „Nächster Hop“, die IP-Datenflussüberprüfung, die Sicherheitsgruppenansicht und NSG-Datenflussprotokolle. Die Überwachung auf Szenarioebene bietet im Gegensatz zur Überwachung einzelner Netzwerkressourcen eine End-to-End-Ansicht der Netzwerkressourcen.

-    [Ressourcenüberwachung](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-resource-level-monitoring): Die Überwachung auf Ressourcenebene umfasst vier Features: Diagnoseprotokolle, Metriken, Problembehandlung und Resource Health. Alle diese Features arbeiten auf Netzwerkressourcenebene.

![Azure-Netzwerkprotokolle](./media/azure-log-audit/azure-log-audit-fig4.png)

Network Watcher ist ein regionaler Dienst, mit dem Sie Bedingungen auf der Ebene von Netzwerkszenarien in Azure überwachen und diagnostizieren können. Die Tools zur Netzwerkdiagnose und -visualisierung von Network Watcher helfen Ihnen dabei, Ihr Netzwerk in Azure zu verstehen, Diagnosen durchzuführen und Einblicke zu gewinnen.

**NSG-Datenflussprotokollierung**: Mithilfe von Datenflussprotokollen für Netzwerksicherheitsgruppen können Sie Protokolle zum Datenverkehr erfassen, der von den Sicherheitsregeln in der Gruppe zugelassen oder verweigert wird. Diese Datenflussprotokolle sind im JSON-Format verfasst und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

### <a name="27-network-security-group-flow-logging"></a>2.7 Protokollieren des Datenflusses von Netzwerksicherheitsgruppen

[Datenflussprotokolle für Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/network-watcher/network-watcher-nsg-flow-logging-overview) sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Datenflussprotokolle sind im JSON-Format verfasst und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

Da sich Datenflussprotokolle auf Netzwerksicherheitsgruppen beziehen, werden sie nicht wie andere Protokolle angezeigt. Datenflussprotokolle werden nur innerhalb eines Speicherkontos gespeichert.

Für Datenflussprotokolle gelten die gleichen Aufbewahrungsrichtlinien wie für andere Protokolle. Die Protokolle verfügen über eine Aufbewahrungsrichtlinie, die auf einen Zeitraum zwischen einem und 365 Tagen festgelegt werden kann. Wenn keine Aufbewahrungsrichtlinie festgelegt wurde, werden die Protokolle unbegrenzt aufbewahrt.

**Diagnoseprotokolle**

Regelmäßige und spontane Ereignisse werden von Netzwerkressourcen erstellt, in Speicherkonten protokolliert und dann an Event Hub oder Log Analytics gesendet. Diese Protokolle geben Einblick in die Integrität einer Ressource. Diese Protokolle können in Tools wie Power BI und Log Analytics angezeigt werden. Informationen zum Anzeigen von Diagnoseprotokollen finden Sie unter [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-networking-analytics).

![Diagnoseprotokolle](./media/azure-log-audit/azure-log-audit-fig5.png)

Diagnoseprotokolle sind für [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-monitor-log), [Netzwerksicherheitsgruppen](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log), Routen und [Application Gateway](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics) verfügbar.

Network Watcher bietet eine Ansicht der Diagnoseprotokolle. Diese Ansicht enthält alle Netzwerkressourcen, die die Diagnoseprotokollierung unterstützen. In dieser Ansicht können Sie Netzwerkressourcen einfach und schnell aktivieren und deaktivieren.


Zusätzlich zu den vorangehenden Protokollierungsfunktionen umfasst Network Watcher derzeit die folgenden Funktionen:
- [Topologie](https://docs.microsoft.com/azure/network-watcher/network-watcher-topology-overview): Bietet eine Ansicht auf Netzwerkebene mit verschiedenen Verbindungen und Beziehungen zwischen Netzwerkressourcen innerhalb einer Ressourcengruppe.

- [Variable Paketerfassung](https://docs.microsoft.com/azure/network-watcher/network-watcher-packet-capture-overview): Erfasst am virtuellen Computer ein- und ausgehende Paketdaten. Erweiterte Filteroptionen und präzise Steuerelemente erlauben z. B. das Festlegen von Zeitraum und Größeneinschränkungen und bieten damit viel Flexibilität. Die Paketdaten können in einem Blobspeicher oder auf dem lokalen Datenträger im CAP-Format gespeichert werden.

-    [IP-Datenflussüberprüfung](https://docs.microsoft.com/azure/network-watcher/network-watcher-ip-flow-verify-overview): Überprüft basierend auf 5-Tupel-Paketparametern (Ziel-IP, Quell-IP, Zielport, Quellport und Protokoll) anhand der Datenflussinformationen, ob ein Paket zugelassen oder verweigert wird. Wenn das Paket durch eine Sicherheitsgruppe verweigert wird, werden die Namen der Regel und der Gruppe, die das Paket verweigert haben, zurückgegeben.

-    [Nächster Hop](https://docs.microsoft.com/azure/network-watcher/network-watcher-next-hop-overview): Ermittelt den nächsten Hop für Pakete, die im Azure-Netzwerkfabric weitergeleitet werden, sodass Sie eine Diagnose auf falsch konfigurierte benutzerdefinierte Routen durchführen können.

-    [Sicherheitsgruppenansicht](https://docs.microsoft.com/azure/network-watcher/network-watcher-security-group-view-overview): Ruft die geltenden und angewendeten Sicherheitsregeln ab, die auf einen virtuellen Computer angewendet werden.

-    [Problembehandlung für Virtual Network-Gateways und -Verbindungen](https://docs.microsoft.com/azure/network-watcher/network-watcher-troubleshoot-manage-rest): Bietet die Möglichkeit zum Beheben von Problemen, die bei Virtual Network-Gateways und -Verbindungen auftreten können.

-    [Grenzwerte für Netzwerkabonnements](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#network-subscription-limits): Ermöglicht die Anzeige der Verwendung von Netzwerkressourcen mit bestimmten Grenzwerten.

### <a name="28-application-insight"></a>2.8 Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) ist ein erweiterbarer, für Webentwickler konzipierter Dienst zur Verwaltung der Anwendungsleistung (Application Performance Management, APM) auf mehreren Plattformen. Verwenden Sie ihn, um Ihre aktiven Webanwendung zu überwachen. Der Dienst erkennt automatisch Leistungsanomalien. Er verfügt über leistungsstarke Analysetools, mit denen Sie Probleme diagnostizieren und nachvollziehen können, wie Ihre App von den Benutzern verwendet wird.

 Der Dienst unterstützt Sie bei der kontinuierlichen Verbesserung der Leistung und Benutzerfreundlichkeit Ihrer App.

 Er lässt sich für Apps auf einer Vielzahl von Plattformen einsetzen, wie z.B. .NET, Node.js oder J2EE – lokal gehostet oder in der Cloud. Der Dienst lässt sich in Ihren DevOps-Prozess integrieren und verfügt über Verbindungspunkte mit verschiedenen Entwicklungstools.

![Application Insights](./media/azure-log-audit/azure-log-audit-fig6.png)

Application Insights ist für Entwicklerteams konzipiert und hilft Ihnen dabei, die Leistung und Verwendung Ihrer App nachzuvollziehen. Der Dienst überwacht:

-    **Anforderungsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, welche Seiten zu welchen Tageszeiten am häufigsten verwendet werden und wo Ihre Benutzer sind. Stellen Sie fest, welche Seiten die beste Leistung aufweisen. Wenn die Antwortzeiten und Fehlerraten bei mehr Anforderungen ansteigen, haben Sie möglicherweise ein Problem mit den Ressourcen.

-    **Abhängigkeitsraten, Antwortzeiten und Fehlerraten**: Finden Sie heraus, ob Sie von externen Diensten verlangsamt werden.

-    **Ausnahmen**: Analysieren Sie die aggregierten Statistiken, oder wählen Sie bestimmte Instanzen aus, und untersuchen Sie die Stapelüberwachung und die zugehörigen Anforderungen. Sowohl die Server- als auch die Browserausnahmen werden gemeldet.

-    **Seitenansichten und Ladeleistung**: Von den Browsern der Benutzer gemeldet.

-    **AJAX-Aufrufe** von Webseiten: Raten, Antwortzeiten und Fehlerraten.

-    **Anzahl von Benutzern und Sitzungen**.

-    **Leistungsindikatoren** von Ihren Windows- oder Linux-Servercomputern, z.B. CPU, Arbeitsspeicher und Netzwerkverwendung.

-    **Hostdiagnose** von Docker oder Azure.

-    **Diagnose-Ablaufverfolgungsprotokolle** aus Ihrer App, sodass Sie Ablaufverfolgungsereignisse mit Anforderungen korrelieren können.

-    **Benutzerdefinierte Ereignisse und Metriken**, die Sie selbst im Client- oder Servercode schreiben, um Geschäftsereignisse zu verfolgen, wie z.B. verkaufte oder gewonnene Spiele.

**Liste der Integrationsszenarien und Beschreibungen**:

| Integrationsszenarien | Beschreibung |
| --------------------- | :---------- |
|[Anwendungszuordnung](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-app-map)|Die Komponenten der App mit wichtigen Metriken und Warnungen.||
|[Diagnosesuche für Instanzdaten](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-diagnostic-search)| Suchen und filtern Sie Ereignisse wie Anforderungen, Ausnahmen, Abhängigkeitsaufrufe, Protokollablaufverfolgungen und Seitenaufrufe.||
|[Metrik-Explorer für aggregierte Daten](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-metrics-explorer)|Durchsuchen, filtern und segmentieren Sie aggregierte Daten wie z.B. Anforderungs-, Fehler- und Ausnahmeraten, Antwortzeiten und Seitenladezeiten.||
|[Dashboards](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-dashboards#dashboards)|Kombinieren Sie Daten aus mehreren Ressourcen, und geben Sie sie für andere frei. Dies ist sehr gut für Anwendungen mit mehreren Komponenten und für die kontinuierliche Anzeige im Teamraum geeignet.||
|[Live Metrics Stream](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-live-stream)|Wenn Sie einen neuen Build bereitstellen, sehen Sie sich diese beinahe in Echtzeit verfügbaren Leistungsindikatoren an, um sicherzustellen, dass alles wie erwartet funktioniert.||
|[Analyse](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-analytics)|Beantworten Sie schwierige Fragen zur Leistung und Nutzung Ihrer App mithilfe dieser leistungsstarken Abfragesprache.||
|[Automatische und manuelle Warnungen](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-alerts)|Automatische Warnungen sind an die normalen Telemetriemuster Ihrer App angepasst und werden ausgelöst, wenn etwas nicht den üblichen Mustern entspricht. Sie können auch Warnungen auf bestimmten Ebenen von benutzerdefinierten oder standardmäßigen Metriken festlegen.||
|[Visual Studio](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio)|Zeigen Sie Leistungsdaten im Code an. Wechseln Sie von Stapelüberwachungen zum Code.||
|[Power BI](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-power-bi)|Integrieren Sie Nutzungsmetriken und andere Business Intelligence-Daten.||
|[REST-API](https://dev.applicationinsights.io/)|Schreiben Sie Code zum Ausführen von Abfragen für Ihre Metriken und Rohdaten.||
|[Fortlaufendem Export](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-export-telemetry)|Exportieren Sie große Mengen von Rohdaten in den Speicher, wenn sie eintreffen.||

### <a name="29-azure-security-center-alerts"></a>2.9 Azure Security Center-Warnungen
[Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro) erfasst, analysiert und vereinigt automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von verbundenen Partnerlösungen, z. B. Lösungen zum Schutz von Firewalls und Endpunkten, um echte Bedrohungen zu erkennen und falsch positive Ergebnisse zu reduzieren. Eine Liste mit priorisierten Sicherheitswarnungen wird im Security Center zusammen mit den Informationen angezeigt, die Sie zum schnellen Untersuchen des Problems benötigen. Außerdem sind Empfehlungen zum Reagieren auf einen Angriff vorhanden.

Bei der Bedrohungserkennung von Security Center werden automatisch Sicherheitsinformationen von Ihren Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen gesammelt. Diese Informationen, bei denen es sich häufig um korrelierende Informationen aus mehreren Quellen handelt, werden analysiert, um Bedrohungen zu identifizieren. Sicherheitswarnungen werden in Security Center zusammen mit Empfehlungen zur Lösung der Bedrohung priorisiert.

![Azure Security Center](./media/azure-log-audit/azure-log-audit-fig7.png)

Für Security Center werden professionelle Sicherheitsanalysen genutzt, die weit über signaturbasierte Ansätze hinausgehen. Bahnbrechende Weiterentwicklungen der Big Data- und [Machine Learning](https://azure.microsoft.com/blog/machine-learning-in-azure-security-center/)-Technologie werden angewendet, um Ereignisse im gesamten Cloudfabric auszuwerten. So können Bedrohungen erkannt werden, die bei Verwendung von manuellen Konzepten nicht identifiziert werden können, und die Entwicklung von Angriffen kann vorhergesagt werden. Zu diesen Sicherheitsanalysen gehört Folgendes:

-    **Integrierte Informationen zu Bedrohungen**: Es wird nach bekannten Angreifern gesucht. Hierzu werden globale Bedrohungsinformationen (Threat Intelligence) von Microsoft-Produkten und -Diensten, von der Microsoft Digital Crimes Unit (DCU), vom Microsoft Security Response Center (MSRC) sowie von externen Feeds angewendet.

-    **Verhaltensanalyse**: Es werden bekannte Muster angewendet, um schädliches Verhalten zu erkennen.

-    **Anomalieerkennung**: Es werden statistische Profile erstellt, um typische Verlaufsdaten zu erhalten. Sie werden benachrichtigt, wenn es zu Abweichungen von der Baseline kommt, die einem potenziellen Angriffsvektor entsprechen.


Viele für den Sicherheitsbetrieb und die Bearbeitung von Incidents zuständige Teams nutzen eine SIEM-Lösung (Security Information and Event Management) als Ausgangspunkt für die Selektierung und Untersuchung von Sicherheitswarnungen. Mit der Azure-Protokollintegration können Kunden Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen gesammelte Sicherheitsereignisse für den virtuellen Computer nahezu in Echtzeit mit ihrer Protokollanalyse oder SIEM-Lösung synchronisieren.


## <a name="30-log-analytics"></a>3.0 Log Analytics

Log Analytics ist ein Dienst in der [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview), mit dem Sie Daten sammeln und analysieren können, die von Ressourcen in Ihren Cloud- und lokalen Umgebungen generiert werden. Der Dienst bietet Echtzeiteinblicke mithilfe integrierter Suchfunktionen und benutzerdefinierter Dashboards, sodass Sie Millionen von Datensätzen über all Ihre Workloads und Server hinweg unabhängig vom physischen Standort analysieren können.

![Log Analytics](./media/azure-log-audit/azure-log-audit-fig8.png)

Im Mittelpunkt von Log Analytics steht das OMS-Repository, das in der Azure-Cloud gehostet wird. Konfigurieren Sie Datenquellen und fügen Sie Ihrem Abonnement Lösungen hinzu, um Daten aus verbundenen Quellen im Repository zu sammeln. Datenquellen und Lösungen erzeugen verschiedene Arten von Datensätzen, die jeweils über einen eigenen Eigenschaftensatz verfügen, aber dennoch in Abfragen im Repository gemeinsam analysiert werden können. So können Sie die gleichen Tools und Methoden für verschiedene Arten von Daten verwenden, die aus verschiedenen Quellen gesammelt werden.

Bei den verbundenen Quellen handelt es sich um die Computer und anderen Ressourcen, die die Daten generieren, die von Log Analytics gesammelt werden. Hierzu können Agents gehören, die auf direkt verbundenen [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-windows-agents)- oder [Linux](https://docs.microsoft.com/azure/log-analytics/log-analytics-linux-agents)-Computern installiert sind, oder Agents in einer [verbundenen System Center Operations Manager-Verwaltungsgruppe](https://docs.microsoft.com/azure/log-analytics/log-analytics-om-agents). Log Analytics kann auch Daten aus [Azure-Speichern](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) sammeln.

[Datenquellen](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources) sind die verschiedenen Arten von Daten, die aus jeder verbundenen Quelle gesammelt werden. Hierzu zählen Ereignisse und [Leistungsdaten](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-performance-counters) aus [Windows](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)- und Linux-Agents sowie Quellen wie [IIS-Protokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs) und [benutzerdefinierte Textprotokolle](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-custom-logs). Sie konfigurieren jede Datenquelle, aus der Sie Daten sammeln möchten, und die Konfiguration wird automatisch an jede verbundene Quelle übermittelt.

[Protokolle und Metriken für Azure-Dienste](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-storage) können auf vier Arten erfasst werden:
1.    Azure-Diagnosen direkt an Log Analytics („Diagnose“ in der folgenden Tabelle)

2.    Azure-Diagnosen über Azure Storage an Log Analytics („Storage“ in der folgenden Tabelle)

3.    Connectors für Azure-Dienste („Connectors“ in der folgenden Tabelle)

4.    Skripts zum Sammeln und anschließenden Veröffentlichen von Daten in Log Analytics (Leerstellen in der folgenden Tabelle und für nicht aufgeführte Dienste)

| Dienst | Ressourcentyp | Protokolle | Metriken | Lösung |
| :------ | :------------ | :--- | :------ | :------- |
|Anwendungsgateways|    Microsoft.Network/<br>applicationGateways|    Diagnose|Diagnose|    [Azure Application](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics) [Gateway-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-application-gateway-analytics-solution-in-log-analytics)|
|Application Insights||         Connector|    Connector|    [Application Insights](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)-[Connector (Vorschau)](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)|
|Automation-Konten|    Microsoft.Automation/<br>AutomationAccounts|    Diagnose||         [Weitere Informationen](https://docs.microsoft.com/en-us/azure/automation/automation-manage-send-joblogs-log-analytics)|
|Batch-Konten|    Microsoft.Batch/<br>batchAccounts|    Diagnose|    Diagnose||
|Klassische Clouddienste||         Speicher||         [Weitere Informationen](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage-iis-table)|
|Cognitive Services|    Microsoft.CognitiveServices/<br>accounts|         Diagnose|||
|Data Lake Analytics|    Microsoft.DataLakeAnalytics/<br>accounts|    Diagnose|||
|Data Lake Store|    Microsoft.DataLakeStore/<br>accounts|    Diagnose|||
|Event Hub-Namespace|    Microsoft.EventHub/<br>Namespaces|    Diagnose|    Diagnose||
|IoT Hubs|    Microsoft.Devices/<br>IotHubs||         Diagnose||
|Key Vault|    Microsoft.KeyVault/<br>Tresore|    Diagnose     ||    [KeyVault-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-key-vault)|
|Load Balancer|    Microsoft.Network/<br>loadBalancers|    Diagnose|||
|Logik-Apps|    Microsoft.Logic/<br>Workflows|     Diagnose|    Diagnose||
||Microsoft.Logic/<br>integrationAccounts||||
|Netzwerksicherheitsgruppen|    Microsoft.Network/<br>networksecuritygroups|Diagnose||     [Azure-Netzwerksicherheitsgruppen-Analyse](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-networking-analytics#azure-network-security-group-analytics-solution-in-log-analytics)|
|Recovery-Tresore|    Microsoft.RecoveryServices/<br>vaults|||[Azure Recovery Services Analytics (Vorschau)](https://github.com/krnese/AzureDeploy/blob/master/OMS/MSOMS/Solutions/recoveryservices/)|
|Suchdienste|    Microsoft.Search/<br>searchServices|    Diagnose|    Diagnose||
|Service Bus-Namespace|    Microsoft.ServiceBus/<br>namespaces|    Diagnose|Diagnose|    [Service Bus Analytics (Vorschau)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-servicebus-solution)|
|Service Fabric||         Speicher||     [Service Fabric-Analysen (Vorschau)](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-service-fabric)|
|SQL (v12)|    Microsoft.Sql/<br>servers/<br>Datenbanken||          Diagnose||
||Microsoft.Sql/<br>servers/<br>elasticPools||||
|Speicher|||              Skript|    [Azure Storage Analytics (Vorschau)](https://github.com/Azure/azure-quickstart-templates/tree/master/oms-azure-storage-analytics-solution)|
|Virtual Machines|    Microsoft.Compute/<br>virtualMachines|    Erweiterung|    Durchwahl||
||||Diagnose||
|VM-Skalierungsgruppen|    Microsoft.Compute/<br>virtualMachines      ||Diagnose||
||Microsoft.Compute/<br>virtualMachineScaleSets/<br>virtualMachines||||
|Webserverfarmen|Microsoft.Web/<br>serverfarms||     Diagnose
|Websites|    Microsoft.Web/<br>sites ||         Diagnose|    [Weitere Informationen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-webappazure-oms-monitoring)|
||Microsoft.Web/<br>sites/<br>slots|||||


## <a name="40-log-integration-with-on-premises-siem-systems"></a>4.0 Protokollintegration mit lokalen SIEM-Systemen
Mit der [Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324) können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen **SIEM-Systeme** (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren.

![Protokollintegration](./media/azure-log-audit/azure-log-audit-fig9.png)

Die Azure-Protokollintegration erfasst Azure-Diagnosen von Ihren virtuellen Windows-Computern (WAD), Azure-Aktivitätsprotokollen, Azure Security Center-Warnungen und Azure-Ressourcenanbieterprotokollen. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Assets (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.



Die Azure-Protokollintegration unterstützt derzeit die Integration von Azure-Aktivitätsprotokollen, Windows-Ereignisprotokollen vom virtuellen Computer unter Windows in Ihr Azure-Abonnement, Azure Security Center-Warnungen, Azure-Diagnoseprotokolle sowie Azure Active Directory-Überwachungsprotokolle.

| Protokolltyp | Log Analytics unterstützt JSON (Splunk, ArcSight, Qradar) |
| :------- | :-------------------------------------------------------- |
|AAD-Überwachungsprotokolle|    Ja|
|Aktivitätsprotokolle|    Ja|
|ASC-Warnungen    |Ja|
|Diagnoseprotokolle (Ressourcenprotokolle)|    Ja|
|VM-Protokolle|    Ja, über weitergeleitete Ereignisse und nicht über JSON|


In der folgenden Tabelle werden die Protokollkategorie und Details der SIEM-Integration erläutert.

[Erste Schritte mit der Azure-Protokollintegration](https://docs.microsoft.com/azure/security/security-azure-log-integration-get-started): In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure-WAD-Speicher sowie Azure-Aktivitätsprotokolle, Azure Security Center-Warnungen und Azure Active Directory-Überwachungsprotokolle integrieren.

Integrationsszenarien

-    [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.

-    [Azure log Integration frequently asked questions (FAQ)](https://docs.microsoft.com/azure/security/security-azure-log-integration-faq) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.

-    [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](https://docs.microsoft.com/azure/security-center/security-center-integrating-alerts-with-log-integration) : In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.

## <a name="next-steps"></a>Nächste Schritte

- [Überwachung und Protokollierung](https://www.microsoft.com/trustcenter/security/auditingandlogging)

Schützen von Daten durch Beibehalten der Sichtbarkeit und schnelles Reagieren auf zeitnahe Sicherheitswarnungen

- [Sicherheitsprotokollierung und Überwachungprotokollsammlung in Azure](https://azure.microsoft.com/resources/videos/security-logging-and-audit-log-collection/)

Welche Einstellungen durchgesetzt werden müssen, um sicherzustellen, dass Ihre Azure-Instanzen die richtigen Sicherheits- und Überwachungsprotokolle sammeln.

- [Konfigurieren von Überwachungseinstellungen für eine Websitesammlung](https://support.office.com/article/Configure-audit-settings-for-a-site-collection-A9920C97-38C0-44F2-8BCB-4CF1E2AE22D2?ui=&rs=&ad=US)

Als Administrator einer Websitesammlung können Sie den Verlauf der Aktionen abrufen, die von einem bestimmten Benutzer durchgeführt werden. Zudem können Sie den Verlauf der Aktionen abrufen, die während eines bestimmten Zeitraums durchgeführt werden. 

- [Suchen des Überwachungsprotokolls im Office 365 Security & Compliance Center](https://support.office.com/article/Search-the-audit-log-in-the-Office-365-Security-Compliance-Center-0d4d0f35-390b-4518-800e-0c7ec95e946c?ui=&rs=&ad=US)

Mit dem Office 365 Security & Compliance Center können Sie das vereinheitlichte Überwachungsprotokoll suchen, um Benutzer- und Administratoraktivitäten in Ihrer Office 365-Organisation anzuzeigen.



