---
title: Azure Event Hubs-Diagnoseprotokolle | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Diagnoseprotokolle von Event Hubs in Microsoft Azure analysieren.
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
ms.sourcegitcommit: 4d7d0e1f5ffb395bf91bbdac1ab4b9ec3f9dee1c
ms.openlocfilehash: cb8c7930ee423543c91366de64220ee55609a4cf


---
# <a name="event-hub-diagnostic-logs"></a>Event Hub-Diagnoseprotokolle

## <a name="introduction"></a>Einführung
Event Hubs bietet zwei Typen von Protokollen: 
* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), die immer aktiviert sind und Einblicke in Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden;
* [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), die vom Benutzer konfigurierbar sind und umfangreichere Einblicke in alle Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden, beginnend beim Erstellen und Aktualisieren über die Ausführung bis zum Löschen.

## <a name="how-to-enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. Gehen Sie folgendermaßen vor, um sie zu aktivieren:

Melden Sie sich beim Azure-Portal an, navigieren Sie zu dem Blatt „Streamingauftrag“, und verwenden Sie das Blatt „Diagnoseprotokolle“ unter „Überwachung“.

![Blatt „Navigation zu Diagnoseprotokollen“](./media/event-hubs-diagnostic-logs/image1.png)  

Klicken Sie dann auf den Link „Diagnose aktivieren“.

![Aktivieren der Diagnoseprotokolle](./media/event-hubs-diagnostic-logs/image2.png)

Ändern Sie in der geöffneten Diagnose den Status in „Nein“.

![Ändern des Status der Diagnoseprotokolle](./media/event-hubs-diagnostic-logs/image3.png)

Konfigurieren Sie das gewünschte Archivierungsziel (Speicherkonto, Ereignishub, Log Analytics), und wählen Sie die Kategorien der Protokolle, die Sie erfassen möchten (Ausführung, Erstellung). Speichern Sie dann die neue Diagnosekonfiguration.

Nach dem Speichern dauert es ungefähr zehn Minuten, bis die Konfiguration wirksam ist. Danach werden Protokolle im konfigurierten Archivierungsziel angezeigt, die Sie auf dem Blatt „Diagnoseprotokolle“ sehen können:

Weitere Informationen zum Konfigurieren der Diagnose finden Sie auf der Seite [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostic-logs-categories"></a>Diagnoseprotokollkategorien
Es gibt zwei Kategorien von Diagnoseprotokollen, die wir derzeit erfassen:

* **ArchivalLogs:** Hiermit werden Protokolle für das Event Hub-Archiv (insbesondere im Zusammenhang mit Archivfehlern) erfasst.
* **OperationalLogs:** Hiermit werden Abläufe während Event Hubs-Vorgängen erfasst. Dazu zählen der Vorgangstyp (etwa Event Hub-Erstellung), verwendete Ressourcen und der Status des Vorgangs.

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“
Alle Protokolle werden im JSON-Format gespeichert, und jeder Eintrag enthält Zeichenfolgenfelder im folgenden Format:


### <a name="archive-error-schema"></a>Archivfehlerschema
Name | Beschreibung
------- | -------
TaskName | Die Beschreibung des fehlgeschlagenen Tasks
ActivityId | Interne ID zur Nachverfolgung
trackingId | Interne ID zur Nachverfolgung
resourceId | ARM-Ressourcen-ID
eventHub | Vollständiger Event Hub-Name (mit Namespace-Name)
partitionId | Die Partition, auf die im Event Hub geschrieben wird
archiveStep | ArchiveFlushWriter
startTime | Fehlerstartzeit
failures | Häufigkeit, mit der ein Fehler aufgetreten ist
durationInSeconds | Dauer des Fehlers
Message: | Fehlermeldung
category | ArchiveLogs

#### <a name="example-archive-log"></a>Beispiel eines Archivprotokolls

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
Name | Beschreibung
------- | -------
ActivityId | Interne ID zur Nachverfolgung
EventName | Vorgangsname           
resourceId | ARM-Ressourcen-ID
SubscriptionId | Abonnement-ID
EventTimeString | Vorgangsdauer
EventProperties | Vorgangseigenschaften
Status | Vorgangsstatus
Aufrufer | Aufrufer des Vorgangs (Portal oder Verwaltungsclient)
category | OperationalLogs

#### <a name="example-operation-log"></a>Beispiel eines Vorgangsprotokolls

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


<!--HONumber=Feb17_HO1-->


