---
title: "Nutzen der Überwachungsdaten von Azure | Microsoft Docs"
description: "Erfahren Sie etwas über alle zurzeit in Azure verfügbaren Überwachungsdatenquellen."
author: johnkemnetz
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/17/2017
ms.author: johnkem
translationtype: Human Translation
ms.sourcegitcommit: bb1ca3189e6c39b46eaa5151bf0c74dbf4a35228
ms.openlocfilehash: 2a8bb4a5d66eef23afd776b40df487198f452b1e
ms.lasthandoff: 03/18/2017


---
# <a name="consume-monitoring-data-from-azure"></a>Nutzen der Überwachungsdaten von Azure

Wir bringen mit der Azure Monitor-Pipeline die Überwachungsdaten von der gesamten Azure-Plattform an einem Ort zusammen. Wir wissen aber auch, dass bisher noch nicht alle Überwachungsdaten in dieser Pipeline verfügbar sind. In diesem Artikel werden die verschiedenen Möglichkeiten für den programmgesteuerten Zugriff auf Überwachungsdaten von Azure-Diensten zusammengefasst.

## <a name="options-for-data-consumption"></a>Optionen für die Datennutzung

| Datentyp | Kategorie | Unterstützte Dienste | Zugriffsmethoden |
| --- | --- | --- | --- |
| Azure Monitor-Metriken auf Plattformebene | Metriken | [Siehe Liste hier](monitoring-supported-metrics.md) | <ul><li>**REST-API:** [Azure Monitor-Metrik-API](https://docs.microsoft.com/rest/api/monitor/metrics)</li><li>**Speicher-Blob oder Event Hub:** [Diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| Compute-Metriken für Gastbetriebssysteme (z.B. Leistungsindikatoren) | Metriken | [Windows](../virtual-machines-dotnet-diagnostics.md)- und Linux-VMs (V2), [Clouddienste](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Speichertabelle oder Blob:** [Windows oder Linux – Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event Hub:** [Windows Azure-Diagnose](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| Benutzerdefinierte oder Anwendungsmetriken | Metriken | Alle mit Application Insights instrumentierten Anwendungen | <ul><li>**REST-API:** [Application Insights-REST-API](https://dev.applicationinsights.io/reference)</li></ul> |
| Speichermetrik | Metriken | Azure Storage | <ul><li>**Speichertabelle:** [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)</li></ul> |
| Aktivitätsprotokoll | Ereignisse | Alle Azure-Dienste | <ul><li>**REST-API:** [Azure Monitor-Ereignis-API](https://docs.microsoft.com/rest/api/monitor/events)</li><li>**Speicherblob oder Event Hub:** [Protokollprofil](monitoring-overview-activity-logs.md#export-the-activity-log-with-log-profiles)</li></ul> |
| Azure Monitor-Diagnoseprotokolle | Ereignisse | [Siehe Liste hier](monitoring-overview-of-diagnostic-logs.md#supported-services-and-schema-for-diagnostic-logs) | <ul><li>**Speicher-Blob oder Event Hub:** [Diagnoseeinstellungen](monitoring-overview-of-diagnostic-logs.md#diagnostic-settings)</li></ul> |
| Compute-Protokolle für Gastbetriebssysteme (z.B. IIS, ETW, syslogs) | Ereignisse | [Windows](../virtual-machines-dotnet-diagnostics.md)- und Linux-VMs (V2), [Clouddienste](../cloud-services/cloud-services-dotnet-diagnostics-trace-flow.md), [Service Fabric](../service-fabric/service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md) | <ul><li>**Speichertabelle oder Blob:** [Windows oder Linux – Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics-storage.md)</li><li>**Event Hub:** [Windows Azure-Diagnose](../event-hubs/event-hubs-streaming-azure-diags-data.md)</li></ul> |
| App Service-Protokolle | Ereignisse | App Services | <ul><li>**File, Table oder Blob Storage:** [Web-App-Diagnose](../app-service-web/web-sites-enable-diagnostic-log.md)</li></ul> |
| Speicherprotokolle | Ereignisse | Azure Storage | <ul><li>**Speichertabelle:** [Speicheranalyse](https://docs.microsoft.com/rest/api/storageservices/fileservices/storage-analytics)</li></ul> |
| Security Center-Warnungen | Ereignisse | Azure Security Center | <ul><li>**REST-API:** [Sicherheitswarnungen](https://msdn.microsoft.com/library/mt704050.aspx)</li></ul> |
| Active Directory-Berichte | Ereignisse | Azure Active Directory | <ul><li>**REST-API:** [Azure Active Directory-Graph-API](../active-directory/active-directory-reporting-api-getting-started.md)</li></ul> |
| Security Center-Ressourcenstatus | Status | [Alle unterstützten Ressourcen](https://msdn.microsoft.com/library/mt704041.aspx#Anchor_1) | <ul><li>**REST-API:** [Sicherheitsstatus](https://msdn.microsoft.com/library/mt704041.aspx)</li></ul> |
| Resource Health | Status | Unterstützte Dienste | <ul><li>**REST-API:** [Ressourcenintegritäts-REST-API](https://azure.microsoft.com/blog/reduce-troubleshooting-time-with-azure-resource-health/)</li></ul> |
| Azure Monitor-Metrikwarnungen | Benachrichtigungen | [Siehe Liste hier](monitoring-supported-metrics.md) | <ul><li>**Webhook:** [Azure-Metrikwarnungen](insights-webhooks-alerts.md)</li></ul> |
| Azure Monitor-Aktivitätsprotokollwarnungen | Benachrichtigungen | Alle Azure-Dienste | <ul><li>**Webhook:** Azure-Aktivitätsprotokollwarnungen</li></ul> |
| Benachrichtigungen zum automatischen Skalieren | Benachrichtigungen | [Siehe Liste hier](monitoring-overview-autoscale.md#supported-services-for-autoscale) | <ul><li>**Webhook:** [Benachrichtigung zur automatischen Skalierung mit dem Webhook-Nutzlastschema](insights-autoscale-to-webhook-email.md#autoscale-notification-webhook-payload-schema)</li></ul> |
| Warnungen zu OMS-Protokollsuchabfragen | Benachrichtigungen | OMS Log Analytics | <ul><li>**Webhook:** [Log Analytics-Warnungen](../log-analytics/log-analytics-alerts-actions.md#webhook-actions)</li></ul> |
| Application Insights-Metrikwarnungen | Benachrichtigungen | Application Insights | <ul><li>**Webhook:** [Application Insights-Warnungen](../application-insights/app-insights-alerts.md)</li></ul> |
| Application Insights-Webtests | Benachrichtigungen | Application Insights | <ul><li>**Webhook:** [Application Insights-Warnungen](../application-insights/app-insights-alerts.md)</li></ul> |

## <a name="next-steps"></a>Nächste Schritte

- Erfahren Sie mehr zu [Azure Monitor-Metriken](monitoring-overview-metrics.md).
- Erfahren Sie mehr zum [Azure-Aktivitätsprotokoll](monitoring-overview-activity-logs.md).
- Erfahren Sie mehr zu [Azure-Diagnoseprotokollen](monitoring-overview-of-diagnostic-logs.md).

