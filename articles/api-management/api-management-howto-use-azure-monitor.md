---
title: "Überwachen von API Management mit Azure Monitor | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie den Azure API Management-Dienst mithilfe von Azure Monitor überwachen."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 0f64947755c79739bb6f15325929bd074cfd7210
ms.lasthandoff: 03/31/2017

---
# <a name="monitor-api-management-with-azure-monitor"></a>Überwachen von API Management mit Azure Monitor
Azure Monitor ist ein Azure-Dienst, mit dem Sie all Ihre Azure-Ressourcen von einer einzigen Quelle aus verwalten können. Mit Azure Monitor können Sie Metriken und Protokolle aus Azure-Ressourcen wie API Management visualisieren, abfragen, weiterleiten und archivieren und ggf. notwendige Maßnahmen ergreifen. 

Im folgenden Video wird die Überwachung von API Management mithilfe von Azure Monitor veranschaulicht. Weitere Informationen zu Azure Monitor finden Sie unter [Erste Schritte mit Azure Monitor]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Metriken
API Management gibt zurzeit fünf Metriken aus, und wir planen, in Zukunft weitere hinzuzufügen. Diese Metriken werden jede Minute ausgegeben, sodass Sie einen Überblick über den Zustand und die Integrität Ihrer APIs nahezu in Echtzeit erhalten. Im Folgenden wird eine Zusammenfassung der Metriken aufgeführt:
* Gatewayanforderungen gesamt: Anzahl der API-Anforderungen innerhalb des Zeitraums. 
* Erfolgreiche Gatewayanforderungen: Anzahl der API-Anforderungen, die erfolgreiche HTTP-Antwortcodes einschließlich 304, 307 und alle unter 301 (z.B. 200) erhalten haben. 
* Fehlerhafte Gatewayanforderungen: Anzahl der API-Anforderungen, die fehlerhafte HTTP-Antwortcodes einschließlich 400 und alle über 500 erhalten haben.
* Nicht autorisierte Gatewayanforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes einschließlich 401, 403 und 429 erhalten haben. 
* Andere Gateway-Anforderungen: Anzahl der API-Anforderungen, die HTTP-Antwortcodes erhalten haben, die keiner der oben genannten Kategorien angehören (z.B. 418).

Sie können in Ihrem API Management-Dienst auf Metriken oder in Azure Monitor auf Metriken all Ihrer Azure-Ressourcen zugreifen. So zeigen Sie Metriken in Ihrem API Management-Dienst an
1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem API Management-Dienst.
3. Klicken Sie auf **Metriken**.

![Blatt „Metriken“][metrics-blade]

Weitere Informationen zur Verwendung von Metriken finden Sie unter [Übersicht über Metriken].

## <a name="activity-logs"></a>Aktivitätsprotokolle
Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre API Management-Dienste ausgeführt wurden. Sie wurden zuvor als „Überwachungsprotokolle“ oder „Betriebsprotokolle“ bezeichnet. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für Ihre API Management-Dienste durchgeführt wurden. 

> [!NOTE]
> Aktivitätsprotokolle enthalten keine Lesevorgänge (GET) oder Vorgänge, die im klassischen Herausgeberportal oder mithilfe der ursprünglichen Management-APIs durchgeführt wurden.

Sie können in Ihrem API Management-Dienst auf Aktivitätsprotokolle oder in Azure Monitor auf Protokolle all Ihrer Azure-Ressourcen zugreifen. So zeigen Sie Aktivitätsprotokolle in Ihrem API Management-Dienst an
1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem API Management-Dienst.
3. Klicken Sie auf **Aktivitätsprotokoll**.

![Blatt „Aktivitätsprotokolle“][activity-logs-blade]

Weitere Informationen zur Verwendung von Metriken finden Sie unter [Übersicht über Aktivitätsprotokolle].

## <a name="alerts"></a>Warnungen
Sie können die Konfiguration so durchführen, dass Warnungen basierend auf Metriken und Aktivitätsprotokollen empfangen werden. Mit Azure Monitor können Sie eine Warnung so konfigurieren, dass Folgendes erfolgt, wenn sie ausgelöst wird:

* Senden einer E-Mail-Benachrichtigung
* Aufrufen eines Webhooks
* Aufrufen einer Azure-Logik-App

Sie können Warnungsregeln in Ihrem API Management-Dienst oder in Azure Monitor konfigurieren. So konfigurieren Sie sie in API Management 
1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem API Management-Dienst.
3. Klicken Sie auf **Warnungsregeln**.

![Blatt „Warnungsregeln“][alert-rules-blade]

Weitere Informationen zum Verwenden von Warnungen finden Sie unter [Übersicht über Warnungen].

## <a name="diagnostic-logs"></a>Diagnoseprotokolle
Diagnoseprotokolle bieten umfassende Informationen zu Vorgängen und Fehlern, die zur Überwachung und Problembehandlung relevant sind. Diagnoseprotokolle unterscheiden sich von Aktivitätsprotokollen. Aktivitätsprotokolle geben Einblick in die Vorgänge, die für Ihre Azure-Ressourcen ausgeführt wurden. Diagnoseprotokolle bieten Einblick in Vorgänge, die Ihre Ressource selbst ausgeführt hat.

API Management bietet derzeit Diagnoseprotokolle (stündlich erfasst) zu einzelnen API-Anforderungen, bei denen jeder Eintrag die folgende Struktur aufweist:

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

Sie können in Ihrem API Management-Dienst auf Diagnoseprotokolle oder in Azure Monitor auf Protokolle all Ihrer Azure-Ressourcen zugreifen. So zeigen Sie Diagnoseprotokolle in Ihrem API Management-Dienst an
1. Öffnen Sie das Azure-Portal.
2. Wechseln Sie zu Ihrem API Management-Dienst.
3. Klicken Sie auf **Diagnoseprotokoll**.

![Blatt „Diagnoseprotokolle“][diagnostic-logs-blade]

Weitere Informationen zur Verwendung von Metriken finden Sie unter [Übersicht über Diagnoseprotokolle].

## <a name="next-step"></a>Nächster Schritt

* [Erste Schritte mit Azure Monitor]
* [Übersicht über Metriken]
* [Übersicht über Aktivitätsprotokolle]
* [Übersicht über Diagnoseprotokolle]
* [Übersicht über Warnungen]

[Erste Schritte mit Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md
[Übersicht über Metriken]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[Übersicht über Aktivitätsprotokolle]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[Übersicht über Diagnoseprotokolle]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[Übersicht über Warnungen]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png

