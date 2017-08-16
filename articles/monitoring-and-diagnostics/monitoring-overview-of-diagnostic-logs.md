---
title: "Übersicht über Azure-Diagnoseprotokolle | Microsoft Docs"
description: Hier erfahren Sie, worum es sich bei Azure-Diagnoseprotokollen handelt und wie Sie mithilfe von Diagnoseprotokollen Ereignisse innerhalb einer Azure-Ressource nachvollziehen.
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
ms.date: 08/02/2017
ms.author: johnkem; magoedte
ms.translationtype: HT
ms.sourcegitcommit: 8b857b4a629618d84f66da28d46f79c2b74171df
ms.openlocfilehash: 8961676a60d922912e383937ca38c5d2f89a348a
ms.contentlocale: de-de
ms.lasthandoff: 08/04/2017

---
# <a name="collect-and-consume-log-data-from-your-azure-resources"></a>Erfassen und Nutzen von Protokolldaten aus Ihren Azure-Ressourcen

## <a name="what-are-azure-resource-diagnostic-logs"></a>Was sind Diagnoseprotokolle auf Azure-Ressourcenebene?
**Diagnoseprotokolle auf Azure-Ressourcenebene** sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. Beispielweise sind Netzwerksicherheitsgruppen-Regelzähler und Key Vault-Überwachungen zwei Kategorien von Ressourcenprotokollen.

Diagnoseprotokolle auf Ressourcenebene unterscheiden sich vom [Aktivitätsprotokoll](monitoring-overview-activity-logs.md). Das Aktivitätsprotokoll ermöglicht Einblicke in die Vorgänge, die für Ressourcen Ihres Abonnements mit dem Resource Manager durchgeführt wurden, z.B. das Erstellen eines virtuellen Computers oder das Löschen einer Logik-App. Das Aktivitätsprotokoll ist ein Protokoll auf Abonnementebene. Diagnoseprotokolle auf Ressourcenebene ermöglichen Einblicke in Vorgänge, die für die Ressource selbst durchgeführt wurden, z.B. das Abrufen eines Geheimnisses aus einem Key Vault.

Diagnoseprotokolle auf Ressourcenebene unterscheiden sich auch von Diagnoseprotokollen auf Gastbetriebssystemebene. Diagnoseprotokolle auf Gastbetriebssystemebene werden von einem Agent erfasst, der auf einem virtuellen Computer oder einem anderen Ressourcentyp ausgeführt wird. Für Diagnoseprotokolle auf Ressourcenebene ist kein Agent erforderlich, und ressourcenspezifische Daten werden von der Azure-Plattform selbst erfasst. Für Diagnoseprotokolle auf Gastbetriebssystemebene werden Daten dagegen aus dem Betriebssystem und den Anwendungen erfasst, die auf einem virtuellen Computer ausgeführt werden.

Die hier beschriebene neue Art von Diagnoseprotokollen für Ressourcen wird nicht von allen Ressourcen unterstützt. Dieser Artikel enthält einen Abschnitt, in dem aufgeführt ist, welche Ressourcentypen die neuen Diagnoseprotokolle der Ressourcenebene unterstützen.

![Diagnoseprotokolle für Ressourcen im Vergleich zu anderen Protokolltypen ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

Abbildung 1: Diagnoseprotokolle für Ressourcen im Vergleich zu anderen Protokolltypen

## <a name="what-you-can-do-with-resource-level-diagnostic-logs"></a>Möglichkeiten mit Diagnoseprotokollen auf Ressourcenebene
Hier sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle auf Ressourcenebene aufgeführt:

![Logische Anordnung von Diagnoseprotokollen für Ressourcen](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [**Speicherkonto**](monitoring-archive-diagnostic-logs.md) gespeichert werden. Mithilfe der **Diagnoseeinstellungen für Ressourcen** können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* [Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) streamen.
* Diagnoseprotokolle können mit [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)

Sie können ein Speicherkonto oder Event Hubs-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

## <a name="resource-diagnostic-settings"></a>Diagnoseeinstellungen für Ressourcen
Ressourcendiagnoseprotokolle für computefremde Ressourcen werden mithilfe von Diagnoseeinstellungen für Ressourcen konfiguriert. Mit **Diagnoseeinstellungen für Ressourcen** für ein Ressourcensteuerelement können Sie Folgendes festlegen:

* Wohin Ressourcendiagnoseprotokolle gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics)
* Welche Protokollkategorien gesendet werden sollen
* Wie lange die einzelnen Protokollkategorien in einem Speicherkonto beibehalten werden sollen
    - Wenn für die Beibehaltungsdauer 0 Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

Diese Einstellungen können für eine Ressource ganz einfach über das Blatt „Diagnose“ im Azure-Portal, mithilfe von Azure PowerShell oder CLI-Befehlen oder über die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx) konfiguriert werden.

> [!WARNING]
> Bei Diagnoseprotokollen und Metriken für Computeressourcen (etwa virtuelle Computer oder Service Fabric) wird [ein separater Mechanismus für die Konfiguration und Auswahl von Ausgaben](../azure-diagnostics.md)verwendet.
>
>

## <a name="how-to-enable-collection-of-resource-diagnostic-logs"></a>Gewusst wie: Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen
Die Erfassung von Diagnoseprotokollen für Ressourcen kann [im Zuge der Ressourcenerstellung in einer Resource Manager-Vorlage](./monitoring-enable-diagnostic-logs-using-template.md) und später im Portal über das Blatt der Ressource aktiviert werden. Darüber hinaus können Sie die Erfassung jederzeit mithilfe von Azure PowerShell oder CLI-Befehlen oder mithilfe der Azure Monitor-REST-API aktivieren.

> [!TIP]
> Diese Anweisungen lassen sich unter Umständen nicht immer direkt auf jede Ressource anwenden. Informationen zu Sonderschritten, die ggf. für bestimmte Ressourcentypen erforderlich sind, finden Sie unter den Schemalinks am Ende dieser Seite.
>
>

### <a name="enable-collection-of-resource-diagnostic-logs-in-the-portal"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen im Portal
Sie können die Erfassung von Diagnoseprotokollen für Ressourcen nach dem Erstellen einer Ressource im Azure-Portal aktivieren, indem Sie die folgenden Schritte ausführen:

1. Wechseln Sie zum Blatt für die Ressource, und öffnen Sie das Blatt **Diagnose** .
2. Klicken Sie auf **Ein**, und wählen Sie ein Speicherkonto und/oder eine Event Hub-Instanz aus.

   ![Aktivieren von Diagnoseprotokollen nach der Ressourcenerstellung](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3. Wählen Sie unter **Protokolle** die zu erfassenden oder zu streamenden **Protokollkategorien** aus.
4. Klicken Sie auf **Speichern**.

### <a name="enable-collection-of-resource-diagnostic-logs-via-powershell"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen mit PowerShell
Verwenden Sie die folgenden Befehle, um die Erfassung von Diagnoseprotokollen für Ressourcen mit Azure PowerShell zu aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

Die Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format: `{Service Bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Sie können die Ressourcen-ID mit dem folgenden Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-cli"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen per CLI
Mit den folgenden Befehlen können Sie Diagnoseprotokolle für Ressourcen über die Azure-Befehlszeilenschnittstelle aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

Die Speicherkonto-ID ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Die Service Bus-Regel-ID ist eine Zeichenfolge mit dem folgenden Format: `{Service Bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-collection-of-resource-diagnostic-logs-via-rest-api"></a>Aktivieren der Erfassung von Diagnoseprotokollen für Ressourcen per REST-API
Informationen zum Ändern der Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-resource-diagnostic-settings-in-the-portal"></a>Verwalten von Diagnoseeinstellungen für Ressourcen im Portal
Stellen Sie sicher, dass für alle Ihre Ressourcen Diagnoseeinstellungen festgelegt sind. Navigieren Sie im Portal zum Blatt **Überwachung**, und öffnen Sie das Blatt **Diagnoseprotokolle**.

![Das Blatt „Diagnoseprotokolle“ im Portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Möglicherweise müssen Sie auf „Weitere Dienste“ klicken, um das Blatt „Überwachung“ zu finden.

Auf diesem Blatt können Sie alle Ressourcen anzeigen und filtern, die Diagnoseprotokolle unterstützen, um zu ermitteln, ob die Diagnose dafür aktiviert ist. Sie können auch überprüfen, an welches Speicherkonto, welchen Event Hub und welchen Log Analytics-Arbeitsbereich diese Protokolle übertragen werden.

![Ergebnisse auf dem Blatt „Diagnoseprotokolle“ im Portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Durch Klicken auf eine Ressource werden alle Protokolle angezeigt, die im Speicherkonto gespeichert sind. Außerdem können Sie die Diagnoseeinstellungen ausschalten oder ändern. Klicken Sie auf das Symbol „Herunterladen“, um Protokolle für einen bestimmten Zeitraum herunterzuladen.

![Das Blatt „Diagnoseprotokolle“ für eine Ressource](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [!NOTE]
> Nur wenn Sie die Diagnoseeinstellungen so konfiguriert haben, dass sie in einem Speicherkonto gespeichert werden, werden Diagnoseprotokolle in dieser Ansicht angezeigt und stehen zum Download zur Verfügung.
>
>

Wenn Sie auf den Link für **Diagnoseeinstellungen** klicken, wird das Blatt „Diagnoseeinstellungen“ angezeigt, auf dem Sie die Diagnoseeinstellungen für die ausgewählte Ressource aktivieren, deaktivieren oder ändern können.

## <a name="supported-services-and-schema-for-resource-diagnostic-logs"></a>Unterstützte Dienste und Schema für Diagnoseprotokolle für Ressourcen
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

* [Streamen von Diagnoseprotokollen für Ressourcen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* Ändern der Diagnoseeinstellungen für Ressourcen mithilfe der Azure Monitor-REST-API ([Service Diagnostic Settings](https://msdn.microsoft.com/library/azure/dn931931.aspx) (Diagnoseeinstellungen für Dienste))
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)

