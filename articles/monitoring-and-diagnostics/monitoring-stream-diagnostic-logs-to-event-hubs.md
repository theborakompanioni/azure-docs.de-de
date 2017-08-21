---
title: Streamen von Azure-Diagnoseprotokollen an Event Hubs | Microsoft Docs
description: Hier erfahren Sie, wie Sie Azure-Diagnoseprotokolle an Event Hubs streamen.
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 42bc4845-c564-4568-b72d-0614591ebd80
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: johnkem
ms.translationtype: HT
ms.sourcegitcommit: cddb80997d29267db6873373e0a8609d54dd1576
ms.openlocfilehash: d974d64dbafe15707a6ce86144b98bad334f7f00
ms.contentlocale: de-de
ms.lasthandoff: 07/18/2017

---
# <a name="stream-azure-diagnostic-logs-to-event-hubs"></a>Streamen von Azure-Diagnoseprotokollen an Event Hubs
**[Azure-Diagnoseprotokolle](monitoring-overview-of-diagnostic-logs.md)** können nahezu in Echtzeit an eine beliebige Anwendung gestreamt werden – entweder mithilfe der integrierten Portal-Option „In Event Hubs exportieren“ oder durch Aktivieren der Service Bus-Regel-ID in einer Diagnoseeinstellung (über die Azure PowerShell-Cmdlets oder über die Azure-Befehlszeilenschnittstelle).

## <a name="what-you-can-do-with-diagnostics-logs-and-event-hubs"></a>Verwendungsmöglichkeiten für Diagnoseprotokolle und Event Hubs
Im Anschluss finden Sie eine kleine Auswahl von Verwendungsmöglichkeiten für das Streamen von Diagnoseprotokollen:

* **Streamen von Protokollen an Protokollierungs- und Telemetriesysteme von Drittanbietern:** Event Hubs-Streaming entwickelt sich mehr und mehr zum Mechanismus für die Weiterreichung von Diagnoseprotokollen an SIEMs und Protokollanalyselösungen von Drittanbietern.
* **Anzeigen der Dienstintegrität durch Streamen von Daten zum langsamsten Pfad an PowerBI:** Mithilfe von Event Hubs, Stream Analytics und PowerBI können Sie sich anhand Ihrer Diagnosedaten problemlos und nahezu in Echtzeit einen Einblick in Ihre Azure-Dienste verschaffen. [Dieser Dokumentationsartikel](../stream-analytics/stream-analytics-power-bi-dashboard.md)bietet eine gute Übersicht über die Einrichtung von Event Hubs, die Verarbeitung von Daten mit Stream Analytics und die Verwendung von PowerBI als Ausgabe. Hier finden Sie einige Tipps für die Einrichtung von Diagnoseprotokollen:
  
  * Ein Event Hub wird automatisch für eine Kategorie von Diagnoseprotokollen erstellt, wenn Sie die entsprechende Option im Portal oder über PowerShell aktivieren. Es empfiehlt sich daher, den Event Hub im Namespace mit dem Namen auszuwählen, der mit **insights-** beginnt.
  * Der folgende SQL-Code ist ein Beispiel für eine Stream Analytics-Abfrage, mit der Sie alle Protokolldaten in einer PowerBI-Tabelle analysieren können:

    ```sql
    SELECT
    records.ArrayValue.[Properties you want to track]
    INTO
    [OutputSourceName – the PowerBI source]
    FROM
    [InputSourceName] AS e
    CROSS APPLY GetArrayElements(e.records) AS records
    ```

* **Erstellen einer benutzerdefinierten Telemetrie- und Protokollierungsplattform:** Event Hubs ermöglicht dank des hochgradig skalierbaren Veröffentlichen/Abonnieren-Konzepts eine flexible Erfassung von Diagnoseprotokollen. Dies ist interessant, wenn Sie bereits über eine benutzerdefinierte Telemetrieplattform verfügen oder eine solche Plattform erstellen möchten. [Informationen zur Verwendung von Event Hubs für eine globale Telemetrieplattform finden Sie in der Anleitung von Dan Rosanova.](https://azure.microsoft.com/documentation/videos/build-2015-designing-and-sizing-a-global-scale-telemetry-platform-on-azure-event-Hubs/)

## <a name="enable-streaming-of-diagnostic-logs"></a>Aktivieren des Streamens von Diagnoseprotokollen
Das Streamen von Diagnoseprotokollen kann programmgesteuert, über das Portal oder mithilfe der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931943.aspx)aktiviert werden. Dabei wählen Sie immer einen Event Hubs-Namespace aus, in dem für jede aktualisierte Protokollkategorie eine Event Hub-Instanz erstellt wird. Eine **Diagnoseprotokollkategorie** ist ein Protokolltyp, der von einer Ressource erfasst werden kann. Die Protokollkategorien, die für eine bestimmte Ressource erfasst werden sollen, können Sie im Azure-Portal auf dem Blatt „Diagnose“ auswählen.

![Protokollkategorien im Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/log-categories.png)

> [!WARNING]
> Zum Aktivieren und Streamen von Diagnoseprotokollen aus Computeressourcen (beispielsweise virtuelle Computer oder Service Fabric) müssen [andere Schritte](../event-hubs/event-hubs-streaming-azure-diags-data.md)ausgeführt werden.
> 
> 

Der Service Bus- oder Event Hubs-Namespace muss sich nicht unter demselben Abonnement befinden, das Protokolle ausgibt, sofern der Benutzer, der die Einstellung konfiguriert, über den entsprechenden RBAC-Zugriff auf beide Abonnements verfügt.

### <a name="via-powershell-cmdlets"></a>Verwenden von PowerShell-Cmdlets
Wenn Sie das Streaming über die [Azure PowerShell-Cmdlets](insights-powershell-samples.md) aktivieren möchten, können Sie das Cmdlet `Set-AzureRmDiagnosticSetting` mit folgenden Parametern verwenden:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId [your resource Id] -ServiceBusRuleId [your Service Bus rule id] -Enabled $true
```

Die Service Bus-Regel-ID (ServiceBusRuleId) ist eine Zeichenfolge im folgenden Format: `{Service Bus resource ID}/authorizationrules/{key name}`. Beispiel: `/subscriptions/{subscription ID}/resourceGroups/Default-ServiceBus-WestUS/providers/Microsoft.ServiceBus/namespaces/{Service Bus namespace}/authorizationrules/RootManageSharedAccessKey`.

### <a name="via-azure-cli"></a>Verwenden der Azure-Befehlszeilenschnittstelle
Wenn Sie das Streaming über die [Azure-Befehlszeilenschnittstelle](insights-cli-samples.md) aktivieren möchten, können Sie den Befehl `insights diagnostic set` wie folgt verwenden:

```azurecli
azure insights diagnostic set --resourceId <resourceId> --serviceBusRuleId <serviceBusRuleId> --enabled true
```

Die Service Bus-Regel-ID muss im gleichen Format angegeben werden wie bei Verwendung des PowerShell-Cmdlets.

### <a name="via-azure-portal"></a>Verwenden des Azure-Portals
Wenn Sie das Streaming über das Azure-Portal aktivieren möchten, navigieren Sie zu den Diagnoseeinstellungen einer Ressource, und wählen Sie **In Event Hubs exportieren** aus.

![Exportieren in Event Hubs im Portal](./media/monitoring-stream-diagnostic-logs-to-event-hubs/portal-export.png)

Wählen Sie zum Konfigurieren einen vorhandenen Event Bus-Namespace aus. Der ausgewählte Namespace fungiert als Ziel für die Event Hub-Erstellung (falls Sie erstmals Diagnoseprotokolle streamen) oder für das Streaming (falls bereits Ressourcen vorhanden sind, die diese Protokollkategorie an diesen Namespace streamen), und die Richtlinie definiert die Berechtigungen für den Streamingmechanismus. Für das Streaming an einen Event Hub werden aktuell Berechtigungen für das Verwalten, Senden und Lauschen benötigt. Sie können freigegebene Zugriffsrichtlinien für Ihren Event Hubs-Namespace im Portal auf der Registerkarte **Konfigurieren** für Ihren Namespace erstellen oder ändern. Zum Aktualisieren einer dieser Diagnoseeinstellungen muss der Client in der Event Hubs-Autorisierungsregel über die **ListKey**-Berechtigung verfügen.

## <a name="how-do-i-consume-the-log-data-from-event-hubs"></a>Nutzen der Protokolldaten aus Event Hubs
Hier sehen Sie ein Beispiel für eine Datenausgabe aus Event Hubs:

```json
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
| --- | --- |
| records |Ein Array sämtlicher Protokollereignisse in dieser Nutzlast. |
| time |Der Zeitpunkt, zu dem das Ereignis aufgetreten ist. |
| category |Die Protokollkategorie für dieses Ereignis. |
| resourceId |Die Ressourcen-ID der Ressource, die dieses Ereignis generiert hat. |
| operationName |Der Name des Vorgangs. |
| level |Optional. Gibt die Protokollereignisebene an. |
| properties |Die Eigenschaften des Ereignisses. |

Eine Liste mit allen Ressourcenanbietern, die das Streamen an Event Hubs unterstützen, finden Sie [hier](monitoring-overview-of-diagnostic-logs.md).

## <a name="stream-data-from-compute-resources"></a>Streamen von Daten von Computeressourcen
Mithilfe des Windows Azure-Diagnose-Agents können Sie auch Diagnoseprotokolle von Computeressourcen streamen. Informationen zum Einrichten finden Sie in [diesem Artikel](../event-hubs/event-hubs-streaming-azure-diags-data.md).

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Azure-Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md)
* [Erste Schritte mit Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)


