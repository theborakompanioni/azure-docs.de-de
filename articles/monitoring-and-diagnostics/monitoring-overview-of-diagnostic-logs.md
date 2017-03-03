---
title: "Übersicht über Azure-Diagnoseprotokolle | Microsoft Docs"
description: Hier erfahren Sie, worum es sich bei Azure-Diagnoseprotokollen handelt und wie Sie mithilfe von Diagnoseprotokollen Ereignisse innerhalb einer Azure-Ressource nachvollziehen.
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: fe8887df-b0e6-46f8-b2c0-11994d28e44f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: johnkem; magoedte
translationtype: Human Translation
ms.sourcegitcommit: fbc96a248de20b67a72e6a0150fe4b9b754ec4fe
ms.openlocfilehash: d61ec29026ae5bbbdf33d7810e2e35c4d6bee1e7
ms.lasthandoff: 02/10/2017


---
# <a name="overview-of-azure-diagnostic-logs"></a>Übersicht über Azure-Diagnoseprotokolle
**Azure-Diagnoseprotokolle** sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. (Windows-Systemereignisprotokolle sind beispielsweise eine Diagnoseprotokollkategorie für virtuelle Computer; Blob-, Tabellen- und Warteschlangenprotokolle sind Diagnoseprotokollkategorien für Speicherkonten.) Darüber hinaus unterscheiden sich Diagnoseprotokolle vom [Aktivitätsprotokoll (ehemals Überwachungsprotokoll oder Betriebsprotokoll genannt)](monitoring-overview-activity-logs.md), das Informationen zu den Vorgängen liefert, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden. Die hier beschriebene neue Art von Diagnoseprotokollen wird nicht von allen Ressourcen unterstützt. Die weiter unten bereitgestellte Liste mit unterstützten Diensten gibt Aufschluss darüber, welche Ressourcentypen die neuen Diagnoseprotokolle unterstützen.

![Logische Position von Diagnoseprotokollen](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## <a name="what-you-can-do-with-diagnostic-logs"></a>Verwendungsmöglichkeiten für Diagnoseprotokolle
Im Anschluss sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle aufgeführt:

* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [**Speicherkonto**](monitoring-archive-diagnostic-logs.md) gespeichert werden. Mithilfe der **Diagnoseeinstellungen**können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* [Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) streamen.
* Diagnoseprotokolle können mit [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)

Das Speicherkonto oder der Event Hub-Namespace muss sich nicht unter demselben Abonnement befinden, wie die Ressource, die Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, den entsprechenden RBAC-Zugriff auf beide Abonnements hat.

## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Diagnoseprotokolle für computefremde Ressourcen werden mithilfe von Diagnoseeinstellungen konfiguriert. **Diagnoseeinstellungen** für ein Ressourcensteuerelement können Sie Folgendes festlegen:

* Wohin Diagnoseprotokolle gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics)
* Welche Protokollkategorien gesendet werden sollen
* Wie lange die einzelnen Protokollkategorien in einem Speicherkonto aufbewahrt werden sollen. Bei Angabe einer Aufbewahrungsdauer von null Tagen werden die Protokolle dauerhaft aufbewahrt. Andernfalls kann dieser Wert zwischen 1 und 2147483647 liegen. Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert. Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

Diese Einstellungen können für eine Ressource ganz einfach über das Blatt „Diagnose“ im Azure-Portal, mithilfe von Azure PowerShell oder CLI-Befehlen oder über die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx) konfiguriert werden.

> [!WARNING]
> Bei Diagnoseprotokollen und Metriken für Computeressourcen (etwa virtuelle Computer oder Service Fabric) wird [ein separater Mechanismus für die Konfiguration und Auswahl von Ausgaben](../azure-diagnostics.md)verwendet.
>
>

## <a name="how-to-enable-collection-of-diagnostic-logs"></a>Gewusst wie: Aktivieren der Erfassung von Diagnoseprotokollen
Die Erfassung von Diagnoseprotokollen kann im Zuge der Ressourcenerstellung und später im Portal über das Blatt der Ressource aktiviert werden. Darüber hinaus können Diagnoseprotokolle jederzeit mithilfe von Azure PowerShell oder CLI-Befehlen oder mithilfe der Azure Monitor-REST-API aktiviert werden.

> [!TIP]
> Diese Anweisungen lassen sich unter Umständen nicht immer direkt auf jede Ressource anwenden. Informationen zu Sonderschritten, die ggf. für bestimmte Ressourcentypen erforderlich sind, finden Sie unter den Schemalinks am Ende dieser Seite.
>
>

[In diesem Artikel erfahren Sie, wie Sie beim Erstellen einer Ressource mithilfe einer Ressourcenvorlage Diagnoseeinstellungen aktivieren.](monitoring-enable-diagnostic-logs-using-template.md)

### <a name="enable-diagnostic-logs-in-the-portal"></a>Aktivieren von Diagnoseprotokollen über das Portal
Sie können die Diagnoseprotokolle im Azure-Portal aktivieren, wenn Sie Computeressourcentypen erstellen, indem Sie die Azure-Diagnoseerweiterung für Windows oder Linux aktivieren:

1. Navigieren Sie zu **Neu** , und wählen Sie die gewünschte Ressource.
2. Wählen Sie nach dem Konfigurieren der grundlegenden Einstellungen und dem Auswählen einer Größe auf dem Blatt **Einstellungen** unter **Überwachung** die Option **Aktiviert** und anschließend ein Speicherkonto aus, in dem die Diagnoseprotokolle gespeichert werden sollen. Wenn Sie Diagnoseinformationen an ein Speicherkonto senden, werden Ihnen die üblichen Datengebühren für Speicherung und Transaktionen in Rechnung gestellt.

   ![Aktivieren von Diagnoseprotokollen im Rahmen der Ressourcenerstellung](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3. Klicken Sie auf **OK** , und erstellen Sie die Ressource.

Sie können Diagnoseprotokolle nach dem Erstellen einer Ressource über das Azure-Portal für Nicht-Computeressourcen folgendermaßen aktivieren:

1. Wechseln Sie zum Blatt für die Ressource, und öffnen Sie das Blatt **Diagnose** .
2. Klicken Sie auf **Ein** , und wählen Sie ein Speicherkonto und/oder eine Event Hub-Instanz aus.

   ![Aktivieren von Diagnoseprotokollen nach der Ressourcenerstellung](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3. Wählen Sie unter **Protokolle** die zu erfassenden oder zu streamenden **Protokollkategorien** aus.
4. Klicken Sie auf **Speichern**.

### <a name="enable-diagnostic-logs-via-powershell"></a>Aktivieren von Diagnoseprotokollen über PowerShell
Mit den folgenden Befehlen können Sie Diagnoseprotokolle über Azure PowerShell-Cmdlets aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -StorageAccountId [your storage account id] -Enabled $true
```

Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```powershell
    Set-AzureRmDiagnosticSetting -ResourceId [your resource id] -WorkspaceId [resource id of the log analytics workspace] -Enabled $true
```

Sie können die Ressourcen-ID mit folgendem Befehl aus Ihrem Log Analytics-Arbeitsbereich abrufen:

```powershell
(Get-AzureRmOperationalInsightsWorkspace).ResourceId
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-diagnostic-logs-via-cli"></a>Aktivieren von Diagnoseprotokollen über die CLI
Mit den folgenden Befehlen können Sie Diagnoseprotokolle über die Azure-Befehlszeilenschnittstelle aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true
```

Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.

Verwenden Sie den folgenden Befehl, um das Senden von Diagnoseprotokollen an einen Log Analytics-Arbeitsbereich zu aktivieren:

```azurecli
    azure insights diagnostic set --resourceId <resourceId> --workspaceId <resource id of the log analytics workspace> --enabled true
```

Sie können diese Parameter miteinander kombinieren, um mehrere Ausgabeoptionen zu aktivieren.

### <a name="enable-diagnostic-logs-via-rest-api"></a>Aktivieren von Diagnoseprotokollen über die REST-API
Informationen zum Ändern der Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## <a name="manage-diagnostic-settings-in-the-portal"></a>Verwalten von Diagnoseeinstellungen im Portal
Um sicherzustellen, dass all Ihre Ressourcen korrekt mit den Diagnoseeinstellungen eingerichtet wurden, können Sie im Portal zum Blatt **Überwachen** navigieren und das Blatt **Diagnoseprotokolle** öffnen.

![Das Blatt „Diagnoseprotokolle“ im Portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-nav.png)

Möglicherweise müssen Sie auf „Weitere Dienste“ klicken, um das Blatt „Überwachung“ zu finden.

Auf diesem Blatt können Sie alle Ressourcen anzeigen und filtern, die Diagnoseprotokolle unterstützen, um festzustellen, ob die Diagnose für sie aktiviert ist und in welchem Speicherkonto, Event Hub und/oder Log Analytics-Arbeitsbereich diese Protokolle eingehen.

![Ergebnisse auf dem Blatt „Diagnoseprotokolle“ im Portal](./media/monitoring-overview-of-diagnostic-logs/manage-portal-blade.png)

Durch Klicken auf eine Ressource werden alle Protokolle angezeigt, die im Speicherkonto gespeichert sind. Außerdem können Sie die Diagnoseeinstellungen ausschalten oder ändern. Klicken Sie auf das Symbol „Herunterladen“, um Protokolle für einen bestimmten Zeitraum herunterzuladen.

![Das Blatt „Diagnoseprotokolle“ für eine Ressource](./media/monitoring-overview-of-diagnostic-logs/manage-portal-logs.png)

> [!NOTE]
> Nur wenn Sie die Diagnoseeinstellungen so konfiguriert haben, dass sie in einem Speicherkonto gespeichert werden, werden Diagnoseprotokolle in dieser Ansicht angezeigt und stehen zum Download zur Verfügung.
>
>

Wenn Sie auf den Link für **Diagnoseeinstellungen** klicken, wird das Blatt „Diagnoseeinstellungen“ geöffnet, auf dem Sie die Diagnoseeinstellungen für die ausgewählte Ressource aktivieren, deaktivieren oder ändern können.

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Unterstützte Dienste und Schema für Diagnoseprotokolle
Das Schema für Diagnoseprotokolle variiert abhängig von der Ressource und der Protokollkategorie. Im Anschluss finden Sie die unterstützten Dienste und das zugehörige Schema:

| Dienst | Schema und Dokumente |
| --- | --- |
| Lastenausgleichsmodul |[Protokollanalysen für den Azure-Lastenausgleich (Vorschau)](../load-balancer/load-balancer-monitor-log.md) |
| Netzwerksicherheitsgruppen |[Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) |
| Anwendungsgateways |[Diagnoseprotokollierung für Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Schlüsseltresor |[Azure-Schlüsseltresor-Protokollierung](../key-vault/key-vault-logging.md) |
| Azure Search |[Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../search/search-traffic-analytics.md) |
| Data Lake-Speicher |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Data Lake Analytics |[Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Logik-Apps |[Benutzerdefiniertes Logic Apps-B2B-Nachverfolgungsschema](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md) |
| Azure Batch |[Diagnoseprotokolle für Azure Batch](../batch/batch-diagnostics.md) |
| Azure-Automatisierung |[Protokollanalysen für Azure Automation](../automation/automation-manage-send-joblogs-log-analytics.md) |
| Event Hubs |[Azure Event Hubs-Diagnoseprotokolle](../event-hubs/event-hubs-diagnostic-logs.md) |
| Stream Analytics |[Auftragsdiagnoseprotokolle](../stream-analytics/stream-analytics-job-diagnostic-logs.md) |
| Service Bus |Kein Schema verfügbar. |


## <a name="supported-log-categories-per-resource-type"></a>Unterstützte Protokollkategorien pro Ressourcentyp
|Ressourcentyp|Kategorie|Anzeigename der Kategorie|
|---|---|---|
|Microsoft.Automation/automationAccounts|JobLogs|Auftragsprotokolle|
|Microsoft.Automation/automationAccounts|JobStreams|Auftragsdatenströme|
|Microsoft.Batch/batchAccounts|ServiceLog|Dienstprotokolle|
|Microsoft.DataLakeAnalytics/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeAnalytics/accounts|Requests|Anforderungsprotokolle|
|Microsoft.DataLakeStore/accounts|Audit|Überwachungsprotokolle|
|Microsoft.DataLakeStore/accounts|Requests|Anforderungsprotokolle|
|Microsoft.EventHub/namespaces|ArchiveLogs|Archivprotokolle|
|Microsoft.EventHub/namespaces|OperationalLogs|Betriebsprotokolle|
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
|Microsoft.Search/searchServices|OperationLogs|Vorgangsprotokolle|
|Microsoft.ServerManagement/nodes|RequestLogs|Anforderungsprotokolle|
|Microsoft.ServiceBus/namespaces|OperationalLogs|Betriebsprotokolle|
|Microsoft.StreamAnalytics/streamingjobs|Ausführung|Ausführung|
|Microsoft.StreamAnalytics/streamingjobs|Erstellen|Erstellen|

## <a name="next-steps"></a>Nächste Schritte
* [Streamen von Diagnoseprotokollen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
* [Ändern der Diagnoseeinstellungen mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
* [Analysieren der Protokolle mit OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)

