<properties
	pageTitle="Streamen von Azure-Diagnoseprotokollen an Event Hubs | Microsoft Azure"
	description="Hier erfahren Sie, wie Sie Azure-Diagnoseprotokolle an Event Hubs streamen."
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
	ms.date="08/08/2016"
	ms.author="johnkem"/>

# Streamen von Azure-Diagnoseprotokollen an Event Hubs

**[Azure-Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)** können nahezu in Echtzeit an eine beliebige Anwendung gestreamt werden – entweder mithilfe der integrierten Portal-Option „In Event Hubs exportieren“ oder durch Aktivieren der Service Bus-Regel-ID in einer Diagnoseeinstellung (über die Azure PowerShell-Cmdlets oder über die Azure-Befehlszeilenschnittstelle).

## Verwendungsmöglichkeiten für Diagnoseprotokolle und Event Hubs
Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen von Diagnoseprotokollen:

- **Streamen von Protokollen an Protokollierungs- und Telemetriesysteme von Drittanbietern:** Event Hubs-Streaming entwickelt sich mehr und mehr zum Mechanismus für die Weiterreichung von Diagnoseprotokollen an SIEMs und Protokollanalyselösungen von Drittanbietern.

- **Anzeigen der Dienstintegrität durch Streamen von Daten zum langsamsten Pfad an PowerBI:** Mithilfe von Event Hubs, Stream Analytics und PowerBI können Sie sich anhand Ihrer Diagnosedaten problemlos und nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. [Dieser Dokumentationsartikel](../stream-analytics/stream-analytics-power-bi-dashboard.md) bietet eine gute Übersicht über die Einrichtung von Event Hubs, die Verarbeitung von Daten mit Stream Analytics und die Verwendung von PowerBI als Ausgabe. Im Anschluss finden Sie einige Tipps für die Einrichtung von Diagnoseprotokollen:
	- Event Hubs wird automatisch für eine Kategorie von Diagnoseprotokollen erstellt, wenn Sie die entsprechende Option im Portal oder über PowerShell aktivieren. Es empfiehlt sich daher, Event Hubs im Service Bus-Namespace mit dem Namen auszuwählen, der mit „insights-“ beginnt.
	- Hier sehen Sie ein Beispiel für eine Stream Analytics-Abfrage, mit der Sie ganz einfach alle Protokolldaten in einer PowerBI-Tabelle analysieren können:

```
SELECT
records.ArrayValue.[Properties you want to track]
INTO
[OutputSourceName – the PowerBI source]
FROM
[InputSourceName] AS e
CROSS APPLY GetArrayElements(e.records) AS records
```

- **Erstellen einer benutzerdefinierter Telemetrie- und Protokollierungsplattform:** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung von Diagnoseprotokollen. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der [Anleitung von Dan Rosanova](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/).

## Aktivieren des Streamens von Diagnoseprotokollen
Das Streamen von Diagnoseprotokollen kann programmgesteuert, über das Portal oder mithilfe der [Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx) aktiviert werden. Dabei wählen Sie immer einen Service Bus-Namespace aus, in dem für jede aktualisierte Protokollkategorie eine Event Hubs-Instanz erstellt wird. Eine **Diagnoseprotokollkategorie** ist ein Protokolltyp, der von einer Ressource erfasst werden kann. Die Protokollkategorien, die für eine bestimmte Ressource erfasst werden sollen, können Sie im Azure-Portal auf dem Blatt „Diagnose“ auswählen.

![Protokollkategorien im Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [AZURE.WARNING] Zum Aktivieren und Streamen von Diagnoseprotokollen aus Computeressourcen (beispielsweise virtuelle Computer oder Service Fabric) müssen [andere Schritte](../event-hubs/event-hubs-streaming-azure-diags-data.md) ausgeführt werden.

### Verwenden von PowerShell-Cmdlets
Wenn Sie das Streaming über die [Azure PowerShell-Cmdlets](insights-powershell-samples.md) aktivieren möchten, können Sie das `Set-AzureRmDiagnosticSetting`-Cmdlet mit folgenden Parametern verwenden:

```
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your service bus rule id] -Enabled $true
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{service bus resource ID}/authorizationrules/{key name}`. Beispiel: `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{service bus namespace}/authorizationrules/RootManageSharedAccessKey`.


### Verwenden der Azure-Befehlszeilenschnittstelle
Wenn Sie das Streaming über die [Azure-Befehlszeilenschnittstelle](insights-cli-samples.md) aktivieren möchten, können Sie den `insights diagnostic set`-Befehl wie folgt verwenden:

```
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Die Service Bus-Regel-ID muss im gleichen Format angegeben werden wie bei Verwendung des PowerShell-Cmdlets.

###Verwenden des Azure-Portals
Wenn Sie das Streaming über das Azure-Portal aktivieren möchten, navigieren Sie zu den Diagnoseeinstellungen einer Ressource, und wählen Sie „In Event Hubs exportieren“ aus.

![Exportieren in Event Hubs im Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Wählen Sie zum Konfigurieren einen vorhandenen Service Bus-Namespace aus. Der ausgewählte Namespace fungiert als Ziel für die Event Hubs-Erstellung (falls Sie erstmals Diagnoseprotokolle streamen) oder für das Streaming (falls bereits Ressourcen vorhanden sind, die diese Protokollkategorie an diesen Namespace streamen), und die Richtlinie definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an Event Hubs werden aktuell Verwaltungs-, Lese- und Sendeberechtigungen benötigt. Sie können freigegebene Zugriffsrichtlinien für Ihren Service Bus-Namespace im klassischen Portal auf der Registerkarte „Konfigurieren“ erstellen oder ändern. Zum Aktualisieren einer dieser Diagnoseeinstellungen muss der Client in der Service Bus-Autorisierungsregel über die ListKey-Berechtigung verfügen.

##Nutzen der Protokolldaten aus Event Hubs
Hier sehen Sie ein Beispiel für eine Datenausgabe aus Event Hubs:

```
{
    "records": [
        {
            "time": "2016-07-15T18:00:22.6235064Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330013509921957/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Error",
            "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T17:58:55.048482Z",
                "endTime": "2016-07-15T18:00:22.4109204Z",
                "status": "Failed",
                "code": "BadGateway",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330013509921957",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "29a9862f-969b-4c70-90c4-dfbdc814e413",
                    "clientTrackingId": "08587330013509921958"
                }
            }
        },
        {
            "time": "2016-07-15T18:01:15.7532989Z",
            "workflowId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA",
            "resourceId": "/SUBSCRIPTIONS/DF602C9C-7AA0-407D-A6FB-EB20C8BD1192/RESOURCEGROUPS/JOHNKEMTEST/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/JOHNKEMTESTLA/RUNS/08587330012106702630/ACTIONS/SEND_EMAIL",
            "category": "WorkflowRuntime",
            "level": "Information",
            "operationName": "Microsoft.Logic/workflows/workflowActionStarted",
            "properties": {
                "$schema": "2016-04-01-preview",
                "startTime": "2016-07-15T18:01:15.5828115Z",
                "status": "Running",
                "resource": {
                    "subscriptionId": "df602c9c-7aa0-407d-a6fb-eb20c8bd1192",
                    "resourceGroupName": "JohnKemTest",
                    "workflowId": "243aac67fe904cf195d4a28297803785",
                    "workflowName": "JohnKemTestLA",
                    "runId": "08587330012106702630",
                    "location": "westus",
                    "actionName": "Send_email"
                },
                "correlation": {
                    "actionTrackingId": "042fb72c-7bd4-439e-89eb-3cf4409d429e",
                    "clientTrackingId": "08587330012106702632"
                }
            }
        }
    ]
}
```

| Elementname | Beschreibung |
|--------------|--------------------------------------------------------|
|records | Ein Array sämtlicher Protokollereignisse in dieser Nutzlast. |
|time | Der Zeitpunkt, zu dem das Ereignis aufgetreten ist. |
|category | Die Protokollkategorie für dieses Ereignis. |
|resourceId | Die Ressourcen-ID der Ressource, die dieses Ereignis generiert hat. |
|operationName | Der Name des Vorgangs. |
|level | Optional. Gibt die Protokollereignisebene an. |
|properties | Die Eigenschaften des Ereignisses. |


Eine Liste mit allen Ressourcenanbietern, die das Streamen an Event Hubs unterstützen, finden Sie [hier](monitoring-overview-of-diagnostic-logs.md).

##Nächste Schritte
- [Weitere Informationen zu Azure-Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md)
- [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)

<!---HONumber=AcomDC_0817_2016-->