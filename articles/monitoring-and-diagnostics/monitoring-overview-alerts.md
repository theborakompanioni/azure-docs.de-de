---
title: "Überblick über Warnungen in Microsoft Azure | Microsoft Docs"
description: "Mit Warnungen können Sie Metriken, Ereignisse oder Protokolle für Ihre Azure-Ressourcen überwachen. Lassen Sie sich benachrichtigen, wenn eine von Ihnen festgelegte Bedingung erfüllt ist."
author: rboucher
manager: carolz
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: a6dea224-57bf-43d8-a292-06523037d70b
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/24/2016
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f80179a487d08244cbd5e57d34b10c4ab67b3cf5


---
# <a name="overview-of-alerts-in-microsoft-azure"></a>Überblick über Warnungen in Microsoft Azure
In diesem Artikel wird beschrieben, was Warnungen sind, welche Vorteile sie haben und wie Sie mit der Nutzung beginnen.  

## <a name="what-are-alerts"></a>Was sind Warnungen?
Mithilfe von Warnungen können Sie Metriken, Ereignisse oder Protokolle zu Ihren Azure-Ressourcen überwachen und sich bei Eintreten bestimmter von Ihnen festgelegter Bedingungen benachrichtigen lassen.

Sie können Warnungen basierend auf folgenden Elementen erhalten:

* **Metrikwerte**: Diese Warnung wird ausgelöst, wenn der Wert für eine bestimmte Metrik einen von Ihnen definierten Schwellenwert über- oder unterschreitet. Das Auslösen erfolgt sowohl, wenn die Bedingung erstmals erfüllt wird, als auch danach, wenn diese Bedingung nicht mehr erfüllt wird.
* **Aktivitätsprotokollereignisse**: Diese Warnung kann für jedes Ereignis oder nur dann ausgelöst werden, wenn eine bestimmte Anzahl von Ereignissen aufgetreten ist.

## <a name="alerts-in-different-azure-services"></a>Warnungen bei unterschiedlichen Azure-Diensten
Warnungen sind für verschiedene Dienste verfügbar, z.B. die folgenden:

* **Application Insights**: Ermöglicht Webtest- und Metrikwarnungen. Informationen dazu finden Sie unter [Einrichten von Warnungen in Application Insights](../application-insights/app-insights-alerts.md) und [Überwachen der Verfügbarkeit und Reaktionsfähigkeit von Websites](../application-insights/app-insights-monitor-web-app-availability.md).
* **Log Analytics (Operations Management Suite)**: Ermöglicht die Weiterleitung von Diagnoseprotokollen an Log Analytics. Die Operations Management Suite ermöglicht Metrik-, Protokollwarnungen und andere Typen von Warnungen. Weitere Informationen finden Sie unter [Warnungen in Log Analytics](../log-analytics/log-analytics-alerts.md).  
* **Azure Monitor**: Ermöglicht das Festlegen von Warnungen basierend auf Metrikwerten und Aktivitätsprotokollereignissen. Azure Monitor enthält die [Azure Monitor-REST-API](https://msdn.microsoft.com/library/dn931943.aspx).  Weitere Informationen finden Sie unter [Nutzung des Azure-Portals, von PowerShell oder der Befehlszeilenschnittstelle zum Erstellen von Warnungen](insights-alerts-portal.md).

## <a name="alert-actions"></a>Warnungsaktionen
Sie können eine Warnung so konfigurieren, dass folgende Aktionen ausgeführt werden:

* Senden von E-Mail-Benachrichtigungen an den Dienstadministrator, an Co-Administratoren oder an zusätzliche von Ihnen festgelegte E-Mail-Adressen
* Aufrufen eines Webhooks, um zusätzliche automatisierte Aktionen auszuführen
  
  ![Warnungen im Überblick](./media/monitoring-overview-alerts/AlertsOverviewResource3.png)

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen über Warnregeln und ihre Konfiguration erhalten Sie hier:

* [Azure-Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [Befehlszeilenschnittstelle](insights-alerts-command-line-interface.md)
* [Azure Monitor-REST-API](https://msdn.microsoft.com/library/azure/dn931945.aspx)




<!--HONumber=Nov16_HO3-->


