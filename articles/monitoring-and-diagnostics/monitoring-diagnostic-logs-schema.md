---
title: "Azure-Diagnoseprotokolle – unterstützte Dienste, Schemas und Kategorien | Microsoft-Dokumentation"
description: "Erläuterung der unterstützten Dienste und Ereignisschemas für Azure-Diagnose-Protokolle."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: aa4fa6e0310b2725005dfa34e3225c89fb4282d6
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="supported-services-schemas-and-categories-for-azure-diagnostic-logs"></a>Unterstützte Dienste, Schemas und Kategorien für Azure-Diagnoseprotokolle

[Azure-Ressourcendiagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md) sind von Ihren Azure-Ressourcen ausgegebene Protokolle, die den Betrieb der jeweiligen Ressource beschreiben. Diese Protokolle sind ressourcentypspezifisch. In diesem Artikel beschreiben wir den Satz von unterstützten Diensten und Ereignisschemas für Ereignisse, der vom jeweiligen Dienst ausgegeben wird. Dieser Artikel enthält auch eine vollständige Liste der pro Ressourcentyp verfügbaren Protokollkategorien.

## <a name="supported-services-and-schemas-for-resource-diagnostic-logs"></a>Unterstützte Dienste und Schemas für Ressourcendiagnoseprotokolle
Das Schema für Diagnoseprotokolle für Ressourcen variiert abhängig von der Ressource und der Protokollkategorie.   

| Dienst | Schema und Dokumente |
| --- | --- |
| API Management | Schema nicht verfügbar. |
| Anwendungsgateways |[Diagnoseprotokollierung für Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Azure-Automatisierung |[Protokollanalysen für Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Azure Batch |[Diagnoseprotokolle für Azure Batch](../batch/batch-diagnostics.md) |
| Customer Insights | Schema nicht verfügbar. |
| Content Delivery Network | Schema nicht verfügbar. |
| CosmosDB | Schema nicht verfügbar. |
| Data Lake Analytics |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Data Lake-Speicher |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Event Hubs |[Azure Event Hubs-Diagnoseprotokolle](../event-hubs/event-hubs-diagnostic-logs.md) |
| Schlüsseltresor |[Azure-Schlüsseltresor-Protokollierung](../key-vault/key-vault-logging.md) |
| Lastenausgleichsmodul |[Log Analytics für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
| Logik-Apps |[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Netzwerksicherheitsgruppen |[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) |
| Recovery Services | Schema nicht verfügbar.|
| Suche |[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../search/search-traffic-analytics.md) |
| Server Management | Schema nicht verfügbar. |
| SERVICE BUS |[Azure Service Bus Diagnoseprotokolle](../service-bus-messaging/service-bus-diagnostic-logs.md) |
| Stream Analytics |[Auftragsdiagnoseprotokolle](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |

## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Protokollkategorien pro Ressourcentyp
|Ressourcentyp|Kategorie|Anzeigename der Kategorie|
|---|---|---|
|Microsoft.ApiManagement/service|GatewayLogs|Protokolle im Zusammenhang mit dem ApiManagement-Gateway|
|Microsoft.Automation/automationAccounts|JobLogs|Auftragsprotokolle|
|Microsoft.Automation/automationAccounts|JobStreams|Auftragsdatenströme|
|Microsoft.Automation/automationAccounts|DscNodeStatus|DSC-Knotenstatus|
|Microsoft.Batch/batchAccounts|ServiceLog|Dienstprotokolle|
|Microsoft.Cdn/profiles/endpoints|CoreAnalytics|Ruft die Metriken des Endpunkts ab, z.B. Bandbreite, ausgehenden Datenverkehr usw.|
|Microsoft.CustomerInsights/hubs|AuditEvents|AuditEvents|
|Microsoft.DataLakeAnalytics/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeAnalytics/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DataLakeStore/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeStore/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DocumentDB/databaseAccounts|DataPlaneRequests|DataPlaneRequests|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archivprotokolle|
|Microsoft.EventHub/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.EventHub/namespaces|AutoScaleLogs|Protokolle zur automatischen Skalierung|
|Microsoft.KeyVault/vaults|AuditEvent|Überwachungsprotokolle|
|Microsoft.Logic/workflows|WorkflowRuntime|Diagnoseereignisse zur Workflowlaufzeit|
|Microsoft.Logic/integrationAccounts|IntegrationAccountTrackingEvents|Integrationskonto –Nachverfolgen von Ereignissen|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupEvent|Ereignis der Netzwerksicherheitsgruppe|
|Microsoft.Network/networksecuritygroups|NetworkSecurityGroupRuleCounter|Regelzähler der Netzwerksicherheitsgruppe|
|Microsoft.Network/loadBalancers|LoadBalancerAlertEvent|Load Balancer-Warnereignisse|
|Microsoft.Network/loadBalancers|LoadBalancerProbeHealthStatus|Integritätsstatus der Load Balancer-Stichprobe|
|Microsoft.Network/applicationGateways|ApplicationGatewayAccessLog|Application Gateway-Zugriffsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayPerformanceLog|Application Gateway-Leistungsprotokoll|
|Microsoft.Network/applicationGateways|ApplicationGatewayFirewallLog|Application Gateway-Firewallprotokoll|
|Microsoft.RecoveryServices/Vaults|AzureBackupReport|Azure Backup-Berichtsdaten|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryJobs|Azure Site Recovery-Aufträge|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryEvents|Azure Site Recovery-Ereignisse|
|Microsoft.RecoveryServices/Vaults|AzureSiteRecoveryReplicatedItems|Replizierte Azure Site Recovery-Elemente|
|Microsoft.Search/searchServices|OperationLogs|Vorgangsprotokolle|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.StreamAnalytics/streamingjobs|Ausführung|Ausführung|
|Microsoft.StreamAnalytics/streamingjobs|Erstellen|Erstellen|

## <a name="next-steps"></a>Nächste Schritte

* [Weitere Informationen zu Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md)
* [Streamen von Diagnoseprotokollen für Ressourcen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* Ändern der Diagnoseeinstellungen für Ressourcen mithilfe der Azure Monitor-REST-API ([Service Diagnostic Settings](https://msdn.microsoft.com/library/azure/dn931931.aspx) (Diagnoseeinstellungen für Dienste))
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)

