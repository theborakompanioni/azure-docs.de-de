---
title: "Überblick über Warnungen in Microsoft Azure und Microsoft Azure Monitor| Microsoft-Dokumentation"
description: "Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: robb
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 76c8feb077cca27dc96f43e708cdef4fbb0f824c
ms.contentlocale: de-de
ms.lasthandoff: 03/31/2017

---
# <a name="what-are-alerts-in-microsoft-azure"></a>Was sind Warnungen in Microsoft Azure?
In diesem Artikel wird beschrieben, was Warnungen sind, welche Vorteile sie haben und wie Sie mit der Nutzung beginnen. Er bezieht sich insbesondere auf Azure Monitor, enthält aber Verweise auf andere Dienste.  

Mithilfe von Warnungen können Sie Metriken, Ereignisse oder Protokolle zu Ihren Azure-Ressourcen überwachen und sich bei Eintreten bestimmter von Ihnen festgelegter Bedingungen benachrichtigen lassen.  

## <a name="alerts-in-different-azure-services"></a>Warnungen bei unterschiedlichen Azure-Diensten
Warnungen sind für verschiedene Dienste verfügbar, z.B. die folgenden:

* **Application Insights**: Ermöglicht Webtest- und Metrikwarnungen. Informationen dazu finden Sie unter [Einrichten von Warnungen in Application Insights](../application-insights/app-insights-alerts.md) und [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites](../application-insights/app-insights-monitor-web-app-availability.md).
* **Log Analytics (Operations Management Suite)**: Ermöglicht die Weiterleitung von Aktivitäts- und Diagnoseprotokollen an Log Analytics. Die Operations Management Suite ermöglicht Metrik-, Protokollwarnungen und andere Typen von Warnungen. Weitere Informationen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md).  
* **Azure Monitor**: Ermöglicht das Festlegen von Warnungen basierend auf Metrikwerten und Aktivitätsprotokollereignissen. Mit der [Azure Monitor-REST-API](https://msdn.microsoft.com/library/dn931943.aspx) können Sie Benachrichtigungen verwalten.  Weitere Informationen finden Sie unter [Nutzung des Azure-Portals, von PowerShell oder der Befehlszeilenschnittstelle zum Erstellen von Warnungen](insights-alerts-portal.md).

## <a name="visual-summary"></a>Visuelle Zusammenfassung
Das folgende Diagramm enthält eine Zusammenfassung der Warnungen und ihrer Funktionen insbesondere in Azure Monitor. Für die zuvor aufgeführten Dienste können andere Aktionen verfügbar sein. Beispielsweise sind Warnungen zu Diagnoseprotokollen derzeit nur in Log Analytics verfügbar.

![Warnungen im Überblick](./media/monitoring-overview-alerts/Alerts_Overview_Resource_v4.png)

## <a name="what-can-trigger-alerts-in-azure-monitor"></a>Auslöser von Warnungen in Azure Monitor

Sie können Warnungen basierend auf folgenden Elementen erhalten:

* **Metrikwerte**: Diese Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert über- oder unterschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird. Eine laufend ergänzte Liste verfügbarer Metriken, die vom Azure-Monitor unterstützt werden, finden Sie unter [Unterstützte Metriken von Azure Monitor](monitoring-supported-metrics.md).
* **Aktivitätsprotokollereignisse:** Diese Warnung kann ausgelöst werden, wenn ein bestimmtes Ereignis für eine Ressource eintritt oder wenn eine Dienstbenachrichtigung in Ihrem Abonnement veröffentlicht wird.

## <a name="what-can-metric-alerts-do"></a>Welche Aktionen können Metrikwarnungen ausführen?
Sie können eine Warnung so konfigurieren, dass folgende Aktionen ausgeführt werden:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
* Aufrufen eines Webhooks, um zusätzliche automatisierte Aktionen auszuführen Beispielsweise kann aufgerufen werden:
    - Azure Automation-Runbook
    - Azure Function
    - Azure-Logik-App
    - ein Drittanbieter-Dienst

## <a name="what-can-activity-log-alerts-do"></a>Welche Aktionen können Aktivitätsprotokollwarnungen ausführen?
Sie können eine Warnung so konfigurieren, dass folgende Aktionen ausgeführt werden:
* Auslösen, wenn ein bestimmtes Ereignis für eine Ressource in Ihrem Abonnement eintritt
* Auslösen, wenn eine Dienstbenachrichtigung in Ihrem Abonnement veröffentlicht wird
* Benachrichtigen von Mitgliedern einer Aktivitätsgruppe per
    * sms
    * E-Mail
    * Webhook

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Warnregeln und ihre Konfiguration erhalten Sie hier:

* Weitere Informationen zu [Metriken](monitoring-overview-metrics.md)
* Konfigurieren von [Metrikwarnungen über das Azure-Portal](insights-alerts-portal.md)
* Konfigurieren von [Metrikwarnungen mithilfe von PowerShell](insights-alerts-powershell.md)
* Konfigurieren von [Metrikwarnungen über die Befehlszeilenschnittstelle (CLI)](insights-alerts-command-line-interface.md)
* Konfigurieren von [Metrikwarnungen mithilfe der Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)
* Weitere Informationen zu [Aktivitätsprotokollen](monitoring-overview-activity-logs.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über das Azure-Portal](monitoring-activity-log-alerts.md)
* Konfigurieren von [Aktivitätsprotokollwarnungen über den Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Weitere Informationen zum [Webhookschema für Aktivitätsprotokollwarnungen](monitoring-activity-log-alerts-webhook.md)
* Weitere Informationen zu [Dienstbenachrichtigungen](monitoring-service-notifications.md)
* Weitere Informationen zu [Aktionsgruppen](monitoring-action-groups.md)

