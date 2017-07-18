---
title: "Überblick über Metriken in Microsoft Azure | Microsoft-Dokumentation"
description: "Überblick über Metriken und ihre Verwendung in Microsoft Azure"
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 405ec51c-0946-4ec9-b535-60f65c4a5bd1
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: johnkem
ms.translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 86e025f9211a1d7ed07e831b7ce4c21be351513b
ms.contentlocale: de-de
ms.lasthandoff: 03/09/2017

---

# <a name="overview-of-metrics-in-microsoft-azure"></a>Überblick über Metriken in Microsoft Azure
In diesem Artikel wird beschrieben, was Metriken in Microsoft Azure sind, welche Vorteile sie haben und wie Sie mit deren Nutzung beginnen.  

## <a name="what-are-metrics"></a>Was sind Metriken?
Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese Metriken für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.

## <a name="what-can-you-do-with-metrics"></a>Wozu kann ich Metriken nutzen?
Metriken sind eine wertvolle Quelle für Telemetriedaten, mit denen Sie folgende Aufgaben ausführen können:

* **Verfolgen der Leistung** Ihrer Ressourcen (z.B. virtuelle Computer, Websites oder Logik-Apps) durch Darstellung der entsprechenden Metriken in einem Diagramm im Portal, das Sie an Ihr Dashboard anheften.
* **Erhalten von Benachrichtigungen über Probleme**, die die Leistung Ihrer Ressourcen beeinträchtigen, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.
* **Konfigurieren automatisierter Aktionen**, z. B. der automatischen Skalierung einer Ressource oder der Auslösung eines Runbooks, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.
* **Ausführen fortgeschrittener Analysen** oder Erstellen von Berichten zu Leistungs- oder Nutzungstrends für Ihre Ressource.
* **Archivieren** des Leistungs- oder Integritätsverlaufs Ihrer Ressourcen zu **Kompatibilitäts- oder Überwachungszwecken**.

## <a name="what-are-the-characteristics-of-metrics"></a>Was sind die Merkmale von Metriken?
Metriken weisen folgende Merkmale auf:

* Sämtliche Metriken werden im **1-Minuten-Takt** erfasst. Jede Minute erhalten Sie von Ihrer Ressource einen Metrikwert, der Ihnen einen Überblick über den Zustand und die Integrität Ihrer Ressource nahezu in Echtzeit bietet.
* Metriken sind **sofort verfügbar**. Sie müssen sie nicht aktivieren oder zusätzliche Diagnosen einrichten.
* Sie haben **30 Tage lang Zugriff auf den Verlauf** jeder Metrik. Sie können schnell den aktuellen und monatlichen Trend für die Leistung oder Integrität Ihrer Ressource überblicken.

Weitere Funktionen:

* Konfigurieren einer **Warnungsregel für eine Metrik, die eine Benachrichtigung sendet oder eine automatisierte Aktion ausführt**, sobald der Metrikwert einen von Ihnen festgelegten Schwellenwert überschreitet. Die automatische Skalierung ist eine besondere automatisierte Aktion, mit der Sie Ihre Ressource horizontal hochskalieren können, um die auf Ihrer Website oder Ihren Computeressourcen eingehenden Anforderungen oder Lasten zu bewältigen. Sie können eine Regel für die automatische Skalierung konfigurieren, die das horizontale Hoch- und Herunterskalieren auslöst, sobald eine Metrik einen bestimmten Schwellenwert überschreitet.

* **Leiten** Sie alle Metriken an Application Insights oder Log Analytics (OMS) weiter, um unmittelbare Analysen, Suchen und benutzerdefinierte Warnungen für von Ihren Ressourcen gesendete Metrikdaten zu ermöglichen. Sie können auch Metriken an einen Event Hub streamen, wodurch Sie sie dann an Azure Stream Analytics oder benutzerdefinierte Apps weiterleiten können, um Analysen nahezu in Echtzeit durchführen zu können. Event Hub-Streaming wird mit Diagnoseeinstellungen eingerichtet.

* **Archivieren Sie Metriken im Speicher**, um sie länger aufzubewahren oder zur Erstellung von Offlineberichten zu verwenden. Sie können Ihre Metriken an Azure Blob Storage weiterleiten, wenn Sie die Diagnoseeinstellungen für Ihre Ressource konfigurieren.

* Einfaches Entdecken, Zugreifen auf und **Anzeigen aller Metriken** über das Azure-Portal, wenn Sie eine Ressource auswählen und die Metriken in einem Diagramm darstellen.

* **Nutzen** von Metriken über die neuen Azure Monitor-REST-APIs.

* **Abfragen** von Metriken mithilfe von PowerShell-Cmdlets oder der plattformübergreifenden REST-API.

  ![Routing von Metriken in Azure Monitor](./media/monitoring-overview-metrics/Metrics_Overview_v4.png)

## <a name="access-metrics-via-the-portal"></a>Zugreifen auf Metriken über das Portal
Es folgt eine kurze exemplarische Vorgehensweise zum Erstellen eines Metrikdiagramm mithilfe des Azure-Portals.

### <a name="to-view-metrics-after-creating-a-resource"></a>So zeigen Sie Metriken nach Erstellen einer Ressource an
1. Öffnen Sie das Azure-Portal.
2. Erstellen Sie eine Azure App Service-Website.
3. Wechseln Sie nach dem Erstellen einer Website zum Blatt **Übersicht** der Website.
4. Auf der Kachel **Überwachung** können Sie neue Metriken anzeigen. Sie können die Kachel bearbeiten und weitere Metriken auswählen.

   ![Metriken für eine Ressource in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### <a name="to-access-all-metrics-in-a-single-place"></a>So greifen Sie zentral auf alle Metriken zu
1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zur neuen Registerkarte **Überwachen**, und wählen Sie darunter die Option **Metriken** aus.
3. Wählen Sie in der Dropdownliste Ihr Abonnement, Ihre Ressourcengruppe und den Namen der Ressource aus.
4. Sehen Sie sich die Liste mit den verfügbaren Metriken an. Wählen Sie die Metrik aus, die Sie interessiert, und stellen Sie sie dar.
5. Durch Klicken auf den Stift in der rechten oberen Ecke können Sie sie an das Dashboard heften.

   ![Zugreifen auf alle Metriken von einer zentralen Stelle in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)

> [!NOTE]
> Sie können auf Metriken auf Hostebene von VMs (basierend auf Azure Resource Manager) und VM-Skalierungsgruppen aus zugreifen, ohne eine zusätzliche Diagnose einzurichten. Diese neuen Metriken auf Hostebene sind für Windows- und Linux-Instanzen verfügbar. Diese Metriken sind nicht zu verwechseln mit Metriken auf Gastbetriebssystemebene, auf die Sie zugreifen können, wenn Sie die Azure-Diagnose auf Ihren virtuellen Computern oder in Ihren VM-Skalierungsgruppen eingeschaltet haben. Weitere Informationen zum Konfigurieren der Diagnose finden Sie unter [Was ist Microsoft Azure-Diagnose?](../azure-diagnostics.md).
>
>

## <a name="access-metrics-via-the-rest-api"></a>Zugreifen auf Metriken über die REST-API
Auf Azure-Metriken kann über Azure Monitor-APIs zugegriffen werden. Es gibt zwei APIs, mit denen Sie Metriken ermitteln und darauf zugreifen können:

* Verwenden Sie die [Azure Monitor-REST-API für Metrikdefinitionen](https://msdn.microsoft.com/library/mt743621.aspx) für den Zugriff auf die Liste von Metriken, die für einen Dienst verfügbar sind.
* Verwenden Sie die [Azure Monitor-REST-API für Metriken](https://msdn.microsoft.com/library/mt743622.aspx) für den Zugriff auf die eigentlichen Metrikdaten.

> [!NOTE]
> In diesem Artikel geht es um die Metriken, die über die [neue API für Metriken](https://msdn.microsoft.com/library/dn931930.aspx) für Azure-Ressourcen abgerufen werden können. Die aktuelle API-Version für die neue Metrikdefinitions-API lautet 2016-03-01, die Version für die Metrik-API lautet 2016-09-01. Auf die Legacy-Metrikdefinitionen und -Metriken kann über die API-Version 2014-04-01 zugegriffen werden.
>
>

Eine ausführliche exemplarische Vorgehensweise unter Verwendung der Azure Monitor-REST-APIs finden Sie unter [Exemplarische Vorgehensweise zur Azure Monitor-REST-API](monitoring-rest-api-walkthrough.md).

## <a name="export-metrics"></a>Exportieren von Metriken
Die Exportoptionen für Metriken können Sie auf der Registerkarte **Monitor** auf dem Blatt **Diagnoseprotokolle** anzeigen. Sie können Metriken (und Diagnoseprotokolle) zum Weiterleiten an Blob Storage, Azure Event Hubs oder OMS für die Anwendungsfälle auswählen, die wir zuvor in diesem Artikel erwähnt haben.

 ![Exportoptionen für Metriken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Dies können Sie über Resource Manager-Vorlagen, [PowerShell](insights-powershell-samples.md), die [Azure CLI](insights-cli-samples.md) oder [REST-APIs](https://msdn.microsoft.com/library/dn931943.aspx) konfigurieren.

## <a name="take-action-on-metrics"></a>Ausführen von Aktionen für Metriken
Um Benachrichtigungen zu empfangen oder aufgrund von Metrikdaten automatisierte Aktionen auszuführen, können Sie Warnungsregeln oder Einstellungen für die automatische Skalierung konfigurieren.

### <a name="configure-alert-rules"></a>Konfigurieren von Warnungsregeln
Sie können Warnungsregeln zu Metriken konfigurieren. Diese Warnungsregeln können überprüfen, ob eine Metrik einen bestimmten Schwellenwert überschritten hat. Sie können Sie dann per E-Mail benachrichtigen oder einen Webhook auslösen, der zum Ausführen eines benutzerdefinierten Skripts genutzt werden kann. Außerdem können Sie mit dem Webhook Integrationen von Drittanbieterprodukten konfigurieren.

 ![Metriken und Warnungsregeln in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### <a name="autoscale-your-azure-resources"></a>Automatische Skalierung Ihrer Azure-Ressourcen
Einige Azure-Ressourcen unterstützen das horizontale Hoch- oder Herunterskalieren mehrerer Instanzen, um Ihre Workloads zu bewältigen. Die automatische Skalierungsfunktion steht für App Services (Web-Apps), VM-Skalierungsgruppen und klassische Azure Cloud Services zur Verfügung. Sie können Regeln zur automatischen Skalierung konfigurieren, damit horizontal hoch- oder herunterskaliert wird, sobald eine bestimmte, für Ihre Workload relevante Metrik einen von Ihnen festgelegten Schwellenwert überschreitet. Weitere Informationen finden Sie in der [Übersicht über die automatische Skalierung](monitoring-overview-autoscale.md).

 ![Metriken und automatische Skalierung in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## <a name="learn-about-supported-services-and-metrics"></a>Informationen zu unterstützten Diensten und Metriken
Azure Monitor ist eine neue Metrikinfrastruktur. Sie unterstützt die folgenden Azure-Dienste im Azure-Portal sowie die neue Version der Azure Monitor-API:

* VMs (auf Azure Resource Manager-Basis)
* VM-Skalierungsgruppen
* Batch
* Event Hubs-Namespace
* Service Bus-Namespace (nur bei Premium-SKU)
* SQL-Datenbank (Version 12)
* Elastischer SQL-Pool
* Websites
* Webserverfarmen
* Logik-Apps
* IoT Hubs
* Redis Cache
* Netzwerke: Application Gateways
* Suche

Eine ausführliche Liste aller unterstützter Dienste mit den entsprechenden Metriken finden Sie unter [Azure Monitor-Metriken – unterstütze Metriken pro Ressourcentyp](monitoring-supported-metrics.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie auch über die Links in diesem Artikel. Außerdem erfahren Sie etwas zu folgenden Themen:  

* [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md)
* [Erstellen von Warnungsregeln](insights-alerts-portal.md)
* [Analysieren von Protokollen aus Azure Storage mit Log Analytics](../log-analytics/log-analytics-azure-storage.md)

