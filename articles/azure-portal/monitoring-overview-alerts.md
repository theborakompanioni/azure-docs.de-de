---
title: Überblick über Warnungen in Microsoft Azure | Microsoft Docs
description: Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist.
author: rboucher
manager: ''
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics

ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2016
ms.author: robb

---
# Überblick über Warnungen in Microsoft Azure
In diesem Artikel wird beschrieben, was Warnungen sind, welche Vorteile sie haben und wie Sie mit der Nutzung beginnen.

## Was sind Warnungen?
Mithilfe von Warnungen können Sie Metriken, Ereignisse oder Protokolle zu Ihren Azure-Ressourcen überwachen und sich bei Eintreten bestimmter von Ihnen festgelegter Bedingungen benachrichtigen lassen.

Sie können Warnungen basierend auf folgenden Elementen erhalten:

* Metrikwerte: Eine Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert über- oder unterschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.
* Aktivitätsprotokollereignisse: Eine Warnung kann für jedes Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen aufgetreten ist.

## Warnungen bei unterschiedlichen Azure-Diensten
Warnungen sind für verschiedene Dienste verfügbar

* **Application Insights**: Ermöglicht Webtest- und Metrikwarnungen. Informationen dazu finden Sie unter [Einrichten von Warnungen in Application Insights](../application-insights/app-insights-alerts.md) und [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites](../application-insights/app-insights-monitor-web-app-availability.md).
* **OMS Log Analytics**: Ermöglicht das Weiterleiten von Diagnoseprotokollen an Log Analytics. Mit OMS können Sie Metrik-, Protokoll- und andere Warnungstypen festlegen. Weitere Informationen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md).
* **Azure Monitor**: Azure Monitor ermöglicht das Festlegen von Warnungen basierend auf Metrikwerten und Aktivitätsprotokollereignissen. Azure Monitor enthält die [Azure Insights-REST-API](https://msdn.microsoft.com/library/dn931943.aspx). Weitere Informationen finden Sie auch unter [Nutzung des Azure-Portals, PowerShell oder der Befehlszeilenschnittstelle zum Erstellen von Warnungen](insights-alerts-portal.md).

## Warnungsaktionen
Sie können eine Warnung so konfigurieren, dass beim Auslösen folgende Aktionen ausgeführt werden:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren und/oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
* Senden von Webhooks, um zusätzliche automatisierte Aktionen auszuführen
  
  ![Warnungen im Überblick](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)

## Nächste Schritte
Weitere Informationen über Warnregeln und ihre Konfiguration erhalten Sie hier:

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle](insights-alerts-command-line-interface.md)
* [Azure Insights-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)

<!---HONumber=AcomDC_0928_2016-->