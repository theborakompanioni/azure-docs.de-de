---
title: "Übersicht über Azure Monitor | Microsoft-Dokumentation"
description: "Azure Monitor erfasst Statistiken zur Verwendung in Warnungen, in Webhooks, bei der automatischen Skalierung und bei der Automatisierung. Im Artikel werden auch andere Microsoft-Überwachungsoptionen aufgeführt."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 1b962c74-8d36-4778-b816-a893f738f92d
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/02/2017
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: ca55f1c295d99ff92abaef1a919d4c90d3c33b8a
ms.lasthandoff: 03/09/2017


---

# <a name="overview-of-azure-monitor"></a>Übersicht über Azure Monitor
Dieser Artikel enthält eine konzeptionelle Übersicht über das Überwachen von Azure-Ressourcen. Es enthält Verweise auf Informationen zu bestimmten Ressourcentypen.  Allgemeine Informationen zur Überwachung Ihrer Anwendung aus einer Nicht-Azure-basierten Perspektive finden Sie unter [Anleitung zur Überwachung und Diagnose](../best-practices-monitoring.md).

Ein Video mit einer exemplarischen Vorgehensweise für Azure Monitor finden Sie unter  
[Get Started with Azure Monitor](https://channel9.msdn.com/Blogs/Azure-Monitoring/Get-Started-with-Azure-Monitor) (Erste Schritte mit Azure Monitor). Ein weiteres Video, in dem ein Szenario zur Verwendung von Azure Monitor erläutert wird, ist unter [Explore Microsoft Azure monitoring and diagnostics](https://channel9.msdn.com/events/Ignite/2016/BRK2234) (Erkunden der Microsoft Azure-Überwachung und -Diagnose) verfügbar.  

Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme. Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Kenntnisse können Sie die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder Aktionen automatisieren, die andernfalls manuell ausgeführt werden müssten.

Im folgenden Diagramm wird das Konzept der Azure-Überwachung dargestellt (einschließlich der Art von Protokollen, die Sie erfassen können, und den Verwendungsmöglichkeiten der jeweiligen Daten):   

![Modell zur Überwachung und Diagnose für Nicht-Computeressourcen](./media/monitoring-overview/Monitoring_Azure_Resources-compute_v4.png)

Abbildung 1: Konzeptmodell zur Überwachung und Diagnose computefremder Ressourcen

<br/>

![Modell zur Überwachung und Diagnose für Computeressourcen](./media/monitoring-overview/Monitoring_Azure_Resources-non-compute_v4.png)

Abbildung 2: Konzeptmodell zur Überwachung und Diagnose von Computeressourcen

## <a name="monitoring-sources"></a>Überwachungsquellen
### <a name="activity-logs"></a>Aktivitätsprotokolle
Sie können das Aktivitätsprotokoll (zuvor als Betriebs- oder Überwachungsprotokoll bezeichnet) nach Informationen zu Ihrer Ressource aus Sicht der Azure-Infrastruktur durchsuchen. Dieses Protokoll enthält Informationen wie etwa den Erstellungs- oder Löschzeitpunkt von Ressourcen.  

### <a name="host-vm"></a>Virtueller Hostcomputer
**Nur Compute**

Einige Computeressourcen wie Cloud Services, Virtual Machines und Service Fabric verfügen über einen dedizierten virtuellen Hostcomputer, mit dem sie interagieren. Der virtuelle Hostcomputer entspricht dem virtuellen Stammcomputer im Hyper-V-Hypervisormodell. In diesem Fall können Sie Metriken zum virtuellen Hostcomputer alleine zusätzlich zum Gastbetriebssystem erfassen.  

Bei anderen Azure-Diensten besteht nicht zwangsläufig eine 1:1-Zuordnung zwischen Ihrer Ressource und einem bestimmten virtuellen Hostcomputer, sodass keine Metriken für den virtuellen Hostcomputer verfügbar sind.

### <a name="resource---metrics-and-diagnostics-logs"></a>Ressource – Metriken und Diagnoseprotokolle
Die erfassbaren Metriken variieren je nach Ressourcentyp. Virtual Machines stellt beispielsweise eine Statistik zu Datenträger-E/A und CPU-Auslastung in Prozent bereit. Für eine Service Bus-Warteschlange sind diese Statistikwerte jedoch nicht verfügbar. Daher werden hier stattdessen Metriken wie Warteschlangengröße und Durchsatz bereitgestellt.

Für Computeressourcen können Sie Metriken zum Gastbetriebssystem und zu Diagnosemodulen wie der Azure-Diagnose abrufen. Die Azure-Diagnose unterstützt Sie beim Erfassen und Weiterleiten von Diagnosedaten an andere Speicherorte (etwa Azure Storage).

Eine Liste der derzeit erfassbaren Metriken finden Sie unter [unterstützte Metriken](monitoring-supported-metrics.md).

### <a name="application---diagnostics-logs-application-logs-and-metrics"></a>Anwendung – Diagnoseprotokolle, Anwendungsprotokolle und Metriken
**Nur Compute**

Anwendungen können zusätzlich zum Gastbetriebssystem im Computemodell ausgeführt werden. Sie geben ihre eigenen Protokollsätze und Metriken aus.

Zu diesen Metriktypen gehören Folgende:

* Leistungsindikatoren
* Anwendungsprotokolle
* Windows-Ereignisprotokolle
* .NET-Ereignisquelle
* IIS-Protokolle
* Manifestbasiertes ETW
* Absturzabbilder
* Fehlerprotokolle für Kundenanwendungen

## <a name="uses-for-monitoring-data"></a>Verwendungszwecke für Überwachungsdaten
### <a name="visualize"></a>Visualisieren
Indem Sie Ihre Überwachungsdaten in Grafiken und Diagrammen visualisieren, können Sie Trends deutlich schneller ausmachen, als wenn Sie die Daten selbst ansehen.  

Einige Beispiele für Visualisierungsmethoden:

* Verwenden des Azure-Portals
* Weiterleiten von Daten an Azure Application Insights
* Weiterleiten von Daten an Microsoft Power BI
* Weiterleiten von Daten an ein Visualisierungstool eines Drittanbieters mittels Livestreaming oder durch Lesen eines Archivs in Azure Storage

### <a name="archive"></a>Archivieren
Die Überwachungsdaten werden in der Regel in den Azure-Speicher geschrieben und dort solange gespeichert, bis Sie sie löschen.

Einige Möglichkeiten, diese Daten zu nutzen:

* Sobald die Daten geschrieben wurden, können Sie andere Tools innerhalb oder außerhalb von Azure verwenden, um die Daten zu lesen und zu verarbeiten
* Sie laden die Daten in ein lokales Archiv herunter oder verändern Ihre Aufbewahrungsrichtlinie in der Cloud, um die Daten über längere Zeiträume zu speichern  
* <a name="you-leave-the-data-in-azure-storage-indefinitely-though-you-have-to-pay-for-azure-storage-based-on-the-amount-of-data-you-keep"></a>Sie belassen die Daten auf unbestimmte Zeit in Azure Storage, obwohl Sie Azure Storage anhand der Menge an aufbewahrten Daten bezahlen.
  -

### <a name="query"></a>Abfrage
Sie können die Azure Monitor-REST-API, Befehle der plattformübergreifenden Befehlszeilenschnittstelle, PowerShell-Cmdlets oder das .NET SDK verwenden, um auf die Daten im System oder in Azure Storage zuzugreifen.

Beispiele: 

* Abrufen von Daten für eine von Ihnen geschriebene, benutzerdefinierte Überwachungsanwendung
* Erstellen von benutzerdefinierten Abfragen und Senden der Daten an eine Drittanbieteranwendung

### <a name="route"></a>Weiterleiten
Sie können Überwachungsdaten in Echtzeit an andere Speicherorte streamen.

Beispiele:

* Senden der Daten an Application Insights, um die dort vorhandenen Visualisierungstools zu verwenden
* Senden der Daten an Event Hubs, um sie an Drittanbietertools weiterleiten und Echtzeitanalysen durchführen zu können

### <a name="automate"></a>Automatisieren
Sie können die Überwachungsdaten nutzen, um Warnungen, Ereignisse oder sogar ganze Prozesse auszulösen.
Beispiele:

* Verwenden der Daten, um Compute-Instanzen je nach Anwendungslast automatisch horizontal hoch- oder herunterzuskalieren
* Senden von E-Mails, wenn eine Metrik einen vorgegebenen Schwellenwert überschreitet
* Aufrufen einer Web-URL (Webhook), um eine Aktion in einem System außerhalb von Azure auszuführen
* Starten eines Runbooks in Azure Automation, um verschiedenste Aufgaben auszuführen

## <a name="methods-of-use"></a>Nutzungsmethoden
Nachverfolgung, Routing und Abfrage von Daten können ganz allgemein mit einer der folgenden Methoden angepasst werden. Manche Methoden sind unter Umständen für bestimmte Aktionen oder Datentypen nicht verfügbar.

* [Azure-Portal](https://portal.azure.com)
* [PowerShell](insights-powershell-samples.md)  
* [Plattformübergreifende Befehlszeilenschnittstelle](insights-cli-samples.md)
* [REST-API](https://msdn.microsoft.com/library/dn931943.aspx)
* [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## <a name="azures-monitoring-offerings"></a>Überwachungsfunktionen von Azure
Azure bietet verschiedene Möglichkeiten zur Überwachung Ihrer Dienste – von der Bare-Metal-Infrastruktur bis hin zur Anwendungstelemetrie. Die beste Überwachungsstrategie kombiniert alle Methoden, um umfassende und detaillierte Einblicke in die Integrität Ihrer Dienste zu gewinnen.

* [Azure Monitor:](http://aka.ms/azmondocs) bietet Visualisierung, Abfrage, Weiterleitung, Warnung, automatische Skalierung und Automatisierung für Daten sowohl aus der Azure-Infrastruktur (Aktivitätsprotokoll) als auch aus jeder einzelnen Azure-Ressource (Diagnoseprotokolle). Dieser Artikel ist Teil der Azure Monitor-Dokumentation. Der Name „Azure Monitor“ wurde am 25. September bei der Ignite 2016 bekannt gegeben.  Der vorherige Name lautete „Azure Insights“.  
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) : Bietet umfangreiche Erkennungs- und Diagnosefunktionen für Probleme auf der Anwendungsebene des Diensts sowie Integration in die Daten der Azure-Überwachung. Es handelt sich um die Standarddiagnoseplattform für App Service-Web-Apps.  Sie können Daten von anderen Diensten an sie weiterleiten.  
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/), Teil der [Operations Management Suite](https://www.microsoft.com/oms/): bietet eine vollständige IT-Verwaltungslösung für lokale und cloudbasierte Infrastrukturen (z.B. AWS) zusätzlich zu Azure-Ressourcen.  Daten von Azure Monitor können direkt an Log Analytics weitergeleitet werden, sodass Sie die Metriken und Protokolle für Ihre gesamte Umgebung an einem Ort finden.     

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen

* [Azure Monitor in einem Video von der Ignite 2016](https://myignite.microsoft.com/videos/4977)
* [Erste Schritte mit Azure Monitor](monitoring-get-started.md)
* [Azure-Diagnose](../azure-diagnostics.md) : Diagnostizieren Sie Probleme mit Ihren Clouddiensten, virtuellen Computern oder der Service Fabric-Anwendung.
* [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/) : Diagnostizieren Sie Probleme mit Ihrer App Service-Web-App.
* [Problembehandlung für Azure Storage](../storage/storage-e2e-troubleshooting.md) : Lösen Sie Probleme mit Storage-Blobs, Tabellen oder Warteschlangen.
* [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) und die [Operations Management Suite](https://www.microsoft.com/oms/)

