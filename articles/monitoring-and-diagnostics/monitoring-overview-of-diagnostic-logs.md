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
ms.date: 03/12/2017
ms.author: johnkem; magoedte
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 5675a65e3b48e39f44dc320b7b87910ab759b764
ms.lasthandoff: 03/14/2017


---
# <a name="collect-and-consume-diagnostic-data-from-your-azure-resources"></a>Erfassen und Nutzen von Diagnosedaten aus Ihren Azure-Ressourcen

## <a name="what-are-azure-diagnostic-logs"></a>Was sind Azure-Diagnoseprotokolle?
**Azure-Diagnoseprotokolle** sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. Windows-Systemereignisprotokolle sind z.B. eine Kategorie des Diagnoseprotokolls für virtuelle Computer und Blob-, Tabellen- und Warteschlangenprotokolle sind Kategorien der Diagnoseprotokolle für Speicherkonten.

Diagnoseprotokolle unterscheiden sich vom [Aktivitätsprotokoll (früher als Überwachungsprotokoll, oder Betriebsprotokoll bezeichnet)](monitoring-overview-activity-logs.md). Das Aktivitätsprotokoll bietet Einblicke in Vorgänge, die für Ressourcen Ihres Abonnements durchgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

Die hier beschriebene neue Art von Diagnoseprotokollen wird nicht von allen Ressourcen unterstützt. Dieser Artikel enthält einen Abschnitt, in dem aufgeführt ist, welche Ressourcentypen die neuen Diagnoseprotokolle unterstützen.

![Diagnoseprotokolle im Vergleich zu anderen Protokolltypen ](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_vs_other_logs_v5.png)

Abbildung 1: Diagnoseprotokolle im Vergleich zu anderen Protokolltypen

## <a name="what-you-can-do-with-diagnostic-logs"></a>Verwendungsmöglichkeiten für Diagnoseprotokolle
Im Anschluss sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle aufgeführt:

![Logische Position von Diagnoseprotokollen](./media/monitoring-overview-of-diagnostic-logs/Diagnostics_Logs_Actions.png)


* Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem [**Speicherkonto**](monitoring-archive-diagnostic-logs.md) gespeichert werden. Mithilfe der **Diagnoseeinstellungen**können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
* [Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie Power BI an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md) streamen.
* Diagnoseprotokolle können mit [OMS Log Analytics](../log-analytics/log-analytics-azure-storage.md)

Sie können ein Speicherkonto oder Event Hub-Namespace verwenden, das sich nicht im gleichen Abonnement befindet wie das, das Protokolle angibt. Der Benutzer, der die Einstellung konfiguriert, benötigt den entsprechenden RBAC-Zugriff auf beide Abonnements.

## <a name="diagnostic-settings"></a>Diagnoseeinstellungen
Diagnoseprotokolle für computefremde Ressourcen werden mithilfe von Diagnoseeinstellungen konfiguriert. **Diagnoseeinstellungen** für ein Ressourcensteuerelement können Sie Folgendes festlegen:

* Wohin Diagnoseprotokolle gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics)
* Welche Protokollkategorien gesendet werden sollen
* Wie lange die einzelnen Protokollkategorien in einem Speicherkonto beibehalten werden sollen
    - Wenn für die Beibehaltungsdauer&0; Tage festgelegt sind, bedeutet dies, dass Protokolle unbegrenzt beibehalten werden. Andernfalls kann als Wert die Anzahl von Tagen (1 bis 2.147.483.647) festgelegt werden.
    - Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.
    - Aufbewahrungsrichtlinien werden pro Tag angewendet, sodass Protokolle am Ende eines Tages (UTC) ab dem Tag, der nun außerhalb der Aufbewahrungsrichtlinie liegt, gelöscht werden. Beispiel: Wenn Sie eine Aufbewahrungsrichtlinie für einen Tag verwenden, werden heute am Anfang des Tages die Protokolle von vorgestern gelöscht.

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

## <a name="supported-services-and-schema-for-diagnostic-logs"></a>Unterstützte Dienste und Schema für Diagnoseprotokolle
Das Schema für Diagnoseprotokolle variiert abhängig von der Ressource und der Protokollkategorie.   

| Dienst | Schema und Dokumente |
| --- | --- |
| Lastenausgleichsmodul |[Log Analytics für den Azure Load Balancer](../load-balancer/load-balancer-monitor-log.md) |
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
| SERVICE BUS |[Azure Service Bus Diagnoseprotokolle](../service-bus-messaging/service-bus-diagnostic-logs.md) |


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
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)

