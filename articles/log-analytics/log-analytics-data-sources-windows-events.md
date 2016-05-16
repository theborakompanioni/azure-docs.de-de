<properties 
   pageTitle="Windows-Ereignisprotokolle in Log Analytics | Microsoft Azure"
   description="Windows-Ereignisprotokolle gehören zu den häufigsten von Log Analytics verwendeten Datenquellen. Dieser Artikel beschreibt die Konfiguration der Sammlung von Windows-Ereignisprotokollen sowie Details zu den Datensätzen, die im OMS-Repository erstellt werden."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren" />

# Datenquellen für Windows-Ereignisprotokolle in Log Analytics

Windows-Ereignisprotokolle gehören zu den häufigsten für Windows-Agents verwendeten [Datenquellen](log-analytics-data-sources.md), da dies die Methode ist, die von den meisten Anwendungen zum Protokollieren von Informationen und Fehlern verwendet wird. Sie können Ereignisse aus Standardprotokollen wie beispielsweise dem System- und dem Anwendungsprotokoll sammeln und darüber hinaus benutzerdefinierte Protokolle angeben, die von den zu überwachenden Anwendungen erstellt werden.

![Windows-Ereignisse](media/log-analytics-data-sources-windows-events/overview.png)

## Konfigurieren der Windows-Ereignisprotokolle

Konfigurieren Sie Windows-Ereignisprotokolle über das [Menü „Daten“ in den Log Analytics-Einstellungen](log-analytics-data-sources.md/configuring-data-sources).

Log Analytics sammelt nur Ereignisse aus den Windows-Ereignisprotokollen, die in den Einstellungen angegeben wurden. Sie können ein neues Protokoll hinzufügen, indem Sie den Namen des Protokolls eingeben und auf **+** klicken. Für jedes Protokoll werden nur Ereignisse mit den ausgewählten Schweregraden gesammelt. Überprüfen Sie die Schweregrade für das Protokoll, aus dem Sie Daten sammeln möchten. Sie können keine zusätzlichen Kriterien zum Filtern von Ereignissen bereitstellen.

![Windows-Ereignisse konfigurieren](media/log-analytics-data-sources-windows-events/configure.png)


## Datensammlung

Log Analytics sammelt jedes Ereignis mit einem ausgewählten Schweregrad aus einem überwachten Ereignisprotokoll, sobald das Ereignis erstellt wird. Der Agent zeichnet seine Position in jedem Ereignisprotokoll auf, aus dem er Daten sammelt. Wenn der Agent für einen bestimmten Zeitraum offline geht, sammelt Log Analytics Ereignisse ab dem Zeitpunkt der letzten Sammlung, unabhängig davon, ob die Ereignisse erstellt wurden, während der Agent offline war.


## Eigenschaften von Windows-Ereignisdatensätzen

Windows-Ereignisdatensätze weisen den Typ **Event** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--|:--|
| Computer | Name des Computers, auf dem das Ereignis gesammelt wurde. |
| EventCategory | Kategorie des Ereignisses. |
| EventData | Alle Ereignisdaten im Rohdatenformat. |
| EventId | Nummer des Ereignisses. |
| EventLevel | Schweregrad des Ereignisses in numerischer Form. |
| EventLevelName | Schweregrad des Ereignisses in Textform. |
| EventLog | Name des Ereignisprotokolls, aus dem das Ereignis gesammelt wurde. |
| ParameterXml | Ereignisparameterwerte in XML-Format. |
| ManagementGroupName | Name der Verwaltungsgruppe für SCOM-Agents. Bei anderen Agents lautet diese „AOI-<workspace ID>“. |
| RenderedDescription | Ereignisbeschreibung mit Parameterwerten. |
| Quelle | Quelle des Ereignisses. |
| SourceSystem | Typ des Agents, auf dem das Ereignis gesammelt wurde. <br> OpsManager – Windows-Agent, entweder direkt verbunden oder über SCOM <br> Linux – Alle Linux-Agents <br> AzureStorage – Azure-Diagnose |
| TimeGenerated | Datum und Uhrzeit, zu der das Ereignis in Windows erstellt wurde. |
| UserName | Benutzername des Kontos, in dem das Ereignis protokolliert wurde. |



## Protokollsuchvorgänge mit Windows-Ereignissen

Die folgende Tabelle zeigt verschiedene Beispiele für Protokollsuchvorgänge, die Windows-Ereignisdatensätze abrufen.

| Abfrage | Beschreibung |
|:--|:--|
| Type=Event | Alle Windows-Ereignisse. |
| Type=Event EventLevelName=error | Alle Windows-Ereignisse mit dem Schweregrad „error“. |
| Type=Event | Measure count() by Source | Anzahl von Windows-Ereignissen nach Quelle. |
| Type=Event EventLevelName=error | Measure count() by Source | Anzahl von Windows-Fehlerereignissen nach Quelle. |

## Nächste Schritte

- Konfigurieren Sie Log Analytics für die Sammlung von Daten aus anderen [Datenquellen](log-analytics-data-sources.md) zur Analyse.
- Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
- Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md), um die Ereignisdatensätze in einzelnen Feldern zu analysieren.
- Konfigurieren Sie die [Sammlung von Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) aus Ihren Windows-Agents.

<!---HONumber=AcomDC_0504_2016-->