<properties
	pageTitle="Überblick über die Überwachung in Microsoft Azure | Microsoft Azure"
	description="Eine allgemeine Übersicht über die Überwachung und Diagnose in Microsoft Azure mit Warnungen, Webhooks, automatischer Skalierung und mehr."
	authors="rboucher"
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
	ms.date="09/06/2016"
	ms.author="robb"/>

# Überblick über die Überwachung in Microsoft Azure

Dieser Artikel enthält eine konzeptionelle Übersicht über die Überwachung von Azure-Ressourcen sowie Verweise auf weitere Informationen zu spezifischen Ressourcentypen. Allgemeine Informationen zur Überwachung Ihrer Anwendung aus einer Nicht-Azure-basierten Perspektive finden Sie unter [Anleitung zur Überwachung und Diagnose](../best-practices-monitoring.md).

Cloudanwendungen sind komplexe Systeme mit zahlreichen Variablen. Die Überwachung stellt Daten bereit, auf deren Grundlage die ordnungsgemäße Ausführung der Anwendung sichergestellt werden kann. Sie trägt auch zur Vermeidung potenzieller Probleme bei und hilft bei der Behandlung bereits aufgetretener Probleme. Darüber hinaus können Sie auf der Grundlage von Überwachungsdaten umfassende Erkenntnisse über Ihre Anwendung gewinnen. Mithilfe dieser Erkenntnisse können Sie ggf. die Leistung oder Wartungsfreundlichkeit der Anwendung verbessern oder einige Aktionen automatisieren, die ansonsten manuell ausgeführt werden müssten.

Im folgenden Diagramm wird das Konzept der Azure-Überwachung dargestellt (einschließlich der Art von Protokollen, die Sie erfassen können, und den Verwendungsmöglichkeiten der jeweiligen Daten):

![Logisches Modell zur Überwachung und Diagnose für Nicht-Computeressourcen](./media/monitoring-overview/monitoring-azure-resources-non-compute.png)

Abbildung 1: Konzeptmodell zur Überwachung und Diagnose computefremder Ressourcen

<br/>

![Logisches Modell zur Überwachung und Diagnose für Computeressourcen](./media/monitoring-overview/monitoring-azure-resources-compute.png)

Abbildung 2: Konzeptmodell zur Überwachung und Diagnose von Computeressourcen


## Überwachungsquellen
### Infrastrukturaktivitätsprotokolle
Sie können das Überwachungsprotokoll (gelegentlich auch Betriebs- oder Aktivitätsprotokoll genannt) nach Informationen zu Ihrer Ressource aus Sicht der Azure-Infrastruktur durchsuchen. Dieses Protokoll enthält Informationen wie etwa den Erstellungs- oder Löschzeitpunkt von Ressourcen.

### Virtueller Hostcomputer
**Nur Compute**

Sie können aktuell keine Metriken auf dem virtuellen Hostcomputer sammeln, die Aufführung dient der Vollständigkeit.

Einige Computeressourcen wie Cloud Services, Virtual Machines und Service Fabric verfügen über einen dedizierten virtuellen Hostcomputer, mit dem sie interagieren. Der virtuelle Hostcomputer entspricht dem virtuellen Stammcomputer im Hyper-V-Hypervisormodell.

Bei anderen Azure-Diensten besteht nicht zwangsläufig eine 1:1-Zuordnung zwischen Ihrer Ressource und einem bestimmten virtuellen Hostcomputer.


### Ressource – Metriken und Diagnoseprotokolle
Die erfassbaren Metriken variieren je nach Ressourcentyp. Virtual Machines stellt beispielsweise eine Statistik zu Datenträger-E/A und CPU-Auslastung in Prozent bereit. Für eine Service Bus-Warteschlange sind diese Statistikwerte jedoch nicht verfügbar. Daher werden hier stattdessen Metriken wie Warteschlangengröße und Durchsatz bereitgestellt.

Für Computeressourcen können Sie Metriken zum Gastbetriebssystem und zu Diagnosemodulen wie der Azure-Diagnose abrufen. Die Azure-Diagnose unterstützt Sie beim Erfassen und Weiterleiten von Diagnosedaten an andere Speicherorte (etwa Azure Storage).

### Anwendung – Diagnoseprotokolle, Anwendungsprotokolle und Metriken
**Nur Compute**

Anwendungen können zusätzlich zum Gastbetriebssystem im Computemodell ausgeführt werden. Sie geben ihre eigenen Protokollsätze und Metriken aus.

Zu diesen Metriktypen gehören Folgende:

- Leistungsindikatoren
- Anwendungsprotokolle
- Windows-Ereignisprotokolle
- .NET-Ereignisquelle
- IIS-Protokolle
- Manifestbasiertes ETW
- Absturzabbilder
- Fehlerprotokolle für Kundenanwendungen


## Verwendungszwecke für Überwachungsdaten

### Visualisieren
Indem Sie Ihre Überwachungsdaten in Grafiken und Diagrammen visualisieren, können Sie Trends deutlich schneller ausmachen, als wenn Sie die Daten selbst ansehen.

Im Anschluss finden Sie einige Visualisierungsmethoden:

- Verwenden des Azure-Portals
- Weiterleiten von Daten an Azure Application Insights
- Weiterleiten von Daten an Microsoft Power BI
- Weiterleiten von Daten an ein Visualisierungstool eines Drittanbieters – entweder mittels Livestreaming oder durch Lesen eines Archivs in Azure Storage

### Archivieren
Die Überwachungsdaten werden in der Regel in den Azure-Speicher geschrieben und dort solange gespeichert, bis Sie sie löschen.

Einige Möglichkeiten, diese Daten zu nutzen:

- Sobald die Daten geschrieben wurden, können Sie andere Tools innerhalb oder außerhalb von Azure verwenden, um die Daten zu lesen und zu verarbeiten
- Sie laden die Daten in ein lokales Archiv herunter oder verändern Ihre Aufbewahrungsrichtlinie in der Cloud, um die Daten über längere Zeiträume zu speichern
- Sie belassen die Daten auf unbestimmte Zeit im Azure-Speicher Beachten Sie, dass die Abrechnung für den Azure-Speicher auf der Menge der gespeicherten Daten basiert.

### Abfrage
Sie können die Insights-REST-API, die Befehlszeilenschnittstelle, PowerShell-Cmdlets oder das .NET SDK verwenden, um auf die Daten im System oder im Azure-Speicher zuzugreifen.

Beispiele:

-  Abrufen von Daten für eine von Ihnen geschriebene, benutzerdefinierte Überwachungsanwendung
-  Erstellen von benutzerdefinierten Abfragen und Senden der Daten an eine Drittanbieteranwendung

### Weiterleiten
Sie können Überwachungsdaten in Echtzeit an andere Speicherorte streamen.

Beispiele:

- Senden der Daten an Application Insights, um die dort vorhandenen Visualisierungstools zu verwenden
- Senden der Daten an Event Hubs, um sie an Drittanbietertools weiterleiten und Echtzeitanalysen durchführen zu können

### Automatisieren
Sie können die Überwachungsdaten nutzen, um Ereignisse oder sogar ganze Prozesse auszulösen. Beispiele:

- Verwenden der Daten, um Compute-Instanzen je nach Anwendungslast automatisch horizontal hoch- oder herunterzuskalieren
- Senden von E-Mails, wenn eine Metrik einen vorgegebenen Schwellenwert überschreitet
- Aufrufen einer Web-URL (Webhook), um eine Aktion in einem System außerhalb von Azure auszuführen
- Starten eines Runbooks in Azure Automation, um verschiedenste Aufgaben auszuführen

## Nutzungsmethoden
Nachverfolgung, Routing und Abfrage von Daten können ganz allgemein mit einer der folgenden Methoden angepasst werden. Manche Methoden sind unter Umständen für bestimmte Aktionen oder Datentypen nicht verfügbar.

- [Azure-Portal](https://portal.azure.com)
- [PowerShell](insights-powershell-samples.md)
- [Plattformübergreifende Befehlszeilenschnittstelle](insights-cli-samples.md)
- [REST-API](https://msdn.microsoft.com/library/dn931943.aspx)
- [.NET SDK](https://msdn.microsoft.com/library/dn802153.aspx)

## Überwachungsfunktionen von Azure
Azure bietet verschiedene Möglichkeiten zur Überwachung Ihrer Dienste – von der Bare-Metal-Infrastruktur bis hin zur Anwendungstelemetrie. Die beste Überwachungsstrategie kombiniert alle Methoden, um umfassende und detaillierte Einblicke in die Integrität Ihrer Dienste zu gewinnen.

- [Azure Insights-Überwachung (Plattform)](http://aka.ms/azmondocs): Bietet Visualisierung, Abfrage, Weiterleitung, Warnung, automatische Skalierung und Automatisierung für Daten sowohl aus der Azure-Infrastruktur (Aktivitätsprotokoll) als auch aus jeder einzelnen Azure-Ressource (Diagnoseprotokolle). Dieser Artikel ist Teil der Azure Insights-Dokumentation.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/): Bietet umfangreiche Erkennungs- und Diagnosefunktionen für Probleme auf der Anwendungsebene des Diensts sowie Integration in die Daten der Azure-Überwachung.
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/), Teil der [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite): Bietet eine vollständige IT-Verwaltungslösung sowohl für lokale als auch für cloudbasierte Infrastrukturen – einschließlich Protokollsuche und -analysen Ihrer Azure-Ressourcen.


## Nächste Schritte
Weitere Informationen

- [Azure Insights](http://aka.ms/azmondocs): Sehen Sie sich die Links im Inhaltsverzeichnis auf der linken Seite und die Videos unter diesem Link an.
- [Azure-Diagnose](../azure-diagnostics.md): Diagnostizieren Sie Probleme mit Ihren Clouddiensten, virtuellen Computern oder der Service Fabric-Anwendung.
- [Application Insights](https://azure.microsoft.com/documentation/services/application-insights/): Diagnostizieren Sie Probleme mit Ihrer App Service-Web-App.
- [Problembehandlung für Azure Storage](../storage/storage-e2e-troubleshooting.md): Lösen Sie Probleme mit Storage-Blobs, Tabellen oder Warteschlangen.
- [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics/) und die [Operations Management Suite](https://www.microsoft.com/cloud-platform/operations-management-suite)

<!---HONumber=AcomDC_0907_2016-->