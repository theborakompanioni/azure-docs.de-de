---
title: Azure Service Bus-Diagnoseprotokolle | Microsoft Docs
description: Erfahren Sie, wie Sie Diagnoseprotokolle von Service Bus in Microsoft Azure analysieren.
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
ms.sourcegitcommit: 90321171586110a3b60c3df5b749003ebbf70ec9
ms.openlocfilehash: 70205b33e9d52e41f5c1a637fee4da192e2a971a
ms.lasthandoff: 02/22/2017


---
# <a name="service-bus-diagnostic-logs"></a>Service Bus-Diagnoseprotokolle

## <a name="introduction"></a>Einführung
Service Bus bietet zwei Typen von Protokollen: 
* [Aktivitätsprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), die immer aktiviert sind und Einblicke in Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden;
* [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs), die vom Benutzer konfigurierbar sind und umfangreichere Einblicke in alle Vorgänge bieten, die im Rahmen des Auftrags ausgeführt werden, beginnend beim Erstellen und Aktualisieren über die Ausführung bis zum Löschen.

## <a name="how-to-enable-diagnostic-logs"></a>Aktivieren von Diagnoseprotokollen
Die Diagnoseprotokolle sind standardmäßig **deaktiviert**. Gehen Sie folgendermaßen vor, um sie zu aktivieren:

Melden Sie sich beim Azure-Portal an, navigieren Sie zu dem Blatt „Streamingauftrag“, und verwenden Sie das Blatt „Diagnoseprotokolle“ unter „Überwachung“.

![Blatt „Navigation zu Diagnoseprotokollen“](./media/service-bus-diagnostic-logs/image1.png)  

Klicken Sie dann auf den Link „Diagnose aktivieren“.

![Aktivieren der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image2.png)

Ändern Sie in der geöffneten Diagnose den Status in „Nein“.

![Ändern des Status der Diagnoseprotokolle](./media/service-bus-diagnostic-logs/image3.png)

Konfigurieren Sie das gewünschte Archivierungsziel (Speicherkonto, Ereignishub, Log Analytics), und wählen Sie die Kategorien der Protokolle, die Sie erfassen möchten (Ausführung, Erstellung). Speichern Sie dann die neue Diagnosekonfiguration.

Nach dem Speichern dauert es ungefähr zehn Minuten, bis die Konfiguration wirksam ist. Danach werden Protokolle im konfigurierten Archivierungsziel angezeigt, die Sie auf dem Blatt „Diagnoseprotokolle“ sehen können:

Weitere Informationen zum Konfigurieren der Diagnose finden Sie auf der Seite [Diagnoseprotokolle](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).

## <a name="diagnostic-logs-schema"></a>Schema „Diagnoseprotokolle“

Alle Protokolle werden im JSON-Format gespeichert, und jeder Eintrag enthält Zeichenfolgenfelder im folgenden Format.

### <a name="operation-logs"></a>Vorgangsprotokolle

Mit OperationalLogs werden Abläufe während Service Bus-Vorgängen erfasst. Dazu zählen der Vorgangstyp (etwa Warteschlangenerstellung), verwendete Ressourcen und der Status des Vorgangs.

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

