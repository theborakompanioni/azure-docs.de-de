---
title: Azure Service Bus-Diagnoseprotokolle | Microsoft Docs
description: "Hier erfahren Sie, wie Sie Diagnoseprotokolle für Service Bus in Azure einrichten."
keywords: 
documentationcenter: 
services: service-bus-messaging
author: banisadr
manager: 
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 02/17/2017
ms.author: babanisa
translationtype: Human Translation
ms.sourcegitcommit: abcb0eee979853948cf6d981ff8f3a457eeeeef0
ms.openlocfilehash: 65fe81dc90f2dc7a251860adfdd8374912cb8d73
ms.lasthandoff: 03/01/2017


---
# <a name="service-bus-diagnostic-logs"></a>Service Bus-Diagnoseprotokolle

Sie können zwei Typen von Protokollen für Azure Service Bus anzeigen:
* **[Aktivitätsprotokolle](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md)**. Diese Protokolle enthalten Informationen zu Vorgängen, die für einen Auftrag ausgeführt werden. Diese Protokolle sind immer aktiviert.
* **[Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)**. Sie können Diagnoseprotokolle konfigurieren, um umfangreichere Einblicke in alle Vorgänge zu erhalten, die im Rahmen des Auftrags ausgeführt werden. Diagnoseprotokolle enthalten Informationen zu Aktivitäten vom Erstellen bis zum Löschen des Auftrags, einschließlich Updates und Aktivitäten während der Auftragsausführung.

## <a name="turn-on-diagnostic-logs"></a>Aktivieren der Diagnoseprotokolle
Diagnoseprotokolle sind standardmäßig **deaktiviert**. So aktivieren Sie Diagnoseprotokolle:

1.    Rufen Sie im Azure-Portal das Blatt zum Streamingauftrag auf.

2.    Klicken Sie unter **Überwachung** auf das Blatt **Diagnoseprotokolle**.

    ![Blatt „Navigation zu Diagnoseprotokollen“](./media/service-bus-diagnostic-logs/image1.png)  

3.    Wählen Sie **Diagnose aktivieren**.

    ![Aktivieren der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image2.png)

4.    Wählen Sie als **Status** **Ein** aus.

    ![Ändern des Status der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image3.png)

5.    Legen Sie das gewünschte Archivierungsziel fest, z.B. ein Speicherkonto, einen Event Hub oder eine Azure-Protokollanalyse.

6.    Wählen Sie die Kategorien von Protokollen, die Sie erfassen möchten z. B. **Ausführung** oder **Erstellung**.

7.    Speichern Sie die neuen Diagnoseeinstellungen.

Neue Einstellungen werden in etwa zehn Minuten wirksam. Danach werden die Protokolle im gewünschten Archivierungsziel auf dem Blatt **Diagnoseprotokolle** angezeigt.

Weitere Informationen zum Konfigurieren der Diagnose finden Sie in der [Übersicht über Diagnoseprotokolle](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“

Alle Protokolle werden im JavaScript Object Notation (JSON)-Format gespeichert. Jeder Eintrag enthält Zeichenfolgenfelder im nachfolgend beschriebenen Format.

## <a name="operation-logs-example"></a>Beispiel für Vorgangsprotokolle

Protokolle in der Kategorie **OperationalLogs** erfassen, was während des Service Bus-Vorgangs passiert. Diese Protokolle erfassen insbesondere den Vorgangstyp, darunter das Erstellen von Warteschlangen, verwendete Ressourcen und den Status des Vorgangs.

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
* [Einführung in Service Bus](service-bus-messaging-overview.md)
* [Erste Schritte mit Service Bus](service-bus-create-namespace-portal.md)

