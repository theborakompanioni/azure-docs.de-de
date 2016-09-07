<properties
	pageTitle="Übersicht über Azure-Diagnoseprotokolle | Microsoft Azure"
	description="Hier erfahren Sie, worum es sich bei Azure-Diagnoseprotokollen handelt und wie Sie mithilfe von Diagnoseprotokollen Ereignisse innerhalb einer Azure-Ressource nachvollziehen."
	authors="johnkemnetz"
	manager="rboucher"
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/24/2016"
	ms.author="johnkem"/>

# Übersicht über Azure-Diagnoseprotokolle
**Azure-Diagnoseprotokolle** sind von einer Ressource ausgegebene Protokolle mit umfangreichen, in kurzen Abständen erfassten Betriebsdaten der Ressource. Der Inhalt dieser Protokolle variiert je nach Ressourcentyp. (Windows-Systemereignisprotokolle sind beispielsweise eine Diagnoseprotokollkategorie für virtuelle Computer; Blob-, Tabellen- und Warteschlangenprotokolle sind Diagnoseprotokollkategorien für Speicherkonten.) Darüber hinaus unterscheiden sich Diagnoseprotokolle vom [Aktivitätsprotokoll (ehemals Überwachungsprotokoll oder Betriebsprotokoll genannt)](monitoring-overview-activity-logs.md), das Informationen zu den Vorgängen liefert, die auf Ressourcen in Ihrem Abonnement ausgeführt wurden. Die hier beschriebene neue Art von Diagnoseprotokollen wird nicht von allen Ressourcen unterstützt. Die weiter unten bereitgestellte Liste mit unterstützten Diensten gibt Aufschluss darüber, welche Ressourcentypen die neuen Diagnoseprotokolle unterstützen.

![Logische Position von Diagnoseprotokollen](./media/monitoring-overview-of-diagnostic-logs/logical-placement-chart.png)

## Verwendungsmöglichkeiten für Diagnoseprotokolle
Im Anschluss sind einige Verwendungsmöglichkeiten für Diagnoseprotokolle aufgeführt:

- Diagnoseprotokolle können zur Überwachung oder manuellen Überprüfung in einem **Speicherkonto** gespeichert werden. Mithilfe der **Diagnoseeinstellungen** können Sie eine Aufbewahrungsdauer (in Tagen) angeben.
- Sie können Diagnoseprotokolle zur Erfassung durch einen Drittanbieterdienst oder durch eine benutzerdefinierte Analyselösung wie PowerBI [an **Event Hubs** streamen](monitoring-stream-diagnostic-logs-to-event-hubs.md).
- Diagnoseprotokolle können mit [OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md) analysiert werden.

## Diagnoseeinstellungen
Diagnoseprotokolle für computefremde Ressourcen werden mithilfe von Diagnoseeinstellungen konfiguriert. In den **Diagnoseeinstellungen** für ein Ressourcensteuerelement können Sie Folgendes festlegen:

- Wohin Diagnoseprotokolle gesendet werden sollen (Speicherkonto, Event Hubs und/oder OMS Log Analytics)
- Welche Protokollkategorien gesendet werden sollen
- Wie lange die einzelnen Protokollkategorien in einem Speicherkonto aufbewahrt werden sollen. Bei Angabe einer Aufbewahrungsdauer von null Tagen werden die Protokolle dauerhaft aufbewahrt. Wenn Aufbewahrungsrichtlinien festgelegt werden, aber das Speichern von Protokollen in einem Speicherkonto deaktiviert ist (etwa, wenn nur die Event Hubs- oder die OMS-Option aktiviert ist), werden die Aufbewahrungsrichtlinien ignoriert.

Diese Einstellungen können für eine Ressource ganz einfach über das Diagnoseblatt im Azure-Portal, über Azure PowerShell und mithilfe von CLI-Befehlen oder über die [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx) konfiguriert werden.

> [AZURE.WARNING] Bei Diagnoseprotokollen und Metriken für Computeressourcen (etwa virtuelle Computer oder Service Fabric) wird [ein separater Mechanismus für die Konfiguration und Auswahl von Ausgaben](../azure-diagnostics.md) verwendet.

## Gewusst wie: Aktivieren der Erfassung von Diagnoseprotokollen
Die Erfassung von Diagnoseprotokollen kann im Zuge der Ressourcenerstellung und später im Portal über das Blatt der Ressource aktiviert werden. Darüber hinaus können Diagnoseprotokolle jederzeit mithilfe von Azure PowerShell oder CLI-Befehlen oder mithilfe der Insights-REST-API aktiviert werden.

> [AZURE.TIP] Diese Anweisungen lassen sich unter Umständen nicht immer direkt auf jede Ressource anwenden. Informationen zu Sonderschritten, die ggf. für bestimmte Ressourcentypen erforderlich sind, finden Sie unter den Schemalinks am Ende dieser Seite.

[In diesem Artikel erfahren Sie, wie Sie beim Erstellen einer Ressource mithilfe einer Ressourcenvorlage Diagnoseeinstellungen aktivieren.](./monitoring-enable-diagnostic-logs-using-template.md)

### Aktivieren von Diagnoseprotokollen über das Portal
Diagnoseprotokolle können im Azure-Portal wie folgt beim Erstellen einiger Ressourcentypen aktiviert werden:

1.	Navigieren Sie zu **Neu**, und wählen Sie die gewünschte Ressource.
2.	Wählen Sie nach dem Konfigurieren der grundlegenden Einstellungen und dem Auswählen einer Größe auf dem Blatt **Einstellungen** unter **Überwachung** die Option **Aktiviert** und anschließend ein Speicherkonto aus, in dem die Diagnoseprotokolle gespeichert werden sollen. Wenn Sie Diagnoseinformationen an ein Speicherkonto senden, werden Ihnen die üblichen Datengebühren für Speicherung und Transaktionen in Rechnung gestellt. ![Aktivieren von Diagnoseprotokollen im Rahmen der Ressourcenerstellung](./media/monitoring-overview-of-diagnostic-logs/enable-portal-new.png)
3.	Klicken Sie auf **OK**, und erstellen Sie die Ressource.

Wenn Sie Diagnoseprotokolle nach der Erstellung einer Ressource über das Azure-Portal aktivieren möchten, führen Sie die folgenden Schritte aus:

1.	Wechseln Sie zum Blatt für die Ressource, und öffnen Sie das Blatt **Diagnose**.
2.	Klicken Sie auf **Ein**, und wählen Sie ein Speicherkonto und/oder eine Event Hub-Instanz aus. ![Aktivieren von Diagnoseprotokollen nach der Ressourcenerstellung](./media/monitoring-overview-of-diagnostic-logs/enable-portal-existing.png)
3.	Wählen Sie unter **Protokolle** die zu erfassenden oder zu streamenden **Protokollkategorien** aus.
4.	Klicken Sie auf **Speichern**.

### Programmgesteuertes Aktivieren von Diagnoseprotokollen
Mit den folgenden Befehlen können Sie Diagnoseprotokolle über Azure PowerShell-Cmdlets aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -StorageAccountId [your storage account id] -Enabled $true

Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

    Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.

Mit den folgenden Befehlen können Sie Diagnoseprotokolle über die Azure-Befehlszeilenschnittstelle aktivieren:

Verwenden Sie den folgenden Befehl, um das Speichern von Diagnoseprotokollen in einem Speicherkonto zu aktivieren:

    azure insights diagnostic set --resourceId <resourceId> --storageId <storageAccountId> --enabled true

Die Speicherkonto-ID (StorageAccountId) ist die Ressourcen-ID für das Speicherkonto, an das die Protokolle gesendet werden sollen.

Verwenden Sie den folgenden Befehl, um das Streamen von Diagnoseprotokollen an eine Event Hub-Instanz zu aktivieren:

    azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`.

Informationen zum Ändern der Diagnoseeinstellungen mithilfe der Insights-REST-API finden Sie in [diesem Dokument](https://msdn.microsoft.com/library/azure/dn931931.aspx).

## Unterstützte Dienste und Schema für Diagnoseprotokolle
Das Schema für Diagnoseprotokolle variiert abhängig von der Ressource und der Protokollkategorie. Im Anschluss finden Sie die unterstützten Dienste und das zugehörige Schema:

| Dienst | Schema und Dokumente |
|-------------------------------|-----------------------------------------------------------------------------------------------------------------|
| Software Load Balancer | [Protokollanalysen für den Azure-Lastenausgleich (Vorschau)](../load-balancer/load-balancer-monitor-log.md) |
| Netzwerksicherheitsgruppen | [Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) |
| Anwendungsgateways | [Diagnoseprotokollierung für Application Gateway](../application-gateway/application-gateway-diagnostics.md) |
| Schlüsseltresor | [Azure-Schlüsseltresor-Protokollierung](../key-vault/key-vault-logging.md) |
| Azure Search | [Aktivieren und Verwenden von „Datenverkehrsanalyse durchsuchen“](../search/search-traffic-analytics.md) |
| Data Lake-Speicher | [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Store](../data-lake-store/data-lake-store-diagnostic-logs.md) |
| Data Lake Analytics | [Zugreifen auf Diagnoseprotokolle für Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-diagnostic-logs.md) |
| Logik-Apps | Kein Schema verfügbar. |

## Nächste Schritte
- [Streamen von Diagnoseprotokollen an **Event Hubs**](monitoring-stream-diagnostic-logs-to-event-hubs.md)
- [Ändern der Diagnoseeinstellungen mithilfe der Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931931.aspx)
- [Analysieren der Protokolle mit OMS Log Analytics](../log-analytics/log-analytics-azure-storage-json.md)

<!---HONumber=AcomDC_0824_2016-->