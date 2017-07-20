---
title: Azure Service Bus-Diagnoseprotokolle | Microsoft Docs
description: "Hier erfahren Sie, wie Sie Diagnoseprotokolle für Service Bus in Azure einrichten."
keywords: 
documentationcenter: .net
services: service-bus-messaging
author: banisadr
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: babanisa;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: 775900fcae1b2832a5d0951e2a4053562c21455e
ms.contentlocale: de-de
ms.lasthandoff: 06/28/2017


---
# <a name="service-bus-diagnostic-logs"></a>Service Bus-Diagnoseprotokolle

Sie können zwei Typen von Protokollen für Azure Service Bus anzeigen:
* **[Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Diese Protokolle enthalten Informationen zu Vorgängen, die für einen Auftrag ausgeführt werden. Diese Protokolle sind immer aktiviert.
* **[Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Sie können Diagnoseprotokolle konfigurieren, um umfangreichere Informationen zu sämtlichen Vorgängen innerhalb des Auftrags zu erhalten. Diagnoseprotokolle enthalten Informationen zu Aktivitäten vom Erstellen bis zum Löschen des Auftrags, einschließlich Updates und Aktivitäten während der Auftragsausführung.

## <a name="turn-on-diagnostic-logs"></a>Aktivieren der Diagnoseprotokolle

Diagnoseprotokolle sind standardmäßig deaktiviert. Führen Sie die folgenden Schritte aus, um die Diagnoseprotokolle zu aktivieren:

1.  Wechseln Sie im [Azure-Portal](https://portal.azure.com) zum Blatt für den Streamingauftrag.

2.  Klicken Sie unter **Überwachung** auf das Blatt **Diagnoseprotokolle**.

    ![Blatt „Navigation zu Diagnoseprotokollen“](./media/service-bus-diagnostic-logs/image1.png)  

3.  Klicken Sie auf **Diagnose aktivieren**.

    ![Aktivieren der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image2.png)

4.  Klicken Sie für **Status** auf **Ein**.

    ![Ändern des Status der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image3.png)

5.  Legen Sie das gewünschte Archivierungsziel fest, z.B. ein Speicherkonto, einen Event Hub oder Azure Log Analytics.

6.  Wählen Sie die Kategorien von Protokollen aus, die Sie erfassen möchten z.B. **Ausführung** oder **Erstellung**.

7.  Speichern Sie die neuen Diagnoseeinstellungen.

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseprotokolle** angezeigt.

Weitere Informationen zum Konfigurieren der Diagnose finden Sie in der [Übersicht über Azure-Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“

Alle Protokolle werden im JavaScript Object Notation (JSON)-Format gespeichert. Jeder Eintrag enthält Zeichenfolgenfelder im nachfolgend beschriebenen Format.

## <a name="operation-logs-example"></a>Beispiel für Vorgangsprotokolle

Protokolle in der Kategorie **OperationalLogs** erfassen, was während Service Bus-Vorgängen passiert. Diese Protokolle erfassen insbesondere den Vorgangstyp, darunter das Erstellen von Warteschlangen, verwendete Ressourcen und den Status des Vorgangs.

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
{
  "ActivityId": "6aa994ac-b56e-4292-8448-0767a5657cc7",
  "EventName": "Create Queue",
  "resourceId": "/SUBSCRIPTIONS/1A2109E3-9DA0-455B-B937-E35E36C1163C/RESOURCEGROUPS/DEFAULT-SERVICEBUS-CENTRALUS/PROVIDERS/MICROSOFT.SERVICEBUS/NAMESPACES/SHOEBOXEHNS-CY4001",
  "SubscriptionId": "1a2109e3-9da0-455b-b937-e35e36c1163c",
  "EventTimeString": "9/28/2016 8:40:06 PM +00:00",
  "EventProperties": "{\"SubscriptionId\":\"1a2109e3-9da0-455b-b937-e35e36c1163c\",\"Namespace\":\"shoeboxehns-cy4001\",\"Via\":\"https://shoeboxehns-cy4001.servicebus.windows.net/f8096791adb448579ee83d30e006a13e/?api-version=2016-07\",\"TrackingId\":\"5ee74c9e-72b5-4e98-97c4-08a62e56e221_G1\"}",
  "Status": "Succeeded",
  "Caller": "ServiceBus Client",
  "category": "OperationalLogs"
}
```

## <a name="next-steps"></a>Nächste Schritte

Klicken Sie auf die folgenden Links, um mehr über Service Bus zu erfahren:

* [Einführung in Service Bus](service-bus-messaging-overview.md)
* [Erste Schritte mit Service Bus](service-bus-dotnet-get-started-with-queues.md)

