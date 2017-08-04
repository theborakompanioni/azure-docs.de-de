---
title: Problembehandlung bei der Azure-Diagnose | Microsoft-Dokumentation
description: Behandeln Sie Probleme bei der Verwendung der Azure-Diagnose in Azure Virtual Machines, Service Fabric und Cloud Services.
services: monitoring-and-diagnostics
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 66469bce-d457-4d1e-b550-a08d2be4d28c
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/12/2017
ms.author: robb
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: df53e92b877b4790bb700f176a1988d265ec4678
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---
# <a name="azure-diagnostics-troubleshooting"></a>Problembehandlung mit Azure-Diagnose
Informationen zur Problembehandlung mit Azure-Diagnose. Weitere Informationen zu Azure-Diagnose finden Sie unter [Überblick über Azure-Diagnose](azure-diagnostics.md).

## <a name="logical-components"></a>Logische Komponenten
**Launcher für Diagnose-Plug-In (DiagnosticsPluginLauncher.exe)**: Startet die Azure-Diagnoseerweiterung. Dient als Prozess für den Einstiegspunkt.

**Diagnose-Plug-In (DiagnosticsPlugin.exe)**: Hauptprozess, der vom obigen Launcher gestartet wird und den Monitoring Agent konfiguriert, diesen startet und die Lebensdauer verwaltet. 

**Monitoring Agent (MonAgent\*.exe-Prozesse)**: Mit diesen Prozessen wird der Großteil der Arbeitsschritte ausgeführt, z.B. Überwachung, Sammlung und Übertragung der Diagnosedaten.  

## <a name="logartifact-paths"></a>Protokoll-/Artefaktpfade
Hier sind die Pfade zu einigen wichtigen Protokollen und Artefakten angegeben. Im restlichen Teil dieses Dokuments werden wir immer wieder darauf verweisen:
### <a name="cloud-services"></a>Cloud Services
| Artefakt | Pfad |
| --- | --- |
| **Azure-Diagnosekonfigurationsdatei** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\Config.txt |
| **Protokolldateien** | C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version>\ |
| **Lokaler Speicher für Diagnosedaten** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Tables |
| **Konfigurationsdatei für Monitoring Agent** | C:\Resources\Directory\<CloudServiceDeploymentID>.\<RoleName>.DiagnosticStore\WAD0107\Configuration\MaConfig.xml |
| **Paket mit Azure-Diagnoseerweiterung** | %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<version> |
| **Pfad des Hilfsprogramms für die Protokollsammlung** | %SystemDrive%\Packages\GuestAgent\ |
| **MonAgentHost-Protokolldatei** | C:\Resources\Directory\<Clouddienstbereitstellungs-ID>.\<Rollenname>.DiagnosticStore\WAD0107\Configuration\MonAgentHost.<Sequenznummer>.log |

### <a name="virtual-machines"></a>Virtual Machines
| Artefakt | Pfad |
| --- | --- |
| **Azure-Diagnosekonfigurationsdatei** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\RuntimeSettings |
| **Protokolldateien** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Logs\ |
| **Lokaler Speicher für Diagnosedaten** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Tables |
| **Konfigurationsdatei für Monitoring Agent** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD0107\Configuration\MaConfig.xml |
| **Statusdatei** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<version>\Status |
| **Paket mit Azure-Diagnoseerweiterung** | C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>|
| **Pfad des Hilfsprogramms für die Protokollsammlung** | C:\WindowsAzure\Packages |
| **MonAgentHost-Protokolldatei** | C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<Diagnoseversion>\WAD0107\Configuration\MonAgentHost.<Sequenznummer>.log |

## <a name="azure-diagnostics-is-not-starting"></a>Die Azure-Diagnose wird nicht gestartet.
Sehen Sie sich die Dateien **DiagnosticsPluginLauncher.log** und **DiagnosticsPlugin.log** am oben angegebenen Speicherort der Protokolldateien an, um Informationen dazu zu erhalten, warum die Diagnose nicht gestartet werden konnte. 

Die Angabe `Monitoring Agent not reporting success after launch` in diesen Protokollen bedeutet, dass beim Starten von „MonAgentHost.exe“ ein Fehler aufgetreten ist. Durchsuchen Sie die Protokolle nach dieser Angabe. (Die Protokolle befinden sich an dem Speicherort, der im obigen Abschnitt für `MonAgentHost log file` angegeben ist.)

Die letzte Zeile der Protokolldateien enthält den Exitcode.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```
Wenn Sie einen **negativen** Exitcode finden, hilft Ihnen die [Exitcode-Tabelle](#azure-diagnostics-plugin-exit-codes) unter [Referenzen](#references) weiter.

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnosedaten werden nicht in Azure Storage protokolliert
Ermitteln Sie, ob keine Daten oder nur einige Daten nicht angezeigt werden.

### <a name="diagnostics-infrastructure-logs"></a>Diagnoseinfrastrukturprotokolle
In den Diagnoseinfrastrukturprotokollen werden von der Azure-Diagnose alle auftretenden Fehler protokolliert. Stellen Sie sicher, dass Sie die Erfassung von Diagnoseinfrastrukturprotokollen in Ihrer Konfiguration aktiviert haben ([Vorgehensweise](#how-to-check-diagnostics-extension-configuration)), und suchen Sie kurz nach relevanten Fehlern, die in Ihrem konfigurierten Speicherkonto in der Tabelle `DiagnosticInfrastructureLogsTable` angezeigt werden.

### <a name="no-data-is-showing-up"></a>Es werden keine Daten angezeigt
Wenn Ereignisdaten vollständig fehlen, liegt dies in den meisten Fällen an falsch definierten Speicherkontoinformationen.

Lösung: Korrigieren Sie die Diagnosekonfiguration, und installieren Sie die Diagnose erneut.

Wenn das Speicherkonto richtig konfiguriert wurde, sollten Sie per Remotedesktop auf den Computer zugreifen und sicherstellen, dass „DiagnosticsPlugin.exe“ und „MonAgentCore.exe“ ausgeführt werden. Falls nicht, helfen Ihnen die Informationen unter [Die Azure-Diagnose wird nicht gestartet](#azure-diagnostics-is-not-starting) weiter. Wenn die Prozesse ausgeführt werden, können Sie zu [Lokale Erfassung von Daten](#is-data-getting-captured-locally) springen und die Anleitung befolgen.

### <a name="part-of-the-data-is-missing"></a>Ein Teil der Daten fehlt
Trifft zu, wenn Sie einige Daten erhalten, andere dagegen nicht. Dies bedeutet, dass die Datenerfassung bzw. Übertragungspipeline richtig festgelegt wurde. Mit den Informationen in den folgenden Unterabschnitten können Sie eingrenzen, was die Ursache für das Problem ist:
#### <a name="is-collection-configured"></a>Konfiguration der Sammlung: 
Die Diagnosekonfiguration enthält den Teil, der bewirkt, dass Daten eines bestimmten Typs erfasst werden. [Prüfen Sie Ihre Konfiguration](#how-to-check-diagnostics-extension-configuration), um sich zu vergewissern, dass Sie nicht nach Daten suchen, für die Sie die Sammlung nicht konfiguriert haben.
#### <a name="is-the-host-generating-data"></a>Generierung von Daten durch den Host:
- **Leistungsindikatoren**: Öffnen Sie den Systemmonitor, und überprüfen Sie den Leistungsindikator.
- **Ablaufverfolgungsprotokolle**: Greifen Sie per Remotedesktop auf die VM zu, und fügen Sie der Konfigurationsdatei der App einen TextWriterTraceListener hinzu.  Informationen zum Einrichten des Textlisteners finden Sie unter „http://msdn.microsoft.com/en-us/library/sk36c28t.aspx“.  Stellen Sie sicher, dass für das `<trace>`-Element `<trace autoflush="true">` festgelegt ist.<br />
Wenn Sie nicht sehen, dass Ablaufverfolgungsprotokolle generiert werden, helfen Ihnen die Informationen unter [Weitere Informationen zu fehlenden Ablaufverfolgungsprotokollen](#more-about-trace-logs-missing) weiter.
- **ETW-Ablaufverfolgungen**: Greifen Sie per Remotedesktop auf die VM zu, und installieren Sie PerfView.  Führen Sie in PerfView die Optionen „File“ > „User Command“ > „Listen etwprovider1,etwprovider2“ („Datei“ > „Benutzerbefehl“ > „Lauschen etwprovider1,etwprovider2“) usw. aus.  Beachten Sie, dass beim Befehl „Listen“ (Lauschen) die Groß-/Kleinschreibung beachtet wird und die kommagetrennte Liste mit ETW-Anbietern keine Leerstellen enthalten darf.  Falls der Befehl nicht ausgeführt werden kann, können Sie im Perfview-Tool unten rechts auf die Schaltfläche „Log“ (Protokoll) klicken, um anzuzeigen, was ausgeführt werden sollte und wie das Ergebnis lautet.  Wenn die Eingabe richtig ist, wird ein neues Fenster angezeigt, und innerhalb einiger Sekunden werden ETW-Ablaufverfolgungen sichtbar.
- **Ereignisprotokolle**: Greifen Sie per Remotedesktop auf die VM zu. Öffnen Sie `Event Viewer`, und stellen Sie sicher, dass die Ereignisse vorhanden sind.
#### <a name="is-data-getting-captured-locally"></a>Lokale Erfassung von Daten:
Stellen Sie als Nächstes sicher, dass die Daten lokal erfasst werden.
Die Daten werden lokal in `*.tsf`-Dateien im [lokalen Speicher für die Diagnosedaten gespeichert](#log-artifacts-path). Verschiedene Arten von Protokollen werden in unterschiedlichen `.tsf`-Dateien erfasst. Die Namen ähneln den Tabellennamen in Azure Storage. Beispielsweise werden `Performance Counters` in `PerformanceCountersTable.tsf` und Ereignisprotokolle in `WindowsEventLogsTable.tsf` erfasst. Nutzen Sie die Anleitung im Abschnitt [Extraktion von lokalen Protokollen](#local-log-extraction), um die lokalen Auflistungsdateien zu öffnen, und vergewissern Sie sich, dass die Erfassung auf dem Datenträger erfolgt.

Wenn Sie nicht erkennen können, dass Protokolle lokal erfasst werden, und bereits überprüft haben, dass der Host Daten generiert, liegt vermutlich ein Konfigurationsproblem vor. Überprüfen Sie Ihre Konfiguration für den entsprechenden Abschnitt sorgfältig. Überprüfen Sie außerdem die generierte Konfiguration für die MonitoringAgent-Datei [MaConfig.xml](#log-artifacts-path), und stellen Sie Folgendes sicher: Es ist ein Abschnitt vorhanden, in dem die relevante Protokollquelle beschrieben wird, und dieser Abschnitt ist bei der Übersetzung zwischen der Konfiguration der Azure-Diagnose und der Konfiguration des Monitoring Agent nicht verloren gegangen.
#### <a name="is-data-getting-transferred"></a>Übertragung von Daten:
Gehen Sie wie folgt vor, wenn Sie sich davon überzeugt haben, dass Daten lokal erfasst werden, diese in Ihrem Speicherkonto aber immer noch nicht angezeigt werden: 
- Stellen Sie zuerst sicher, dass Sie ein richtiges Speicherkonto angegeben haben und dass Sie für das jeweilige Speicherkonto keinen Rollover für Schlüssel usw. durchgeführt haben. Bei Clouddiensten kommt es manchmal vor, dass Benutzer `useDevelopmentStorage=true` nicht aktualisieren.
- Wenn das angegebene Speicherkonto korrekt ist: Stellen Sie sicher, dass bei Ihnen keine Netzwerkbeschränkungen gelten, die verhindern, dass die Komponenten öffentliche Speicherendpunkte erreichen. Eine Möglichkeit besteht hierbei darin, per Remotedesktop auf den Computer zuzugreifen und selbst Daten in dasselbe Speicherkonto zu schreiben.
- Außerdem können Sie prüfen, welche Fehler vom Monitoring Agent gemeldet werden. Der Monitoring Agent schreibt seine Protokolle in `maeventtable.tsf` im [lokalen Speicher für Diagnosedaten](#log-artifacts-path). Befolgen Sie die Anleitung im Abschnitt [Extraktion von lokalen Protokollen](#local-log-extraction), um diese Datei zu öffnen und nach Möglichkeit zu ermitteln, ob Fehler beim Lesen lokaler Dateien oder Schreiben in den Speicher als `errors` aufgeführt sind.

### <a name="capturing--archiving-logs"></a>Erfassen/Archivieren von Protokollen
Gegebenenfalls haben Sie die obigen Schritte ausgeführt, konnten aber nicht herausfinden, wo der Fehler lag, und erwägen nun die Kontaktaufnahme mit dem Support. Als Erstes werden Sie wahrscheinlich gebeten, Protokolle auf Ihrem Computer zu erfassen. Sie können Zeit sparen, indem Sie dies vorab selbst durchführen. Führen Sie das Hilfsprogramm `CollectGuestLogs.exe` unter [Pfad des Hilfsprogramms für die Protokollsammlung](#log-artifacts-path) aus. Es wird eine ZIP-Datei mit allen relevanten Azure-Protokollen in demselben Ordner generiert.

## <a name="diagnostics-data-tables-not-found"></a>Tabellen mit Diagnosedaten nicht gefunden
Die Tabellen im Azure-Speicher, die ETW-Ereignisse enthalten, werden anhand des folgenden Codes benannt:

```C#
        if (String.IsNullOrEmpty(eventDestination)) {
            if (e == "DefaultEvents")
                tableName = "WADDefault" + MD5(provider);
            else
                tableName = "WADEvent" + MD5(provider) + eventId;
        }
        else
            tableName = "WAD" + eventDestination;
```

Beispiel:

```XML
        <EtwEventSourceProviderConfiguration provider=”prov1”>
          <Event id=”1” />
          <Event id=”2” eventDestination=”dest1” />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider=”prov2”>
          <DefaultEvents eventDestination=”dest2” />
        </EtwEventSourceProviderConfiguration>
```
```JSON
"EtwEventSourceProviderConfiguration": [
    {
        "provider": "prov1",
        "Event": [
            {
                "id": 1
            },
            {
                "id": 2,
                "eventDestination": "dest1"
            }
        ],
        "DefaultEvents": {
            "eventDestination": "DefaultEventDestination",
            "sinks": ""
        }
    },
    {
        "provider": "prov2",
        "DefaultEvents": {
            "eventDestination": "dest2"
        }
    }
]
```
Damit werden vier Tabellen generiert:

| Ereignis | Tabellenname |
| --- | --- |
| provider=”prov1” &lt;Event id=”1” /&gt; |WADEvent+MD5(“prov1”)+”1” |
| provider=”prov1” &lt;Event id=”2” eventDestination=”dest1” /&gt; |WADdest1 |
| provider=”prov1” &lt;DefaultEvents /&gt; |WADDefault+MD5(“prov1”) |
| provider=”prov2” &lt;DefaultEvents eventDestination=”dest2” /&gt; |WADdest2 |

## <a name="references"></a>Referenzen

### <a name="how-to-check-diagnostics-extension-configuration"></a>Überprüfen der Konfiguration der Diagnoseerweiterung
Die einfachste Möglichkeit zum Überprüfen Ihrer Erweiterungskonfiguration ist das Navigieren zu „http://resources.azure.com“ und zu dem virtuellen Computer oder Clouddienst, unter dem sich die betreffende Azure-Diagnoseerweiterung (IaaSDiagnostics/PaaDiagnostics) befindet.

Greifen Sie alternativ dazu per Remotedesktop auf den Computer zu, und sehen Sie sich die Datei für die Azure-Diagnosekonfiguration an, die [hier](#log-artifacts-path) im entsprechenden Abschnitt beschrieben ist.

Suchen Sie jeweils nach **Microsoft.Azure.Diagnostics** und dann nach dem Feld **xmlCfg** oder **WadCfg**. 

Wenn bei virtuellen Computern das Feld „WadCfg“ vorhanden ist, bedeutet dies, dass die Konfiguration im JSON-Code vorliegt. Wenn das Feld „xmlCfg“ vorhanden ist, bedeutet dies, dass die Konfigurationsdatei das XML-Format hat und Base64-codiert ist. Sie müssen sie [decodieren](http://www.bing.com/search?q=base64+decoder), um die von der Diagnose geladenen XML-Daten anzuzeigen.

Wenn Sie bei einer Clouddienstrolle die Konfiguration vom Datenträger auswählen, sind die Daten Base64-codiert. Sie müssen sie also [decodieren](http://www.bing.com/search?q=base64+decoder), um die XML-Daten anzuzeigen, die von der Diagnose geladen wurden.

### <a name="azure-diagnostics-plugin-exit-codes"></a>Azure-Diagnose-Plug-In – Exitcodes
Das Plug-In gibt die folgenden Exitcodes zurück:

| Exitcode | Beschreibung |
| --- | --- |
| 0 |Erfolgreich. |
| -1 |Allgemeiner Fehler. |
| -2 |Die RCF-Datei konnte nicht geladen werden.<p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Startprogramm für das Gast-Agent-Plug-In auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird. |
| -3 |Die Diagnosekonfigurationsdatei kann nicht geladen werden.<p><p>Lösung: Der Grund hierfür ist, dass eine Konfigurationsdatei die Schemaüberprüfung nicht bestanden hat. Gelöst werden kann der Fehler durch Bereitstellung einer Konfigurationsdatei, die dem Schema entspricht. |
| -4 |Es wird bereits eine andere Instanz des Überwachungs-Agents der Diagnose unter Verwendung des lokalen Ressourcenverzeichnisses ausgeführt.<p><p>Lösung: Geben Sie für **LocalResourceDirectory**. |
| -6 |Das Startprogramm für das Gast-Agent-Plug-In hat versucht, die Diagnose mit einer ungültigen Befehlszeile zu starten.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Startprogramm für das Gast-Agent-Plug-In auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird. |
| -10 |Das Diagnose-Plug-In wurde mit einem Ausnahmefehler beendet. |
| -11 |Der Gast-Agent konnte den für den Start und die Überwachung des Überwachungs-Agents zuständigen Prozess nicht erstellen.<p><p>Lösung: Überprüfen Sie, ob genügend Systemressourcen zum Starten neuer Prozesse verfügbar sind.<p> |
| -101 |Ungültige Argumente beim Aufrufen des Diagnose-Plug-Ins.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Startprogramm für das Gast-Agent-Plug-In auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird. |
| -102 |Der Plug-In-Prozess kann sich nicht selbst initialisieren.<p><p>Lösung: Überprüfen Sie, ob genügend Systemressourcen zum Starten neuer Prozesse verfügbar sind. |
| -103 |Der Plug-In-Prozess kann sich nicht selbst initialisieren. Insbesondere kann das Protokollierungsobjekt nicht erstellt werden.<p><p>Lösung: Überprüfen Sie, ob genügend Systemressourcen zum Starten neuer Prozesse verfügbar sind. |
| -104 |Die vom Gast-Agent bereitgestellte RCF-Datei konnte nicht geladen werden.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Startprogramm für das Gast-Agent-Plug-In auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird. |
| -105 |Das Diagnose-Plug-In kann die Diagnosekonfigurationsdatei nicht öffnen.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Diagnose-Plug-In auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird. |
| -106 |Die Diagnosekonfigurationsdatei kann nicht gelesen werden.<p><p>Lösung: Der Grund hierfür ist, dass eine Konfigurationsdatei die Schemaüberprüfung nicht bestanden hat. Gelöst werden kann der Fehler daher durch Bereitstellung einer Konfigurationsdatei, die dem Schema entspricht. Weitere Informationen finden Sie unter [Überprüfen der Konfiguration der Diagnoseerweiterung](#how-to-check-diagnostics-extension-configuration). |
| -107 |Das an den Überwachungs-Agent übergebene Ressourcenverzeichnis ist ungültig.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn der Überwachungs-Agent auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird.</p> |
| -108 |Die Diagnosekonfigurationsdatei kann nicht in die Konfigurationsdatei des Überwachungs-Agents konvertiert werden.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Diagnose-Plug-In manuell mit einer ungültigen Konfigurationsdatei aufgerufen wird. |
| -110 |Allgemeiner Fehler in der Diagnosekonfiguration.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Diagnose-Plug-In manuell mit einer ungültigen Konfigurationsdatei aufgerufen wird. |
| -111 |Der Überwachungs-Agent kann nicht gestartet werden.<p><p>Lösung: Überprüfen Sie, ob genügend Systemressourcen verfügbar sind. |
| -112 |Allgemeiner Fehler |

### <a name="local-log-extraction"></a>Extraktion von lokalen Protokollen
Der Monitoring Agent sammelt Protokolle und Artefakte in Form von `.tsf`-Dateien. Die `.tsf`-Datei ist nicht lesbar, aber Sie können sie wie folgt in eine `.csv`-Datei konvertieren: 

```
<Azure diagnostics extension package>\Monitor\x64\table2csv.exe <relevantLogFile>.tsf
```
Eine neue Datei mit dem Namen `<relevantLogFile>.csv` wird unter demselben Pfad wie die entsprechende `.tsf`-Datei erstellt.

**HINWEIS**: Sie müssen dieses Hilfsprogramm nur für die TSF-Hauptdatei (z.B. PerformanceCountersTable.tsf) ausführen. Die Begleitdateien (z.B. PerformanceCountersTables_\*\*001.tsf, PerformanceCountersTables_\*\*002.tsf usw.) werden automatisch verarbeitet.

### <a name="more-about-trace-logs-missing"></a>Weitere Informationen zu fehlenden Ablaufverfolgungsprotokollen

**Hinweis:** Dies gilt meist nur für Clouddienste, sofern Sie DiagnosticsMonitorTraceListener nicht für eine Anwendung konfiguriert haben, die auf Ihrer IaaS-VM ausgeführt wird. 

- Stellen Sie sicher, dass das DiagnosticMonitorTraceListener-Element in „web.config“ oder „app.config“ konfiguriert wurde.  Dieses Element wird bei Clouddienstprojekten standardmäßig konfiguriert, aber einige Kunden kommentieren es aus. Dies führt dazu, dass die Überwachungsanweisungen von der Diagnose nicht erfasst werden. 
- Falls die Protokolle nicht mit der OnStart- oder Run-Methode geschrieben werden, sollten Sie sicherstellen, dass DiagnosticMonitorTraceListener in „app.config“ enthalten ist.  Standardmäßig ist dieses Element in „web.config“ enthalten, aber dies gilt nur für Code, der innerhalb von „w3wp.exe“ ausgeführt wird. Sie benötigen es also in „app.config“, um Ablaufverfolgungen zu erfassen, die in „WaIISHost.exe“ ausgeführt werden.
- Vergewissern Sie sich, dass Sie „Diagnostics.Trace.TraceXXX“ anstelle von „Diagnostics.Debug.WriteXXX“ verwenden.  Die Debuganweisungen werden aus einem Versionsbuild entfernt.
- Stellen Sie sicher, dass der kompilierte Code über die Diagnostics.Trace-Zeilen verfügt (Reflector, ildasm oder ILSpy für die Überprüfung verwenden).  Diagnostics.Trace-Befehle werden aus der kompilierten Binärdatei entfernt, sofern Sie nicht das Symbol für die bedingte TRACE-Kompilierung verwenden.  Wenn msbuild zum Erstellen des Projekts genutzt wird, ist dies ein Problem, das häufig auftreten kann.

## <a name="known-issues-and-mitigations"></a>Bekannte Probleme und Lösungen
Die folgende Liste enthält bekannte Probleme und Lösungen:

**1. .NET 4.5-Abhängigkeit:**

WAD verfügt über eine Laufzeitabhängigkeit von .NET 4.5 Framework oder höher. Zum Zeitpunkt der Erstellung dieses Artikels ist auf allen Computern, die für Clouddienste bereitgestellt werden, sowie auf allen offiziellen Basisimages virtueller Azure-Computer .NET 4.5 oder höher installiert. Es kann aber trotzdem zu einer Situation kommen, in der Sie WAD auf einem Computer ausführen möchten, auf dem .NET 4.5 oder höher nicht vorhanden ist. Dies passiert, wenn Sie Ihren Computer aus einem alten Image oder einer alten Momentaufnahme erstellen oder einen eigenen benutzerdefinierten Datenträger verwenden.

Hierbei tritt im Allgemeinen der Exitcode **255** auf, wenn „DiagnosticsPluginLauncher.exe“ ausgeführt wird. Fehler aufgrund des Ausnahmefehlers: 
```
System.IO.FileLoadException: Could not load file or assembly 'System.Threading.Tasks, Version=1.5.11.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a' or one of its dependencies
```

**Lösung:** Installieren Sie .NET 4.5 oder höher auf Ihrem Computer.

**2. Daten von Leistungsindikatoren sind im Speicher verfügbar, werden aber nicht im Portal angezeigt:**

Im Portal für virtuelle Computer werden bestimmte Leistungsindikatoren standardmäßig angezeigt. Es kann sein, dass diese nicht angezeigt werden, Sie aber wissen, dass sie generiert werden, da die Daten im Speicher verfügbar sind. Prüfen Sie in diesem Fall Folgendes:
- Haben die Daten im Speicher englische Indikatornamen? Wenn die Indikatornamen nicht auf Englisch sind, können sie vom Portalmetrikdiagramm nicht erkannt werden.
- Wenn Sie in den Namen Ihrer Leistungsindikatoren Platzhalter (\*) verwenden, ist es für das Portal nicht möglich, den konfigurierten und erfassten Indikator zu korrelieren.

**Lösung**: Ändern Sie die Sprache des Computers für Systemkonten in Englisch. „Systemsteuerung“ > „Region“ > „Verwaltung“ > „Einstellungen kopieren“: Deaktivieren Sie die Option „Willkommensseite und Systemkonten“, damit die benutzerdefinierte Sprache nicht auf das Systemkonto angewendet wird. Stellen Sie auch sicher, dass Sie keine Platzhalter verwenden, wenn Sie das Portal als vorrangige Benutzeroberfläche nutzen möchten.
