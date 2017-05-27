---
title: "Aktivieren der Diagnoseprotokollierung für Batch-Ereignisse – Azure | Microsoft-Dokumentation"
description: "Zeichnen Sie Diagnoseprotokollereignisse für Azure Batch-Kontoressourcen wie Pools und Tasks auf, und analysieren Sie sie."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: e14e611d-12cd-4671-91dc-bc506dc853e5
ms.service: batch
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: big-compute
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 6b6c548ca1001587e2b40bbe9ee2fcb298f40d72
ms.openlocfilehash: 16a13909079306256ded06f2100815c46ff562a3
ms.contentlocale: de-de
ms.lasthandoff: 02/28/2017


---
# <a name="log-events-for-diagnostic-evaluation-and-monitoring-of-batch-solutions"></a>Protokollereignisse für die Diagnoseauswertung und -überwachung von Batch-Lösungen

Wie bei vielen Azure-Diensten werden vom Batch-Dienst Protokollereignisse für bestimmte Ressourcen während der Lebensdauer der Ressource ausgegeben. Sie können Azure Batch-Diagnoseprotokolle aktivieren, um für Ressourcen wie Pools und Tasks Ereignisse aufzuzeichnen und anschließend zur Diagnose auszuwerten und zu überwachen. In Batch-Diagnoseprotokollen sind Ereignisse wie das Erstellen und Löschen von Pools und das Starten und Abschließen von Tasks enthalten.

> [!NOTE]
> In diesem Artikel wird die Protokollierung von Ereignissen für Batch-Kontoressourcen beschrieben, nicht jedoch für Auftrags- und Taskausgabedaten. Informationen zum Speichern der Ausgabedaten von Aufträgen und Tasks finden Sie unter [Beibehalten der Ausgabe von Azure Batch-Aufträgen und -Tasks](batch-task-output.md).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
* [Azure Batch-Konto](batch-account-create-portal.md)
* [Azure Storage-Konto](../storage/storage-create-storage-account.md#create-a-storage-account)
  
  Wenn Sie Batch-Diagnoseprotokolle beibehalten möchten, müssen Sie ein Azure Storage-Konto erstellen, unter dem Azure die Protokolle speichert. Geben Sie dieses Storage-Konto an, wenn Sie die [Diagnoseprotokollierung](#enable-diagnostic-logging) für Ihr Batch-Konto aktivieren. Das Storage-Konto, das Sie beim Aktivieren der Protokollsammlung angeben, ist nicht mit dem verknüpften Speicherkonto identisch, das in den Artikeln [Anwendungspakete](batch-application-packages.md) und [Beibehalten der Ausgabe](batch-task-output.md) beschrieben wird.
  
  > [!WARNING]
  > Für die Speicherung von Daten im Azure Storage-Konto wird eine **Gebühr erhoben**. Dies gilt auch für die in diesem Artikel beschriebenen Diagnoseprotokolle. Beachten Sie dies beim Entwerfen Ihrer [Beibehaltungsrichtlinie für Protokolle](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md).
  > 
  > 

## <a name="enable-diagnostic-logging"></a>Aktivieren der Diagnoseprotokollierung
Die Diagnoseprotokollierung ist für das Batch-Konto nicht standardmäßig aktiviert. Sie müssen die Diagnoseprotokollierung für jedes Batch-Konto, das Sie überwachen möchten, explizit aktivieren:

[Gewusst wie: Aktivieren der Erfassung von Diagnoseprotokollen](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

Es wird empfohlen, den Artikel [Übersicht über Azure-Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) ganz zu lesen. In diesem Artikel erfahren Sie nicht nur, wie die Protokollierung aktiviert wird, sondern auch, welche Protokollkategorien von den verschiedenen Azure-Diensten unterstützt werden. So unterstützt Azure Batch beispielsweise derzeit eine Protokollkategorie: **Dienstprotokolle**.

## <a name="service-logs"></a>Dienstprotokolle
Azure Batch-Dienstprotokolle enthalten Ereignisse, die während der Lebensdauer einer Batch-Ressource wie eines Pools oder einer Task ausgegeben werden. Jedes von Batch ausgegebene Ereignis wird im angegebenen Storage-Konto im JSON-Format gespeichert. Dies ist beispielsweise der Text eines Beispiel für ein **Poolerstellungsereignis**:

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicatedComputeNodes": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

Alle Ereignistexte werden im angegebenen Azure Storage-Konto in einer JSON-Datei gespeichert. Wenn Sie auf die Protokolle direkt zugreifen möchten, sollten Sie das [Schema der Diagnoseprotokolle im Speicherkonto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account) ansehen.

## <a name="service-log-events"></a>Dienstprotokollereignisse
Der Batch-Dienst gibt derzeit folgende Dienstprotokollereignisse aus. Diese Liste ist unter Umständen nicht vollständig, da seit der letzten Aktualisierung dieses Artikels möglicherweise weitere Ereignisse hinzugefügt wurden.

| **Dienstprotokollereignisse** |
| --- |
| [Poolerstellungsereignisse][pool_create] |
| [Ereignisse zum Starten des Löschvorgangs von Pools][pool_delete_start] |
| [Ereignisse zum Abschluss des Löschvorgangs von Pools][pool_delete_complete] |
| [Ereignisse zum Start der Größenänderung von Pools][pool_resize_start] |
| [Ereignisse zum Abschluss der Größenänderung von Pools][pool_resize_complete] |
| [Taskstartereignisse][task_start] |
| [Taskabschlussereignisse][task_complete] |
| [Taskfehlerereignisse][task_fail] |

## <a name="next-steps"></a>Nächste Schritte
In einem Azure Storage-Konto können Sie nicht nur Diagnoseprotokollereignisse speichern. Sie können auch Batch-Dienstprotokollereignisse an einen [Azure Event Hub](../event-hubs/event-hubs-what-is-event-hubs.md) streamen und an [Azure Log Analytics](../log-analytics/log-analytics-overview.md) senden.

* [Streamen Sie Azure-Diagnoseprotokolle an Event Hubs.](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)
  
  Streamen Sie Batch-Diagnoseereignisse an Event Hubs, äußerst skalierbare Dateneingangsdienste. Event Hubs können mit einem beliebigen Echtzeitanalyse-Anbieter Millionen Ereignisse pro Sekunde erfassen und anschließend transformieren und speichern.
* [Analysieren von Azure-Diagnoseprotokollen mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)
  
  Senden Sie Diagnoseprotokolle an Log Analytics. Hier können Sie sie im OMS-Portal (Operations Management Suite) analysieren oder zu Analyse in Power BI oder Excel exportieren.

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx

