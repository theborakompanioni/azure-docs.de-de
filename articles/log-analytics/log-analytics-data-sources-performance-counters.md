---
title: Sammeln und Analysieren von Leistungsindikatoren in Azure Log Analytics | Microsoft-Dokumentation
description: "Daten aus Leistungsindikatoren werden von Log Analytics gesammelt, um die Leistung von Windows- und Linux-Agents zu analysieren.  Dieser Artikel beschreibt, wie Sie die Sammlung von Leistungsindikatoren sowohl für Windows- als auch für Linux-Agents konfigurieren, wie die Daten im OMS-Repository gespeichert werden und wie sie im OMS-Portal analysiert werden können."
services: log-analytics
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 20e145e4-2ace-4cd9-b252-71fb4f94099e
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/23/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: ce90e15108ace97d86e7180d79e38652e1be9872
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="windows-and-linux-performance-data-sources-in-log-analytics"></a>Windows- und Linux-Leistungsindikatoren in Log Analytics
Leistungsindikatoren in Windows und Linux bieten Einblick in die Leistung von Hardwarekomponenten, Betriebssystemen und Anwendungen.  Log Analytics kann in sehr kurzen Intervallen Leistungsindikatoren abrufen, um Analysen nahezu in Echtzeit zu ermöglichen. Darüber hinaus kann Log Analytics Leistungsdaten zusammenstellen, um längerfristige Analysen und Berichte zu ermöglichen.

![Leistungsindikatoren](media/log-analytics-data-sources-performance-counters/overview.png)

## <a name="configuring-performance-counters"></a>Konfigurieren von Leistungsindikatoren
Sie konfigurieren Leistungsindikatoren im OMS-Portal über das [Menü „Daten“ in den Log Analytics-Einstellungen](log-analytics-data-sources.md#configuring-data-sources).

Wenn Sie die Windows- oder Linux-Leistungsindikatoren zum ersten Mal für einen neuen OMS-Arbeitsbereich konfigurieren, haben Sie die Möglichkeit, schnell mehrere allgemeine Indikatoren zu erstellen.  Diese werden in einer Liste aufgeführt, und neben jedem Indikator finden Sie ein Kontrollkästchen.  Stellen Sie sicher, dass alle Leistungsindikatoren aktiviert sind, die Sie anfänglich erstellen möchten, und klicken Sie dann auf **Ausgewählte Leistungsindikatoren hinzufügen**.

Sie können für Windows-Leistungsindikatoren eine bestimmte Instanz für jeden Leistungsindikator auswählen. Bei Linux-Leistungsindikatoren gilt, dass die Instanz eines Indikators, die Sie wählen, für alle untergeordneten Indikatoren des übergeordneten Indikators gilt. Die folgende Tabelle zeigt die allgemeinen Instanzen, die jeweils für die Linux- und Windows-Leistungsindikatoren verfügbar sind.

| Instanzname | Beschreibung |
| --- | --- |
| \_Gesamt |Gesamtsumme aller Instanzen |
| \* |Alle Instanzen |
| (/&#124;/var) |Stimmt mit den Instanzen mit den Namen „/“ oder „/var“ überein. |

### <a name="windows-performance-counters"></a>Windows-Leistungsindikatoren

![Windows-Leistungsindikatoren konfigurieren](media/log-analytics-data-sources-performance-counters/configure-windows.png)

Gehen Sie folgendermaßen vor, um einen neuen Windows-Leistungsindikator hinzuzufügen, aus dem Daten gesammelt werden sollen.

1. Geben Sie den Namen des Leistungsindikators im Format *Objekt(Instanz)\Indikator* in das Textfeld ein.  Wenn Sie mit der Eingabe beginnen, wird Ihnen eine Liste mit passenden allgemeinen Indikatoren angezeigt.  Sie können einen Indikator aus der Liste auswählen oder selbst einen eingeben.  Sie können auch durch die Angabe von *Objekt\Indikator* alle Instanzen eines bestimmten Leistungsindikators zurückgeben.
2. Klicken Sie auf **+**, oder drücken Sie die EINGABETASTE****, um der Liste den Indikator hinzuzufügen.
3. Wenn Sie einen Leistungsindikator hinzufügen, verwendet dieser den Standardwert von 10 Sekunden für das **Stichprobenintervall**.  Sie können diesen Standardwert auf einen höheren Wert von bis zu 1800 Sekunden (30 Minuten) festlegen, wenn Sie die Speicheranforderungen der gesammelten Leistungsdaten reduzieren möchten.
4. Wenn Sie mit dem Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie auf die Schaltfläche **Speichern** am oberen Bildschirmrand, um die Konfiguration zu speichern.

### <a name="linux-performance-counters"></a>Leistungsindikatoren von Linux

![Linux-Leistungsindikatoren konfigurieren](media/log-analytics-data-sources-performance-counters/configure-linux.png)

Gehen Sie folgendermaßen vor, um einen neuen Linus-Leistungsindikator hinzuzufügen, aus dem Daten gesammelt werden sollen.

1. Standardmäßig werden alle Konfigurationsänderungen automatisch per Push an alle Agents weitergegeben.  Bei Linux-Agents wird eine Konfigurationsdatei an den Fluentd-Datensammler gesendet.  Wenn Sie diese Datei manuell auf jedem Linux-Agenten ändern möchten, deaktivieren Sie das Kontrollkästchen *Nachstehende Konfiguration auf meine Linux-Computer anwenden*, und führen Sie die folgenden Anleitungen aus.
2. Geben Sie den Namen des Leistungsindikators im Format *Objekt(Instanz)\Indikator* in das Textfeld ein.  Wenn Sie mit der Eingabe beginnen, wird Ihnen eine Liste mit passenden allgemeinen Indikatoren angezeigt.  Sie können einen Indikator aus der Liste auswählen oder selbst einen eingeben.  
3. Klicken Sie auf **+** oder drücken Sie die EINGABETASTE****, um den Indikator der Liste der anderen Leistungsindikatoren für das Objekt hinzuzufügen.
4. Alle Leistungsindikatoren für ein Objekt verwenden das gleiche **Stichprobenintervall**.  Der Standardwert ist 10 Sekunden.  Sie können einen höheren Wert von bis zu 1800 Sekunden (30 Minuten) festlegen, wenn Sie die Speicheranforderungen der gesammelten Leistungsdaten reduzieren möchten.
5. Wenn Sie mit dem Hinzufügen von Leistungsindikatoren fertig sind, klicken Sie auf die Schaltfläche **Speichern** am oberen Bildschirmrand, um die Konfiguration zu speichern.

#### <a name="configure-linux-performance-counters-in-configuration-file"></a>Konfigurieren von Linux-Leistungsindikatoren in der Konfigurationsdatei
Sie müssen die Linux-Leistungsindikatoren nicht mithilfe des OMS-Portals konfigurieren, sondern können die Konfigurationsdateien auch auf dem Linux-Agenten bearbeiten.  Die gesammelten Leistungsmetriken, werden durch die Konfiguration in **/etc/opt/microsoft/omsagent/\<Arbeitsbereichs-ID\>/conf/omsagent.conf** gesteuert. 

Jedes Objekt oder jede Kategorie von Leistungsindikatoren, die gesammelt werden sollen, sollten in der Konfigurationsdatei als einzelnes `<source>` -Element definiert sein. Die Syntax folgt dem unten angegebenen Muster.

    <source>
      type oms_omi  
      object_name "Processor"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>


Die in diesem Element verwendeten Parameter werden in der folgenden Tabelle beschrieben.

| Parameter | Beschreibung |
|:--|:--|
| object\_name | Der Objektname für die Sammlung. |
| instance\_regex |  Ein *regulärer Ausdruck*, der definiert, welche Instanzen gesammelt werden sollen. Der Wert `.*` gibt alle Instanzen an. Sie können `_Total` angeben, um die Prozessormetriken nur für die Instanz „\_Total“ zu sammeln. Um die Prozessmetriken nur für die crond- und sshd-Instanzen zu sammeln, können Sie `(crond\|sshd)` angeben. |
| counter\_name\_regex | Ein *regulärer Ausdruck* für die zu sammelnden Leistungsindikatoren (für das Objekt). Geben Sie `.*`an, um alle Indikatoren für das Objekt zu sammeln. Um nur Indikatoren für Speicherauslagerungsbereiche zu sammeln, können Sie beispielsweise `.+Swap.+` angeben |
| interval | Häufigkeit, mit der die Indikatoren des Objekts gesammelt werden. |


Die folgende Tabelle enthält die Objekte und Leistungsindikatoren, die Sie in der Konfigurationsdatei angeben können.  Für bestimmte Anwendungen stehen weitere Indikatoren zur Verfügung. Siehe hierzu [Sammeln von Leistungsindikatoren für Linux-Anwendungen in Log Analytics](log-analytics-data-sources-linux-applications.md). 

| Objektname | Name des Leistungsindikators |
|:--|:--|
| Logischer Datenträger | % freie Inodes |
| Logischer Datenträger | % freier Speicher |
| Logischer Datenträger | % verwendete Inodes |
| Logischer Datenträger | % verwendeter Speicher |
| Logischer Datenträger | Byte gelesen/s  |
| Logischer Datenträger | Lesevorgänge/s  |
| Logischer Datenträger | Übertragungen/s |
| Logischer Datenträger | Byte geschrieben/s |
| Logischer Datenträger | Schreibvorgänge/s |
| Logischer Datenträger | Freie Megabytes |
| Logischer Datenträger | Logischer Datenträger Bytes/s |
| Arbeitsspeicher | % verfügbarer Speicher |
| Arbeitsspeicher | % verfügbarer Auslagerungsbereich |
| Arbeitsspeicher | % verwendeter Arbeitsspeicher |
| Arbeitsspeicher | % verwendeter Auslagerungsbereich |
| Arbeitsspeicher | Verfügbarer Speicher in MB |
| Arbeitsspeicher | Verfügbarer Auslagerungsbereich in MB |
| Arbeitsspeicher | Seitenlesevorgänge/s |
| Arbeitsspeicher | Schreibvorgänge/s |
| Arbeitsspeicher | Seiten/s |
| Arbeitsspeicher | Verwendeter Auslagerungsbereich in MB |
| Arbeitsspeicher | Verwendeter Speicher in MB |
| Netzwerk | Summe übertragene Bytes |
| Netzwerk | Summe empfangene Bytes |
| Netzwerk | Summe Bytes |
| Netzwerk | Summe übermittelte Pakete |
| Netzwerk | Summe empfangene Pakete |
| Netzwerk | Summe Rx-Fehler |
| Netzwerk | Summe Tx-Fehler |
| Netzwerk | Summe Konflikte |
| Physikalischer Datenträger | Durchschn. Datenträger s/gelesen |
| Physikalischer Datenträger | Durchschn. Datenträger s/übertragen |
| Physikalischer Datenträger | Durchschn. Datenträger s/geschrieben |
| Physikalischer Datenträger | Physischer Datenträger Bytes/s |
| Prozess | PCT-privilegierte Zeit |
| Prozess | PCT-Benutzerzeit |
| Prozess | Verwendeter Arbeitsspeicher in KB |
| Prozess | Virtueller gemeinsamer Speicher |
| Prozessor | % DPC-Zeit |
| Prozessor | % Leerlaufzeit |
| Prozessor | % Interruptzeit |
| Prozessor | % E/a-Wartezeit |
| Prozessor | % Nice-Zeit |
| Prozessor | % privilegierte Zeit |
| Prozessor | % Prozessorzeit |
| Prozessor | % Benutzerzeit |
| System | Freier physischer Speicher |
| System | Freier Speicherplatz in Auslagerungsdateien |
| System | Freier virtueller Arbeitsspeicher |
| System | Prozesse |
| System | Genutzter Speicherplatz in Auslagerungsdateien |
| System | Betriebszeit |
| System | Benutzer |


Im Folgenden wird die Standardkonfiguration für Leistungsmetriken beschrieben.

    <source>
      type oms_omi
      object_name "Physical Disk"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 5m
    </source>
    
    <source>
      type oms_omi
      object_name "Logical Disk"
      instance_regex ".*
      counter_name_regex ".*"
      interval 5m
    </source>
    
    <source>
      type oms_omi
      object_name "Processor"
      instance_regex ".*
      counter_name_regex ".*"
      interval 30s
    </source>
    
    <source>
      type oms_omi
      object_name "Memory"
      instance_regex ".*"
      counter_name_regex ".*"
      interval 30s
    </source>

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
| SourceSystem |Typ des Agents, auf dem die Daten gesammelt wurden. <br><br>OpsManager: Windows-Agent (Direktverbindung oder SCOM) <br> Linux: Alle Linux-Agents  <br> AzureStorage – Azure-Diagnose |
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
Wenn Sie eine Protokollsuche nach Leistungsdaten durchführen, wird standardmäßig die **Listenansicht** angezeigt.  Um die Daten in grafischer Form anzuzeigen, klicken Sie auf **Metriken**.  Für eine detaillierte grafische Darstellung klicken Sie auf **+** neben dem gewünschten Leistungsindikator.  

![Metrikenansicht, reduziert](media/log-analytics-data-sources-performance-counters/metricscollapsed.png)

Informationen zum Aggregieren von Leistungsdaten in einer Protokollsuche finden Sie unter [On-demand metric aggregation and visualization in OMS](http://blogs.technet.microsoft.com/msoms/2016/02/26/on-demand-metric-aggregation-and-visualization-in-oms/) (On-Demand-Aggregation und -Visualisierung von Metriken in OMS).


## <a name="next-steps"></a>Nächste Schritte
* [Sammeln von Leistungsindikatoren aus Linux-Anwendungen wie](log-analytics-data-sources-linux-applications.md) MySQL und Apache HTTP Server.
* Erfahren Sie mehr über [Protokollsuchvorgänge](log-analytics-log-searches.md) zum Analysieren der aus Datenquellen und Lösungen gesammelten Daten.  
* Exportieren Sie gesammelte Daten nach [Power BI](log-analytics-powerbi.md) , um weitere Möglichkeiten der Visualisierung und Analyse zu nutzen.
