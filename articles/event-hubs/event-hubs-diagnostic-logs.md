---
title: Azure Event Hubs-Diagnoseprotokolle | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie Diagnoseprotokolle für Event Hubs in Azure einrichten."
keywords: 
documentationcenter: 
services: event-hubs
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/01/2017
ms.author: babanisa
translationtype: Human Translation
ms.sourcegitcommit: abcb0eee979853948cf6d981ff8f3a457eeeeef0
ms.openlocfilehash: 87c0f3eab8c09c79de06c2e806830b2f67ea5732
ms.lasthandoff: 03/01/2017


---
# <a name="event-hubs-diagnostic-logs"></a>Event Hubs-Diagnoseprotokolle

Sie können zwei Typen von Protokollen für Azure Event Hubs anzeigen:
* **[Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Diese Protokolle enthalten Informationen zu Vorgängen, die für einen Auftrag ausgeführt werden. Diese Protokolle sind immer aktiviert.
* **[Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Sie können Diagnoseprotokolle konfigurieren, um umfangreichere Einblicke in alle Vorgänge zu erhalten, die im Rahmen des Auftrags ausgeführt werden. Diagnoseprotokolle enthalten Informationen zu Aktivitäten vom Erstellen bis zum Löschen des Auftrags, einschließlich Updates und Aktivitäten während der Auftragsausführung.

## <a name="turn-on-diagnostic-logs"></a>Aktivieren der Diagnoseprotokolle
Diagnoseprotokolle sind standardmäßig **deaktiviert**. So aktivieren Sie Diagnoseprotokolle:

1.    Rufen Sie im Azure-Portal das Blatt zum Streamingauftrag auf.

2.    Klicken Sie unter **Überwachung** auf das Blatt **Diagnoseprotokolle**.

    ![Navigation zu Diagnoseprotokollen](./media/event-hubs-diagnostic-logs/image1.png)  

3.    Wählen Sie **Diagnose aktivieren**.

    ![Aktivieren der Diagnoseprotokolle](./media/event-hubs-diagnostic-logs/image2.png)

4.    Wählen Sie als **Status** **Ein** aus.

    ![Ändern des Status der Diagnoseprotokolle](./media/event-hubs-diagnostic-logs/image3.png)

5.    Legen Sie das gewünschte Archivierungsziel fest, z. B. ein Speicherkonto, einen Event Hub oder eine Azure-Protokollanalyse.

6.    Wählen Sie die Kategorien von Protokollen, die Sie erfassen möchten z. B. **Ausführung** oder **Erstellung**.

7.    Speichern Sie die neuen Diagnoseeinstellungen.

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseprotokolle** angezeigt.

Weitere Informationen zum Konfigurieren der Diagnose finden Sie in der [Übersicht über Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-categories"></a>Diagnoseprotokollkategorien
Event Hubs erfassen Diagnoseprotokolle für zwei Kategorien:

* **ArchivalLogs**: Hiermit werden Protokolle für das Event Hub-Archiv (insbesondere im Zusammenhang mit Archivfehlern) erfasst.
* **OperationalLogs:** Hiermit werden Abläufe während Event Hubs-Vorgängen erfasst. Dazu zählen der Vorgangstyp (insbesondere Event Hub-Erstellung), verwendete Ressourcen und der Status des Vorgangs.

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“
Alle Protokolle werden im JavaScript Object Notation (JSON)-Format gespeichert. Jeder Eintrag enthält Zeichenfolgenfelder im nachfolgend beschriebenen Format.

### <a name="archive-logs-schema"></a>Archivprotokollschema

JSON-Zeichenfolgen im Archivprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

Name | Beschreibung
------- | -------
TaskName | Beschreibung der fehlgeschlagenen Aufgabe
ActivityId | Interne ID zur Nachverfolgung
trackingId | Interne ID zur Nachverfolgung
resourceId | Azure Resource Manager-Ressourcen-ID
eventHub | Vollständiger Event Hub-Name (mit Namespace-Name)
partitionId | Die Partition, auf die geschrieben wird
archiveStep | ArchiveFlushWriter
startTime | Fehlerstartzeit
failures | Häufigkeit, mit der ein Fehler aufgetreten ist
durationInSeconds | Dauer des Fehlers
Message: | Fehlermeldung
category | ArchiveLogs

Hier ein Beispiel für eine JSON-Zeichenfolge im Archivierungsprotokoll:

```json
{
     "TaskName": "EventHubArchiveUserError",
     "ActivityId": "21b89a0b-8095-471a-9db8-d151d74ecf26",
     "trackingId": "21b89a0b-8095-471a-9db8-d151d74ecf26_B7",
     "resourceId": "/SUBSCRIPTIONS/854D368F-1828-428F-8F3C-F2AFFA9B2F7D/RESOURCEGROUPS/DEFAULT-EVENTHUB-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/FBETTATI-OPERA-EVENTHUB",
     "eventHub": "fbettati-opera-eventhub:eventhub:eh123~32766",
     "partitionId": "1",
     "archiveStep": "ArchiveFlushWriter",
     "startTime": "9/22/2016 5:11:21 AM",
     "failures": 3,
     "durationInSeconds": 360,
     "message": "Microsoft.WindowsAzure.Storage.StorageException: The remote server returned an error: (404) Not Found. ---> System.Net.WebException: The remote server returned an error: (404) Not Found.\r\n   at Microsoft.WindowsAzure.Storage.Shared.Protocol.HttpResponseParsers.ProcessExpectedStatusCodeNoException[T](HttpStatusCode expectedStatusCode, HttpStatusCode actualStatusCode, T retVal, StorageCommandBase`1 cmd, Exception ex)\r\n   at Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob.<PutBlockImpl>b__3e(RESTCommand`1 cmd, HttpWebResponse resp, Exception ex, OperationContext ctx)\r\n   at Microsoft.WindowsAzure.Storage.Core.Executor.Executor.EndGetResponse[T](IAsyncResult getResponseResult)\r\n   --- End of inner exception stack trace ---\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.StorageAsyncResult`1.End()\r\n   at Microsoft.WindowsAzure.Storage.Core.Util.AsyncExtensions.<>c__DisplayClass4.<CreateCallbackVoid>b__3(IAsyncResult ar)\r\n--- End of stack trace from previous location where exception was thrown ---\r\n   at System.",
     "category": "ArchiveLogs"
}
```

### <a name="operation-logs-schema"></a>Vorgangsprotokollschema

JSON-Zeichenfolgen im Vorgangsprotokoll enthalten Elemente, die in der folgenden Tabelle aufgeführt sind:

Name | Beschreibung
------- | -------
ActivityId | Interne ID zur Nachverfolgung
EventName | Vorgangsname             
resourceId | Azure Resource Manager-Ressourcen-ID
SubscriptionId | Abonnement-ID
EventTimeString | Vorgangsdauer
EventProperties | Vorgangseigenschaften
Status | Vorgangsstatus
Aufrufer | Aufrufer des Vorgangs (Azure-Portal oder Verwaltungsclient)
category | OperationalLogs

Hier ein Beispiel für eine JSON-Zeichenfolge im Vorgangsprotokoll:

```json
Example:
{
     "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
     "EventName": "Create EventHub",
     "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.EVENTHUB/NAMESPACES/SHOEBOXEHNS-CY4001",
     "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
     "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
     "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
     "Status": "Succeeded",
     "Caller": "ServiceBus Client",
     "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Nächste Schritte
* [Einführung in Event Hubs](event-hubs-what-is-event-hubs.md)
* [Übersicht über die Event Hubs-API](event-hubs-api-overview.md)
* [Erste Schritte mit Event Hubs](event-hubs-csharp-ephcs-getstarted.md)

