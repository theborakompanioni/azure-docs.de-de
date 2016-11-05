---
title: Windows- und Linux-Leistungsindikatoren in Log Analytics | Microsoft Docs
description: Daten aus Leistungsindikatoren werden von Log Analytics gesammelt, um die Leistung von Windows- und Linux-Agents zu analysieren.  Dieser Artikel beschreibt, wie Sie die Sammlung von Leistungsindikatoren sowohl für Windows- als auch für Linux-Agents konfigurieren, wie die Daten im OMS-Repository gespeichert werden und wie sie im OMS-Portal analysiert werden können.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/27/2016
ms.author: bwren

---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows- und Linux-Leistungsindikatoren in Log Analytics
Leistungsindikatoren in Windows und Linux bieten Einblick in die Leistung von Hardwarekomponenten, Betriebssystemen und Anwendungen.  Log Analytics kann in sehr kurzen Intervallen Leistungsindikatoren abrufen, um Analysen nahezu in Echtzeit zu ermöglichen. Darüber hinaus kann Log Analytics Leistungsdaten zusammenstellen, um längerfristige Analysen und Berichte zu ermöglichen.

![Leistungsindikatoren](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurieren von Leistungsindikatoren
Sie konfigurieren Leistungsindikatoren über das [Menü „Daten“ in den Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Wenn Sie die Windows- oder Linux-Leistungsindikatoren zum ersten Mal für einen neuen OMS-Arbeitsbereich konfigurieren, haben Sie die Möglichkeit, schnell mehrere allgemeine Indikatoren zu erstellen.  Diese werden in einer Liste aufgeführt, und neben jedem Indikator finden Sie ein Kontrollkästchen.  Stellen Sie sicher, dass alle Leistungsindikatoren aktiviert sind, die Sie anfänglich erstellen möchten, und klicken Sie dann auf **Ausgewählte Leistungsindikatoren hinzufügen**.

![Windows-Leistungsindikatoren konfigurieren](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Gehen Sie folgendermaßen vor, um einen neuen Windows-Leistungsindikator hinzuzufügen, aus dem Daten gesammelt werden sollen.

1. Geben Sie den Namen des Leistungsindikators im Format *Objekt(Instanz)\Indikator* in das Textfeld ein.  Wenn Sie mit der Eingabe beginnen, wird Ihnen eine Liste mit passenden allgemeinen Indikatoren angezeigt.  Sie können einen Indikator aus der Liste auswählen oder selbst einen eingeben.  Sie können auch durch die Angabe von *Objekt\Indikator* alle Instanzen eines bestimmten Leistungsindikators zurückgeben. 
2. Klicken Sie auf **+**, oder drücken Sie die EINGABETASTE****, um der Liste den Indikator hinzuzufügen.
3. Wenn Sie einen Leistungsindikator hinzufügen, verwendet dieser den Standardwert von 10 Sekunden für das **Stichprobenintervall**.  Sie können diesen Standardwert auf einen höheren Wert von bis zu 1800 Sekunden (30 Minuten) festlegen, wenn Sie die Speicheranforderungen der gesammelten Leistungsdaten reduzieren möchten.
4. Wenn Sie mit dem Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie auf die Schaltfläche **Speichern** am oberen Bildschirmrand, um die Konfiguration zu speichern.

![Linux-Leistungsindikatoren konfigurieren](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Gehen Sie folgendermaßen vor, um einen neuen Linus-Leistungsindikator hinzuzufügen, aus dem Daten gesammelt werden sollen.

1. Standardmäßig werden alle Konfigurationsänderungen automatisch per Push an alle Agents weitergegeben.  Bei Linux-Agents wird eine Konfigurationsdatei an den Fluentd-Datensammler gesendet.  Wenn Sie diese Datei manuell auf jedem Linux-Agent ändern möchten, deaktivieren Sie das Kontrollkästchen *Nachstehende Konfiguration auf meine Linux-Computer anwenden*.
2. Geben Sie den Namen des Leistungsindikators im Format *Objekt(Instanz)\Indikator* in das Textfeld ein.  Wenn Sie mit der Eingabe beginnen, wird Ihnen eine Liste mit passenden allgemeinen Indikatoren angezeigt.  Sie können einen Indikator aus der Liste auswählen oder selbst einen eingeben.  
3. Klicken Sie auf **+** oder drücken Sie die EINGABETASTE****, um den Indikator der Liste der anderen Leistungsindikatoren für das Objekt hinzuzufügen.
4. Alle Leistungsindikatoren für ein Objekt verwenden das gleiche **Stichprobenintervall**.  Der Standardwert ist 10 Sekunden.  Sie können einen höheren Wert von bis zu 1800 Sekunden (30 Minuten) festlegen, wenn Sie die Speicheranforderungen der gesammelten Leistungsdaten reduzieren möchten.
5. Wenn Sie mit dem Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie auf die Schaltfläche **Speichern** am oberen Bildschirmrand, um die Konfiguration zu speichern.

## <a name="data-collection"></a>Datensammlung
Log Analytics sammelt die Daten aller angegebenen Leistungsindikatoren im angegebenen Stichprobenintervall auf allen Agents, auf denen diese Indikatoren installiert sind.  Die Daten werden nicht aggregiert, sondern die Rohdaten stehen während des durch Ihr OMS-Abonnement festgelegten Zeitraums in allen Protokollsuchansichten als unformatierte Daten zur Verfügung.

## <a name="performance-record-properties"></a>Eigenschaften von Leistungsdatensätzen
Leistungsdatensätze weisen den Typ **Perf** auf und besitzen die in der folgenden Tabelle aufgeführten Eigenschaften.

| Eigenschaft | Beschreibung |
|:--- |:--- |
| Computer |Computer, auf dem das Ereignis gesammelt wurde. |
| CounterName |Name des Leistungsindikators. |
| CounterPath |Vollständiger Pfad des Leistungsindikators im Format \\\\\<Computer\\Objekt(Instanz)\\Indikator |
| CounterValue |Numerischer Wert des Leistungsindikators |
| InstanceName |Name der Ereignisinstanz.  Leer, wenn keine Instanz vorhanden ist. |
| ObjectName |Name des Leistungsobjekts. |
| SourceSystem |Typ des Agents, auf dem die Daten gesammelt wurden. <br> OpsManager: Windows-Agent (Direktverbindung oder SCOM) <br>  Linux: Alle Linux-Agents  <br>  AzureStorage – Azure-Diagnose |
| TimeGenerated |Datum und Uhrzeit der Datenstichprobe. |

## <a name="sizing-estimates"></a>Größeneinschätzung
 Bei der Sammlung der Daten von einem bestimmten Leistungsindikator mit einem Intervall von 10 Sekunden können Sie von etwa 1 MB pro Tag und Instanz ausgehen.  Sie können die Speicheranforderungen eines bestimmten Leistungsindikators anhand der folgenden Formel einschätzen.

    1 MB x (number of counters) x (number of agents) x (number of instances)

## <a name="log-searches-with-performance-records"></a>Protokollsuchvorgänge mit Leistungsdatensätzen
Die folgende Tabelle zeigt verschiedene Beispiele für Protokollsuchvorgänge, die Leistungsdatensätze abrufen.

| Abfrage | Beschreibung |
|:--- |:--- |
| Type=Perf |Alle Leistungsdaten. |
| Type=Perf Computer="MyComputer" |Alle Leistungsdaten eines bestimmten Computers. |
| Type=Perf CounterName="Aktuelle Warteschlangenlänge" |Alle Leistungsdaten eines bestimmten Leistungsindikators. |
| Type=Perf (ObjectName=Processor) CounterName="% Prozessorzeit" InstanceName=_Total &#124; measure Avg(Average) as AVGCPU by Computer |Durchschnittliche CPU-Nutzung aller Computer. |
| Type=Perf (CounterName="% Prozessorzeit") &#124; measure max(Max) by Computer |Maximale CPU-Nutzung aller Computer. |
| Type=Perf ObjectName=LogicalDisk CounterName="Aktuelle Warteschlangenlänge" Computer="MyComputerName" &#124; measure Avg(Average) by InstanceName |Durchschnittliche aktuelle Länge der Datenträgerwarteschlangen aller Instanzen eines bestimmten Computers |
| Type=Perf CounterName="Übertragungen/s" &#124; measure percentile95(Average) by Computer |95. Perzentil der Datenträgerübertragungen pro Sekunde auf allen Computern. |
| Type=Perf CounterName="% Prozessorzeit" InstanceName="_Total" &#124; measure avg(CounterValue) by Computer Interval 1HOUR |Durchschnittliche CPU-Nutzung pro Stunde auf allen Computern |
| Type=Perf Computer="MyComputer" CounterName=%* InstanceName=_Total &#124; measure percentile70(CounterValue) by CounterName Interval 1HOUR |70. Perzentil pro Stunde jedes prozentualen Indikators für einen bestimmten Computer |
| Type=Perf CounterName="% Prozessorzeit" InstanceName="_Total"  (Computer="MyComputer") &#124; measure min(CounterValue), avg(CounterValue), percentile75(CounterValue), max(CounterValue) by Computer Interval 1HOUR |Durchschnittliche, minimale, maximale und 75.-Perzentil-CPU-Nutzung pro Stunde für einen bestimmten Computer |

## <a name="viewing-performance-data"></a>Anzeigen von Leistungsdaten
Wenn Sie eine Protokollsuche nach Leistungsdaten durchführen, wird standardmäßig die Ansicht **Protokoll** angezeigt.  Um die Daten in grafischer Form anzuzeigen, klicken Sie auf **Metriken**.  Für eine detaillierte grafische Darstellung klicken Sie auf **+** neben dem gewünschten Leistungsindikator.  

![Metrikenansicht, reduziert](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Wenn die ausgewählte Zeitspanne 6 Stunden oder weniger beträgt, wird das Diagramm alle paar Sekunden aktualisiert.  Die Livedaten werden auf der rechten Seite des Diagramms in Hellblau angezeigt.

![Metrikenansicht, erweitert mit Livedaten](media/log-analytics-data-sources-performance-counters/metricsexpanded.png)

Informationen zum Aggregieren von Leistungsdaten in einer Protokollsuche finden Sie unter [On-demand metric aggregation and visualization in OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/) (On-Demand-Aggregation und -Visualisierung von Metriken in OMS).

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
* Exportieren Sie gesammelte Daten nach [Power BI](log-analytics-powerbi.md) , um weitere Möglichkeiten der Visualisierung und Analyse zu nutzen.

<!--HONumber=Oct16_HO2-->


