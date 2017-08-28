---
title: Verwenden von PerfInsights in Microsoft Azure | Microsoft-Dokumentation
description: Lernen Sie, wie Sie Leistungsprobleme bei virtuellen Windows-Computern mit PerfInsights behandeln.
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: genli
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: f22bd42302b96118dba0d4e5e387c6798a0b8777
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="how-to-use-perfinsights"></a>Verwenden von PerfInsights 

[PerfInsights](http://aka.ms/perfinsightsdownload) ist ein automatisiertes Skript, das hilfreiche Diagnoseinformationen erfasst, E/A-Belastungen ausführt und einen Analysebericht zur Problembehandlung bei Windows-VM-Leistungsproblemen in Microsoft Azure bietet. 

Sie sollten dieses Skript ausführen, bevor Sie ein Support-Ticket für VM-Leistungsprobleme bei Microsoft öffnen.

## <a name="supported-troubleshooting-scenarios"></a>Unterstützte Problembehandlungsszenarien

PerfInsights kann verschiedene Arten von Informationen erfassen und analysieren, die in besonderen Szenarien gruppiert sind.

### <a name="collect-disk-configuration"></a>Erfassen der Datenträgerkonfiguration 

Dieses Szenario erfasst die Datenträgerkonfiguration und andere wichtige Informationen einschließlich folgender Elemente:

-   Ereignisprotokolle

-   Netzwerkstatus für alle ein- und ausgehenden Verbindungen

-   Netzwerk- und Firewallkonfigurationseinstellungen

-   Aufgabenliste für alle derzeit auf dem System ausgeführten Anwendungen

-   Von msinfo32 für den virtuellen Computer (VM, Virtual Machine) erstellte Informationsdatei

-   Microsoft SQL Server-Datenbank-Konfigurationseinstellungen (wenn der virtuelle Computer als Server identifiziert wird, auf dem SQL Server ausgeführt wird)

-   Speicherzuverlässigkeitszähler

-   Wichtige Windows-Hotfixes

-   Installierte Filtertreiber

Dies ist eine passive Sammlung von Informationen, die das System nicht beeinträchtigen sollte. 

>[!Note]
>Dieses Szenario ist automatisch in jedem der folgenden Szenarien enthalten.

### <a name="benchmarkstorage-performance-test"></a>Vergleichs-/Speicherleistungstest

Dieses Szenario führt den [DiskSpd](https://github.com/Microsoft/diskspd)-Vergleichstest (IOPS und MBit/s) für alle Laufwerke aus, die dem virtuellen Computer angefügt sind. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Live-Produktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, könnte die Leistung Ihres virtuellen Computers beeinträchtigen.
>

### <a name="general-vm-slow-analysis"></a>Allgemeine VM Slow-Analyse 

Dieses Szenario führt eine [Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx)-Ablaufverfolgung mithilfe der Indikatoren aus, die in der Datei „Generalcounters.txt“ angegeben werden. Wenn der virtuelle Computer als Server identifiziert wird, auf dem SQL Server ausgeführt wird, wird eine Leistungsindikator-Ablaufverfolgung mithilfe der in der Datei „Sqlcounters.txt“ gefundenen Indikatoren ausgeführt. Sie beinhaltet auch Leistungsdiagnosedaten.

### <a name="vm-slow-analysis-and-benchmark"></a>VM Slow-Analyse und Benchmark

Dieses Szenario führt eine [Leistungsindikator](https://msdn.microsoft.com/library/windows/desktop/aa373083(v=vs.85).aspx)-Ablaufverfolgung aus, der ein [DiskSpd](https://github.com/Microsoft/diskspd)-Vergleichstest folgt. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Live-Produktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, könnte die Leistung Ihres virtuellen Computers beeinträchtigen.
>

### <a name="azure-files-analysis"></a>Azure Files-Analyse 

Dieses Szenario führt eine besondere Leistungsindikatorerfassung zusammen mit einer Ablaufverfolgung im Netzwerk durch. Die Erfassung enthält alle „SMB-Client-Freigaben“-Indikatoren. Es folgen einige der wichtigsten SMB-Client-Freigaben-Leistungsindikatoren, die Teil der Erfassung sind:

| **Typ**     | **SMB-Client-Freigaben-Indikator** |
|--------------|-------------------------------|
| IOPS         | Datenanforderungen/s             |
|              | Leseanforderungen/s             |
|              | Schreibanforderungen/s            |
| Latenz      | Durchschn. s/Datenanforderung         |
|              | Durchschn. s/Lesen                 |
|              | Durchschn. s/Schreiben                |
| E/A-Größe      | Durchschn. Bytes/Datenanforderung       |
|              | Durchschn. Bytes/Lesen               |
|              | Durchschn. Bytes/Schreiben              |
| Durchsatz   | Datenbytes/s                |
|              | Lesebytes/s                |
|              | Schreibbytes/s               |
| Warteschlangenlänge | Durchschn. Lesewarteschlangen-Länge        |
|              | Durchschn. Schreibwarteschlangen-Länge       |
|              | Durchschn. Datenwarteschlangen-Länge        |

### <a name="custom-configuration"></a>Benutzerdefinierte Konfiguration 

Wenn Sie eine benutzerdefinierte Konfiguration ausführen, führen Sie alle Ablaufverfolgungen (Leistungsdiagnose, Leistungsindikator, Xperf, Netzwerk, StorPort) parallel aus, je nachdem, wie viele verschiedene Ablaufverfolgungen ausgewählt sind. Nach Abschluss der Ablaufverfolgung führt das Tool den DiskSpd-Vergleichstest aus, wenn diese Option ausgewählt ist. 

> [!Note]
> Dieses Szenario kann sich auf das System auswirken und sollte nicht auf einem Live-Produktionssystem ausgeführt werden. Falls erforderlich, führen Sie dieses Szenario in einem dedizierten Wartungsfenster aus, um Probleme zu vermeiden. Eine erhöhte Workload, die durch eine Ablaufverfolgung oder einen Vergleichstest verursacht wird, könnte die Leistung Ihres virtuellen Computers beeinträchtigen.
>

## <a name="what-kind-of-information-is-collected-by-the-script"></a>Welche Informationen werden vom Skript gesammelt?

Informationen zu Windows-VM, Datenträger- oder Speicherpoolskonfiguration, Leistungsindikatoren, Protokollen und verschiedenen Ablaufverfolgungen werden je nach verwendetem Leistungsszenario erfasst:

|Gesammelte Daten                              |  |  | Leistungsszenarien |  |  | |
|----------------------------------|----------------------------|------------------------------------|--------------------------|--------------------------------|----------------------|----------------------|
|                              | Erfassen der Datenträgerkonfiguration | Vergleichs-/Speicherleistungstest | Allgemeine VM Slow-Analyse | VM Slow-Analyse und Benchmark | Azure Files-Analyse | Benutzerdefinierte Konfiguration |
| Informationen aus Ereignisprotokollen      | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Systeminformationen               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Volumezuordnung                       | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Datenträgerzuordnung                         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Ausgeführte Aufgaben                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Speicherzuverlässigkeitszähler     | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Speicherinformationen              | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| FSUTIL-Ausgabe                    | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Filtertreiberinformationen               | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Netstat-Ausgabe                   | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Network Configuration            | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Firewall-Konfiguration           | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| SQL Server-Konfiguration         | Ja                        | Ja                                | Ja                      | Ja                            | Ja                  | Ja                  |
| Leistungsdiagnose-Ablaufverfolgungen * |                            |                                    | Ja                      |                                |                      | Ja                  |
| Leistungsindikator-Ablaufverfolgung **     |                            |                                    |                          |                                |                      | Ja                  |
| SMB-Indikatorablaufverfolgung **             |                            |                                    |                          |                                | Ja                  |                      |
| SQL Server-Indikatorablaufverfolgung **      |                            |                                    |                          |                                |                      | Ja                  |
| XPerf-Ablaufverfolgung                      |                            |                                    |                          |                                |                      | Ja                  |
| StorPort-Ablaufverfolgung                   |                            |                                    |                          |                                |                      | Ja                  |
| Netzwerkablaufverfolgung                    |                            |                                    |                          |                                | Ja                  | Ja                  |
| DiskSpd-Vergleichstest-Ablaufverfolgung ***      |                            | Ja                                |                          | Ja                            |                      |                      |
|       |                            |                         |                                                   |                      |                      |

### <a name="performance-diagnostics-trace-"></a>Leistungsdiagnose-Ablaufverfolgung (*)

Führt ein regelbasiertes Modul im Hintergrund aus, um Daten zu sammeln und akute Leistungsprobleme zu diagnostizieren. Die folgenden Regeln werden derzeit unterstützt:

- HighCpuUsage-Regel: Erkennt Perioden mit hoher CPU-Auslastung und zeigt die stärksten CPU-Ressourcenverbraucher während dieser Periode an.
- HighDiskUsage-Regel: Erkennt Perioden mit hoher Datenträgerauslastung auf physischen Datenträgern und zeigt die stärksten Datenträger-Ressourcenverbraucher während dieser Periode an.
- HighResolutionDiskMetric-Regel: Zeigt IOPS, Durchsatz und E/A-Latenzmetriken pro 50 Millisekunden für jeden physischen Datenträger an. Mit der Lösung lassen sich schnell Datenträgerdrosselungs-Perioden identifizieren.
- HighMemoryUsage-Regel: Erkennt Perioden mit hoher Arbeitsspeicherauslastung und zeigt die stärksten Arbeitsspeicher-Ressourcenverbraucher während dieser Periode an.

> [!NOTE] 
> Derzeit werden Windows-Versionen unterstützt, die .NET Framework 3.5 oder höher beinhalten.

### <a name="performance-counter-trace-"></a>Leistungsindikator-Ablaufverfolgung (**)

Die Methode erfasst die folgenden Leistungsindikatoren:

- \Process, \Processor, \Memory, \Thread, \PhysicalDisk, \LogicalDisk
- \Cache\Dirty Pages, \Cache\Lazy Write Flushes/sec, \Server\Pool Nonpaged, Failures, \Server\Pool Paged Failures
- Ausgewählte Indikatoren unter \Network Interface, \IPv4\Datagrams, \IPv6\Datagrams, \TCPv4\Segments, \TCPv6\Segments, \Network Adapter, \WFPv4\Packets, \WFPv6\Packets, \UDPv4\Datagrams, \UDPv6\Datagrams, \TCPv4\Connection, \TCPv6\Connection, \Network QoS Policy\Packets, \Per Processor Network Interface Card Activity, \Microsoft Winsock BSP

#### <a name="for-sql-server-instances"></a>Für SQL Server-Instanzen
- \SQL Server:Buffer Manager, \SQLServer:Resource Pool Stats, \SQLServer:SQL Statistics\
- \SQLServer:Locks, \SQLServer:General, Statistics
- \SQLServer:Access Methods

#### <a name="for-azure-files"></a>Für Azure Files
\SMB Client Shares

### <a name="diskspd-benchmark-trace-"></a>DiskSpd-Vergleichstest-Ablaufverfolgung (***)
DiskSpd-E/A-Workloadtests [Betriebssystemdatenträger (Schreibzugriff) und Poollaufwerke (Lese-/Schreibzugriff)]

## <a name="run-the-perfinsights-on-your-vm"></a>Ausführen von PerfInsights auf Ihrem virtuellen Computer

### <a name="what-do-i-have-to-know-before-i-run-the-script"></a>Was muss ich vor dem Ausführen des Skripts wissen? 

**Skriptanforderungen**

1.  Dieses Skript muss auf dem virtuellen Computer ausgeführt werden, bei dem das Leistungsproblem vorliegt. 

2.  Die folgenden Betriebssysteme werden unterstützt: Windows Server 2008 R2, 2012, 2012 R2, 2016; Windows 8.1 und Windows 10.

**Mögliche Probleme, wenn Sie das Skript für Produktions-VMs ausführen:**

1.  Das Skript könnte sich negativ auf die Leistung des virtuellen Computers auswirken, wenn es zusammen mit dem Szenario „Vergleichstest“ oder „Benutzerdefiniert“ verwendet wird, das mit XPerf oder DiskSpd konfiguriert wird. Bei Ausführung des Skripts in einer Produktionsumgebung ist Vorsicht geboten.

2.  Wenn Sie das Skript zusammen mit dem Szenario „Vergleichstest“ oder „Benutzerdefiniert“ verwenden, das mit DiskSpd konfiguriert wird, stellen Sie sicher, dass keine andere Hintergrundaktivität die E/A-Workload auf den getesteten Datenträgern beeinträchtigt.

3.  Standardmäßig verwendet das Skript das temporäre Speicherlaufwerk zum Speichern von Daten. Wenn die Ablaufverfolgung für einen längeren Zeitraum aktiviert bleibt, könnte die Menge der gesammelten Daten relevant sein. Dies kann den verfügbaren Speicher auf dem temporären Datenträger reduzieren und sich damit auf jede Anwendung auswirken, die von diesem Laufwerk abhängig ist.

### <a name="how-do-i-run-perfinsights"></a>Wie führe ich PerfInsights aus? 

Gehen Sie folgendermaßen vor, um das Skript auszuführen:

1. Laden Sie [PerfInsights.zip](http://aka.ms/perfinsightsdownload) herunter.

2. Entsperren Sie die Datei „PerfInsights.zip“. Klicken Sie zu diesem Zweck mit der rechten Maustaste auf die Datei „PerfInsights.zip“, und wählen Sie **Eigenschaften**. Wählen Sie auf der Registerkarte **Allgemein** die Option **Entsperren**, und wählen Sie **OK**. So stellen Sie sicher, dass das Skript ohne zusätzliche Sicherheitsabfragen ausgeführt wird.  

    ![ZIP-Datei entsperren](media/how-to-use-perfInsights/unlock-file.png)

3.  Erweitern Sie die komprimierte PerfInsights.zip-Datei in dem temporären Laufwerk (standardmäßig in der Regel Laufwerk „D“). Die komprimierte Datei sollte die folgenden Dateien und Ordner enthalten:

    ![Dateien im ZIP-Ordner](media/how-to-use-perfInsights/file-folder.png)

4.  Öffnen Sie Windows PowerShell als Administrator, und führen Sie das Skript „PerfInsights.ps1“ aus.

    ```
    cd <the path of PerfInsights folder >
    Powershell.exe -ExecutionPolicy UnRestricted -NoProfile -File .\\PerfInsights.ps1
    ```

    Möglicherweise müssen Sie „y“ eingeben, wenn Sie aufgefordert werden, zu bestätigen, dass Sie die Ausführungsrichtlinie ändern möchten.

    Im Dialogfeld „Haftungsausschluss“ haben Sie die Option, Diagnoseinformationen an den Microsoft-Support weiterzuleiten. Um fortzufahren, müssen Sie auch dem Lizenzvertrag zustimmen. Treffen Sie die Auswahl, und klicken Sie dann auf **Skript ausführen**.

    ![Dialogfeld „Haftungsausschluss“](media/how-to-use-perfInsights/disclaimer.png)

5.  Senden Sie die Fallnummer, falls verfügbar, wenn Sie das Skript ausführen (für unsere Statistiken). Klicken Sie dann auf **OK**.
    
    ![Support-ID eingeben](media/how-to-use-perfInsights/enter-support-number.png)

6.  Wählen Sie Ihr temporäres Speicherlaufwerk. Das Skript kann automatisch den Laufwerkbuchstaben des Laufwerks erkennen. Wenn in dieser Phase Probleme auftreten, könnten Sie aufgefordert werden, das Laufwerk auszuwählen (das Standardlaufwerk ist „D“). Generierte Protokolle werden hier im Ordner „log\_collection“ gespeichert. Nachdem Sie den Laufwerkbuchstaben eingegeben oder übernommen haben, klicken Sie auf **OK**.

    ![Laufwerk eingeben](media/how-to-use-perfInsights/enter-drive.png)

7.  Wählen Sie ein Problembehandlungsszenario in der vorliegenden Liste aus.

       ![Supportszenarien wählen](media/how-to-use-perfInsights/select-scenarios.png)

8.  Sie können PerfInsights auch ohne Benutzeroberfläche ausführen.

    Der folgende Befehl führt das Problembehandlungsszenario „Allgemeine VM Slow-Analyse“ ohne Benutzeroberflächen-Eingabeaufforderung oder Erfassung von Daten für 30 Sekunden aus. Sie werden aufgefordert, dem gleichen Haftungsausschluss und Lizenzvertrag zuzustimmen wie in Schritt 4.

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30"

    Wenn Sie PerfInsights im unbeaufsichtigten Modus ausführen möchten, verwenden Sie den **-AcceptDisclaimerAndShareDiagnostics**-Parameter. Verwenden Sie z.B. den folgenden Befehl:

        powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -NoGui -Scenario vmslow -TracingDuration 30 -AcceptDisclaimerAndShareDiagnostics"

### <a name="how-do-i-troubleshoot-issues-while-running-the-script"></a>Wie behebe ich Probleme beim Ausführen des Skripts?

Wenn das Skript nicht normal beendet wird, können Sie einen inkonsistenten Status bereinigen, indem Sie das Skript wie folgt zusammen mit der Option „-Cleanup“ ausführen:

    powershell.exe -ExecutionPolicy UnRestricted -NoProfile -Command ".\\PerfInsights.ps1 -Cleanup"

Wenn während der automatischen Erkennung des temporären Laufwerks Probleme auftreten, könnten Sie aufgefordert werden, das Laufwerk auszuwählen (das Standardlaufwerk ist „D“).

![Laufwerk eingeben](media/how-to-use-perfInsights/enter-drive.png)

Das Skript deinstalliert die Hilfsprogramme und entfernt temporäre Ordner.

### <a name="troubleshooting-other-script-issues"></a>Problembehandlung bei anderen Skriptproblemen 

Wenn bei der Ausführung des Skripts Probleme auftreten, drücken Sie STRG+C, um die Ausführung des Skripts zu unterbrechen. Wie Sie temporäre Objekte entfernen, erfahren Sie im Abschnitt „Clean up after abnormal termination“ (Bereinigen nach nicht ordnungsgemäßer Beendigung).

Wenn auch nach mehreren Versuchen weiterhin Skriptfehler auftreten, sollten Sie das Skript im „Debugmodus“ ausführen, indem Sie beim Start die „-Debug“-Parameteroption verwenden.

Wenn der Fehler aufgetreten ist, kopieren Sie die vollständige Ausgabe der PowerShell-Konsole, und senden Sie sie an den Microsoft-Support-Agent, der Sie bei der Behebung des Problems unterstützt.

### <a name="how-do-i-run-the-script-in-custom-configuration-mode"></a>Wie führe ich das Skript im benutzerdefinierten Konfigurationsmodus aus?

Durch Auswahl der **Benutzerdefinierten** Konfiguration können Sie mehrere parallele Ablaufverfolgungen aktivieren (verwenden Sie die UMSCHALT-Taste zur Mehrfachauswahl):

![Szenarien wählen](media/how-to-use-perfInsights/select-scenario.png)

Befolgen Sie bei der Auswahl der Szenarien Leistungsdiagnose, Leistungsindikator-Ablaufverfolgung, XPerf-Ablaufverfolgung, Ablaufverfolgung im Netzwerk oder StorPort-Ablaufverfolgung nach dem Starten von Ablaufverfolgungen die Anweisungen in den Dialogfeldern, und versuchen Sie, das Leistungsdefizitproblem zu reproduzieren.

Im folgenden Dialogfeld können Sie eine Ablaufverfolgung starten:

![start-trace](media/how-to-use-perfInsights/start-trace-message.png)

Um die Ablaufverfolgungen zu beenden, müssen Sie den Befehl in einem zweiten Dialogfeld bestätigen.

![stop-trace](media/how-to-use-perfInsights/stop-trace-message.png)
![stop-trace](media/how-to-use-perfInsights/ok-trace-message.png)

Wenn die Ablaufverfolgungen oder Vorgänge abgeschlossen sind, wird in „D:\\log\_collection“ (oder auf dem temporären Laufwerk) eine neue Datei mit der Bezeichnung **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip** generiert. Sie können diese Datei zur Analyse an den Support-Agent senden.

## <a name="review-the-diagnostics-report-created-by-perfinsights"></a>Überprüfen des von PerfInsights erstellten Diagnoseberichts

Innerhalb der von PerfInsights generierten Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip** finden Sie einen HTML-Bericht, der die Ergebnisse von PerfInsights detailliert beschreibt. Um den Bericht anzuzeigen, erweitern Sie die Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip**, und öffnen Sie dann die Datei **PerfInsights Report.html**.

Wählen Sie die Registerkarte **Ergebnisse**.

![Registerkarte „Ergebnisse“](media/how-to-use-perfInsights/findingtab.png)

**Hinweise**

-   Meldungen in rot sind bekannte Konfigurationsprobleme, die Leistungsprobleme verursachen können.

-   Meldungen in gelb sind Warnungen, die nicht optimale Konfigurationen darstellen, die nicht unbedingt Leistungsprobleme verursachen.

-   Meldungen in blau sind nur informative Anweisungen.

Überprüfen Sie die HTTP-Links für alle Fehlermeldungen in rot, um ausführlichere Informationen zu den Ergebnissen und ihren möglichen Auswirkungen auf Leistung oder bewährte Vorgehensweisen für leistungsoptimierte Konfigurationen zu erhalten.

### <a name="disk-configuration-tab"></a>Registerkarte „Datenträgerkonfiguration“

Der Abschnitt **Übersicht** zeigt unterschiedliche Ansichten der Speicherkonfiguration, einschließlich Informationen über DiskPart und Speicherplätze.

Die Abschnitte **DiskMap** und **VolumeMap** beschreiben aus jeweiliger Perspektive, wie logische Volumes und physische Datenträger miteinander verknüpft werden.

In der Perspektive PhysicalDisk (DiskMap) zeigt die Tabelle alle logischen Volumes, die auf dem Datenträger ausgeführt werden. Im folgenden Beispiel führt PhysicalDrive2 2 logische Volumes aus, die auf mehreren Partitionen („J“ und „H“) erstellt werden:

![Registerkarte „Daten“](media/how-to-use-perfInsights/disktab.png)

In der Volume-Perspektive (*VolumeMap*) zeigen die Tabellen alle physischen Datenträger unter den einzelnen logischen Volumes an. Beachten Sie, dass Sie für RAID- und dynamische Datenträger ein logisches Volume auf mehreren physischen Datenträgern ausführen können. Im folgenden Beispiel *C:\\mount* ist ein Bereitstellungspunkt als *SpannedDisk* auf PhysicalDisks \#2 und \#3 konfiguriert:

![Registerkarte „Volume“](media/how-to-use-perfInsights/volumetab.png)

### <a name="sql-server-tab"></a>Registerkarte „SQL Server“

Wenn die Ziel-VM SQL Server-Instanzen hostet, sehen Sie im Bericht eine zusätzliche Registerkarte mit dem Namen **SQL Server**:

![Registerkarte „SQL“](media/how-to-use-perfInsights/sqltab.png)

Dieser Abschnitt enthält eine „Übersicht“ und zusätzliche untergeordnete Registerkarten für jede SQL Server-Instanz, die auf dem virtuellen Computer gehostet wird.

Der Abschnitt „Übersicht“ enthält eine hilfreiche Tabelle, die alle physischen Datenträger (System- und Datendatenträger) zusammenfasst, die ausgeführt werden und eine Mischung von Daten- und Transaktionsprotokolldateien enthalten.

Im folgenden Beispiel wird *PhysicalDrive0* (auf dem Laufwerk „C“ ausgeführt wird) angezeigt, weil sowohl die Datei *modeldev* als auch *modellog* sich auf Laufwerk „C“ befindet, und sie unterschiedlichen Typs sind (z.B. Datendatei und Transaktionsprotokoll):

![loginfo](media/how-to-use-perfInsights/loginfo.png)

Die für SQL Server-Instanzen spezifischen Registerkarten enthalten einen allgemeinen Abschnitt, der grundlegende Informationen über die ausgewählte Instanz und zusätzliche Abschnitte für die erweiterten Informationen einschließlich Einstellungen, Konfigurationen und Benutzeroptionen anzeigt.

## <a name="references-to-the-external-tools-used"></a>Verweise auf die verwendeten externen Tools

### <a name="diskspd"></a>DiskSpd

DiskSpd ist ein Speicherladegenerator- und Leistungstesttool der Windows- und Windows Server- sowie Cloudserver-Infrastruktur-Entwicklungsteams. Weitere Informationen finden Sie unter [DiskSpd](https://github.com/Microsoft/diskspd).

### <a name="xperf"></a>XPerf

Xperf ist ein Befehlszeilentool zum Erfassen von Ablaufverfolgungen aus dem Windows Performance Toolkit.

Weitere Informationen finden Sie unter [Windows Performance Toolkit – Xperf](https://blogs.msdn.microsoft.com/ntdebugging/2008/04/03/windows-performance-toolkit-xperf/).

## <a name="next-steps"></a>Nächste Schritte

### <a name="upload-diagnostics-logs-and-reports-to-microsoft-support-for-further-review"></a>Hochladen von Diagnoseprotokollen und Berichten an den Microsoft-Support zur weiteren Prüfung

Bei der Zusammenarbeit mit den Mitarbeitern des Microsoft-Supports können Sie aufgefordert werden, die Ausgabe zu senden, die von PerfInsights bei der Problembehandlung generiert wird.

Der Support-Agent erstellt einen DTM-Arbeitsbereich für Sie, und Sie erhalten eine E-Mail-Nachricht, die einen Link zum DTM-Portal (https://filetransfer.support.microsoft.com/EFTClient/Account/Login.htm) sowie eine eindeutige Benutzer-ID und das Kennwort enthält.

Diese Nachricht wird von **CTS Automated Diagnostics Services** (ctsadiag@microsoft.com) gesendet.

![Beispiel der Nachricht](media/how-to-use-perfInsights/supportemail.png)

Zur Erhöhung der Sicherheit müssen Sie das Kennwort bei der ersten Verwendung ändern.

Nach der Anmeldung bei DTM sehen Sie ein Dialogfeld zum Hochladen der von PerfInsights erfassten Datei **CollectedData\_jjjj-MM-tt\_hh\_mm\_ss.zip**.

