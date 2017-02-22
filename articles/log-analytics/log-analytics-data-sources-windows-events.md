---
title: Erfassen und Analysieren von Windows-Ereignisprotokollen in OMS Log Analytics | Microsoft-Dokumentation
description: "Windows-Ereignisprotokolle gehören zu den häufigsten von Log Analytics verwendeten Datenquellen.  Dieser Artikel beschreibt die Konfiguration der Sammlung von Windows-Ereignisprotokollen sowie Details zu den Datensätzen, die im OMS-Repository erstellt werden."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.assetid: ee52f564-995b-450f-a6ba-0d7b1dac3f32
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: bwren
translationtype: Human Translation
ms.sourcegitcommit: 653696779e612726ed5b75829a5c6ed2615553d7
ms.openlocfilehash: b6627ed7e3b08e0a94dec229d735114b3ed1b9df


---
# <a name="windows-event-log-data-sources-in-log-analytics"></a>Datenquellen für Windows-Ereignisprotokolle in Log Analytics
Windows-Ereignisprotokolle sind eine der häufigsten [Datenquellen](log-analytics-data-sources.md) zum Sammeln von Daten mithilfe von Windows-Agents, da viele Anwendungen Daten in das Windows-Ereignisprotokoll schreiben.  Sie können Ereignisse aus Standardprotokollen wie beispielsweise dem System- und dem Anwendungsprotokoll sammeln und darüber hinaus benutzerdefinierte Protokolle angeben, die von den zu überwachenden Anwendungen erstellt werden.

![Windows-Ereignisse](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigurieren der Windows-Ereignisprotokolle
Sie konfigurieren Windows-Ereignisprotokolle über das [Menü „Daten“ in den Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Log Analytics sammelt nur Ereignisse aus den Windows-Ereignisprotokollen, die in den Einstellungen angegeben wurden.  Sie können ein Ereignisprotokoll hinzufügen, indem Sie den Namen des Protokolls eingeben und auf **+** klicken.  Für jedes Protokoll werden nur die Ereignisse mit den ausgewählten Schweregraden gesammelt.  Überprüfen Sie die Schweregrade für das Protokoll, aus dem Sie Daten sammeln möchten.  Sie können keine zusätzlichen Kriterien zum Filtern von Ereignissen bereitstellen.

Während der Eingabe des Namens des Ereignisprotokolls bietet Log Analytics Vorschläge gängiger Ereignisprotokollnamen an. Wenn das Protokoll, das Sie hinzufügen möchten, nicht in der Liste enthalten ist, können Sie es dennoch durch Eingabe des vollständigen Namens des Protokolls hinzufügen. Mithilfe der Ereignisanzeige können Sie den vollständigen Namen des Protokolls finden. Öffnen Sie in der Ereignisanzeige die Seite *Eigenschaften* des Protokolls, und kopieren Sie die Zeichenfolge aus dem Feld *Vollständiger Name*.

![Windows-Ereignisse konfigurieren](media/log-analytics-data-sources-windows-events/configure.png)

## <a name="data-collection"></a>Datensammlung
Log Analytics sammelt jedes Ereignis mit einem ausgewählten Schweregrad aus einem überwachten Ereignisprotokoll, sobald das Ereignis erstellt wird.  Der Agent zeichnet seine Position in jedem Ereignisprotokoll auf, aus dem er Daten sammelt.  Wenn der Agent für einen bestimmten Zeitraum offline geht, sammelt Log Analytics Ereignisse ab dem Zeitpunkt der letzten Sammlung, unabhängig davon, ob die Ereignisse erstellt wurden, während der Agent offline war.  Es kann vorkommen, dass diese Ereignisse nicht erfasst werden, falls das Ereignisprotokoll umgebrochen wird und nicht erfasste Ereignisse überschrieben werden, während der Agent offline ist.



## <a name="windows-event-records-properties"></a>Eigenschaften von Windows-Ereignisdatensätzen
Windows-Ereignisdatensätze weisen den Typ **Event** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften:

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Computer |Name des Computers, auf dem das Ereignis gesammelt wurde. |
| EventCategory |Kategorie des Ereignisses. |
| EventData |Alle Ereignisdaten im Rohdatenformat. |
| EventId |Nummer des Ereignisses. |
| EventLevel |Schweregrad des Ereignisses in numerischer Form. |
| EventLevelName |Schweregrad des Ereignisses in Textform. |
| EventLog |Name des Ereignisprotokolls, aus dem das Ereignis gesammelt wurde. |
| ParameterXml |Ereignisparameterwerte in XML-Format. |
| ManagementGroupName |Name der Verwaltungsgruppe für System Center Operations Manager-Agents.  Bei anderen Agents lautet dieser Wert „AOI-<workspace ID>“. |
| RenderedDescription |Ereignisbeschreibung mit Parameterwerten. |
| Quelle |Quelle des Ereignisses. |
| SourceSystem |Typ des Agents, auf dem das Ereignis gesammelt wurde. <br> OpsManager: Windows-Agent (entweder Direktverbindung oder von Operations Manager verwaltet) <br> Linux: Alle Linux-Agents  <br> AzureStorage – Azure-Diagnose |
| TimeGenerated |Datum und Uhrzeit, zu der das Ereignis in Windows erstellt wurde. |
| UserName |Benutzername des Kontos, in dem das Ereignis protokolliert wurde. |

## <a name="log-searches-with-windows-events"></a>Protokollsuchvorgänge mit Windows-Ereignissen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollsuchvorgänge, die Windows-Ereignisdatensätze abrufen.

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=Event |Alle Windows-Ereignisse. |
| Type=Event EventLevelName=error |Alle Windows-Ereignisse mit dem Schweregrad „error“. |
| Type=Event &#124; Measure count() by Source |Anzahl von Windows-Ereignissen nach Quelle. |
| Type=Event EventLevelName=error &#124; Measure count() by Source |Anzahl von Windows-Fehlerereignissen nach Quelle. |

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren Sie Log Analytics für die Sammlung von Daten aus anderen [Datenquellen](log-analytics-data-sources.md) zur Analyse.
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
* Verwenden Sie [benutzerdefinierte Felder](log-analytics-custom-fields.md) , um die Ereignisdatensätze in einzelnen Feldern zu analysieren.
* Konfigurieren Sie die [Sammlung von Leistungsindikatoren](log-analytics-data-sources-performance-counters.md) aus Ihren Windows-Agents.




<!--HONumber=Jan17_HO4-->


