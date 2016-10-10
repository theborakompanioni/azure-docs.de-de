<properties
	pageTitle="Übersicht über Metriken in Microsoft Azure | Microsoft Azure"
	description="Übersicht über Metriken und ihre Verwendung in Microsoft Azure"
	authors="kamathashwin"
	manager=""
	editor=""
	services="monitoring-and-diagnostics"
	documentationCenter="monitoring-and-diagnostics"/>

<tags
	ms.service="monitoring-and-diagnostics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/26/2016"
	ms.author="ashwink"/>

# Überblick über Metriken in Microsoft Azure 

In diesem Artikel wird beschrieben, was Metriken in Microsoft Azure sind, welche Vorteile sie haben und wie Sie mit deren Nutzung beginnen.

## Was sind Metriken?

Mit Azure Monitor können Sie Telemetriedaten verwenden, um sich einen Überblick über Leistung und Integrität Ihrer Workloads in Azure zu verschaffen. Die wichtigsten Typen von Telemetriedaten sind Metriken (auch Leistungsindikatoren genannt), die von den meisten Azure-Ressourcen ausgegeben werden. Azure Monitor bietet Ihnen verschiedene Möglichkeiten, diese Metriken für die Überwachung und Problembehandlung zu konfigurieren und zu nutzen.


## Wozu kann ich Metriken nutzen?

Metriken sind eine wertvolle Quelle für Telemetriedaten, mit denen Sie folgende Aktionen durchführen können:

- **Verfolgen der Leistung** Ihrer Ressourcen (z.B. virtuelle Computer, Websites oder Logik-Apps) durch Darstellung der entsprechenden Metriken in einem Diagramm im Portal, das Sie an Ihr Dashboard anheften.
- **Erhalten von Benachrichtigungen über Probleme**, die die Leistung Ihrer Ressourcen beeinträchtigen, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.
- **Konfigurieren automatisierter Aktionen**, z.B. der automatischen Skalierung einer Ressource oder der Auslösung eines Runbooks, wenn ein Metrikwert einen bestimmten Schwellenwert überschreitet.
- **Ausführen fortgeschrittener Analysen** oder Erstellen von Berichten über Leistungs- oder Nutzungstrends für Ihre Ressource(n).
- **Archivieren** des Leistungs- oder Integritätsverlaufs Ihrer Ressourcen zu **Kompatibilitäts-/Überwachungszwecken**.

##  Merkmale von Metriken
Metriken weisen folgende Merkmale auf:

- Sämtliche Metriken werden **in einem 1-Minutentakt erfasst**. Jede Minute erhalten Sie von Ihrer Ressource einen Metrikwert, der Ihnen einen Überblick über den Zustand und die Integrität Ihrer Ressource nahezu in Echtzeit bietet.
- Metriken sind **sofort einsetzbar, ohne dafür zusätzliche Diagnosefunktionen aktivieren oder einrichten zu müssen**.
- Sie haben **30 Tage lang Zugriff auf den Verlauf** jeder Metrik. Sie können schnell den aktuellen und monatlichen Trend für die Leistung oder Integrität Ihrer Ressource überblicken.

Ihre Möglichkeiten:

- Einfaches Entdecken, Zugreifen auf und **Anzeigen all Ihrer Metriken** über das Azure-Portal, wenn Sie eine Ressource auswählen und in einem Diagramm darstellen.
- Konfigurieren einer **Warnungsregel für eine Metrik, die eine Benachrichtigung sendet oder eine automatisierte Aktion ausführt**, sobald der Metrikwert einen von Ihnen festgelegten Schwellenwert überschreitet. Die automatische Skalierung ist eine besondere automatisierte Aktion, mit der Sie Ihre Ressourcen horizontal hochskalieren können, um die auf Ihrer Website oder Ihren Computeressourcen eingehenden Anforderungen oder Lasten zu bewältigen. Sie können eine Regel für die automatische Skalierung konfigurieren, die das horizontale Hoch- und Herunterskalieren auslöst, sobald eine Metrik einen bestimmten Schwellenwert überschreitet.
- **Archivieren** von Metriken für längere Zeit oder Verwendung von Metriken zur Erstellung von Offlineberichten. Sie können Ihre Metriken an Blobspeicher weiterleiten, wenn Sie die Diagnoseeinstellungen für Ihre Ressource konfigurieren.
- **Streamen** von Metriken an einen Event Hub, wodurch Sie sie dann an Azure Stream Analytics oder benutzerdefinierte Apps weiterleiten können, um Analysen nahezu in Echtzeit durchführen zu können. Dies können Sie in den Diagnoseeinstellungen festlegen.
- **Weiterleiten** aller Metriken an OMS Log Analytics, um unmittelbare Analysen, Suchen und benutzerdefinierte Warnungen für von Ihren Ressourcen gesendete Metrikdaten zu ermöglichen.
- **Verwenden** von Metriken über neue Azure Monitor-REST-APIs.
- **Abfragen** von Metriken mithilfe von PowerShell-Cmdlets oder der plattformübergreifenden REST-API.

 ![Routing von Metriken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview0.png)

## Zugreifen auf Metriken über das Portal
An dieser Stelle folgt eine kurze exemplarische Vorgehensweise zur Erstellung eines Metrikdiagramms über das Azure-Portal.

### Anzeigen von Metriken nach dem Erstellen einer Ressource
1. Öffnen Sie das Azure-Portal.
2. Erstellen Sie eine neue App Service-Website.
3. Nachdem Sie eine Website erstellt haben, wechseln Sie zum Blatt „Übersicht“ der Website.
4. Sie können nun neue Metriken als Kachel „Überwachung“ anzeigen. Sie können die Kachel bearbeiten und weitere Metriken auswählen.

 ![Metriken für eine Ressource in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview1.png)

### Zugreifen auf alle Metriken an einer zentralen Stelle
1. Öffnen Sie das Azure-Portal.
2. Navigieren Sie zur neuen Registerkarte „Überwachen“, und wählen Sie darunter die Option „Metriken“ aus.
3. In der Dropdownliste können Sie Ihr Abonnement, Ihre Ressourcengruppe und den Namen der Ressource auswählen.
4. Nun wird Ihnen die Liste mit den verfügbaren Metriken angezeigt.
5. Wählen Sie die Metrik aus, die Sie interessiert, und stellen Sie sie dar.
6. Durch Klicken auf die Nadel in der oberen rechten Ecke können Sie sie an das Dashboard anheften.

 ![Zugreifen auf alle Metriken von einer zentralen Stelle in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview2.png)


>[AZURE.NOTE] Sie können auf Metriken auf Hostebene von virtuellen Computern (basierend auf Azure Resource Manager) und VM-Skalierungsgruppen aus zugreifen, ohne eine zusätzliche Diagnose einzurichten. Diese neuen Metriken auf Hostebene sind für Windows- und Linux-Instanzen verfügbar. Diese Metriken sind nicht zu verwechseln mit Metriken auf Gastbetriebssystemebene, auf die Sie zugreifen können, wenn Sie die Azure-Diagnose auf Ihren virtuellen Computern oder in Ihren VM-Skalierungsgruppen eingeschaltet haben. Weitere Informationen zum Konfigurieren der Azure-Diagnose finden Sie unter [Was ist die Microsoft Azure-Diagnose?](../azure-diagnostics.md).

## Zugreifen auf Metriken über die REST-API
Azure-Metriken sind über Azure Monitor-APIs zugänglich. Es gibt zwei APIs, mit denen Sie auf Metriken zugreifen können. Diese sind:

- [Die Azure Monitor-REST-API für Metrikdefinitionen](https://msdn.microsoft.com/library/mt743621.aspx) für den Zugriff auf die Liste verfügbarer Metriken für einen Dienst.
- [Die Azure Monitor-REST-API für Metriken](https://msdn.microsoft.com/library/mt743622.aspx) für den Zugriff auf die eigentlichen Metrikdaten.

>[AZURE.NOTE] In diesem Artikel geht es um die Metriken, die über die [neue API für Metriken](https://msdn.microsoft.com/library/dn931930.aspx) für Azure-Ressourcen abgerufen werden können. Die aktuelle API-Version für die neue Metrikdefinitions-API lautet 2016-03-01, die Version für die Metrik-API lautet 2016-09-01. Auf die Legacy-Metrikdefinitionen und -Metriken kann über die API-Version 2014-04-01 zugegriffen werden.

Eine ausführliche exemplarische Vorgehensweise unter Verwendung der Azure Monitor-REST-APIs finden Sie unter [Exemplarische Vorgehensweise zur Azure Monitor-REST-API](monitoring-rest-api-walkthrough.md).

## Exportoptionen für Metriken
Die Exportoptionen für Metriken können Sie auf der Registerkarte „Monitor“ auf dem Blatt „Diagnoseprotokolle“ anzeigen. Sie können Metriken (und Diagnoseprotokolle) zum Weiterleiten an Blob Storage, Event Hubs oder OMS Log Analytics für die Anwendungsfälle auswählen, die wir zuvor in diesem Artikel erwähnt haben.

 ![Exportoptionen für Metriken in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview3.png)

Dies können Sie über Resource Manager-Vorlagen, [PowerShell](insights-powershell-samples.md), [CLI](insights-cli-samples.md) oder [REST-APIs](https://msdn.microsoft.com/library/dn931943.aspx) konfigurieren.

## Ausführen von Aktionen für Metriken
Sie können Benachrichtigungen empfangen oder automatisierte Aktionen für Metrikdaten ausführen. Dafür können Sie Warnungsregeln oder Einstellungen zur automatischen Skalierung konfigurieren.

### Warnungsregeln
Sie können Warnungsregeln zu Metriken konfigurieren. Mithilfe dieser Warnungsregeln können Sie festlegen, dass Sie per E-Mail benachrichtigt werden oder ein Webhook ausgelöst wird, wenn eine Metrik einen bestimmten Schwellenwert überschreitet. Der Webhook kann dann dazu genutzt werden, ein benutzerdefiniertes Skript auszuführen. Außerdem können Sie mit dem Webhook Integrationen von Drittanbieterprodukten konfigurieren.

 ![Metriken und Warnungsregeln in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview4.png)

### Autoscale
Einige Azure-Ressourcen unterstützen mehrere Instanzen zum horizontalen Hoch- oder Herunterskalieren, um Ihre Workloads verarbeiten zu können. Die automatische Skalierungsfunktion steht für App Services (Web-Apps), VM-Skalierungsgruppen (VMSS) und klassische Cloud Services zur Verfügung. Sie können Regeln zur automatischen Skalierung konfigurieren, damit horizontal hoch- oder herunterskaliert wird, sobald eine bestimmte, für Ihre Workload relevante Metrik einen von Ihnen festgelegten Schwellenwert überschreitet. Weitere Informationen finden Sie in der [Übersicht über die automatische Skalierung](monitoring-overview-autoscale.md).

 ![Metriken und automatische Skalierung in Azure Monitor](./media/monitoring-overview-metrics/MetricsOverview5.png)

## Unterstützte Dienste und Metriken
Azure Monitor ist eine neue Metrikinfrastruktur. Sie bietet Unterstützung für die folgenden Azure-Dienste im Azure-Portal sowie die neue Version der Azure Monitor-API:

- Virtuelle Computer (basierend auf Azure Resource Manager)
- VM-Skalierungsgruppen
- Batch
- Event Hub-Namespace
- Service Bus-Namespace (nur bei Premium-SKU)
- SQL (Version 12)
- Elastischer SQL-Pool
- Websites
- Webserverfarmen
- Logik-Apps
- IoT Hubs
- Redis-Cache
- Netzwerke: Anwendungsgateways
- Suche

Eine ausführliche Liste aller unterstützter Dienste mit den entsprechenden Metriken finden Sie unter [Azure Monitor-Metriken – unterstütze Metriken pro Ressourcentyp](monitoring-supported-metrics.md).


## Nächste Schritte

Weitere Informationen finden Sie auch über die Links in diesem Artikel. Außerdem erfahren Sie etwas zu folgenden Themen:

- [Allgemeine Metriken für die automatische Skalierung](insights-autoscale-common-metrics.md)
- [Erstellen von Warnungsregeln](insights-alerts-portal.md)

<!---HONumber=AcomDC_0928_2016-->