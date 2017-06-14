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
ms.date: 03/28/2017
ms.author: robb
ms.translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: 81f9a5a8e5ce8389c12a40eb02dd554fc140e009
ms.contentlocale: de-de
ms.lasthandoff: 03/22/2017


---
# <a name="azure-diagnostics-troubleshooting"></a>Problembehandlung mit Azure-Diagnose
Informationen zur Problembehandlung mit Azure-Diagnose. Weitere Informationen zu Azure-Diagnose finden Sie unter [Überblick über Azure-Diagnose](azure-diagnostics.md).

## <a name="azure-diagnostics-is-not-starting"></a>Die Azure-Diagnose wird nicht gestartet.
Die Diagnose besteht aus zwei Komponenten: einem Gast-Agent-Plug-In und dem Überwachungs-Agent. Sie können die Protokolldateien **DiagnosticsPluginLauncher.log** und **DiagnosticsPlugin.log** auf Informationen darüber überprüfen, warum die Diagnose nicht gestartet wird.  

In einer Clouddienstrolle befinden sich die Protokolldateien für das Gast-Agent-Plug-In in:

```
C:\Logs\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.7.4.0\
```

Auf einem virtuellen Azure-Computer befinden sich die Protokolldateien für das Gast-Agent-Plug-In in:

```
C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\1.7.4.0\Logs\
```

Die letzte Zeile der Protokolldateien enthält den Exitcode.  

```
DiagnosticsPluginLauncher.exe Information: 0 : [4/16/2016 6:24:15 AM] DiagnosticPlugin exited with code 0
```

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
| -106 |Die Diagnosekonfigurationsdatei kann nicht gelesen werden.<p><p>Lösung: Der Grund hierfür ist, dass eine Konfigurationsdatei die Schemaüberprüfung nicht bestanden hat. Gelöst werden kann der Fehler daher durch Bereitstellung einer Konfigurationsdatei, die dem Schema entspricht. Für Azure Virtual Machines finden Sie die Konfiguration, die an die Diagnoseerweiterung übergeben wird, im Ordner *%SystemDrive%\WindowsAzure\Config* auf dem virtuellen Computer. Öffnen Sie die betreffende XML-Datei, und suchen Sie nach **Microsoft.Azure.Diagnostics** und dann nach dem Feld **xmlCfg** oder **WadCfg**. Wenn das Feld „WadCfg“ vorhanden ist, bedeutet dies, dass die Konfigurationsdatei das JSON-Format hat. Wenn das Feld „xmlCfg“ vorhanden ist, bedeutet dies, dass die Konfigurationsdatei das XML-Format hat und Base64-codiert ist. Sie müssen sie decodieren, um die von der Diagnose geladenen XML-Daten anzuzeigen. Für die Clouddienstrolle finden Sie die Konfiguration, die der Diagnoseerweiterung bereitgestellt wird, unter „C:\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\1.7.4.0\config.txt“ auf dem virtuellen Computer. Die Daten sind Base64-codiert und müssen daher zunächst [decodiert](http://www.bing.com/search?q=base64+decoder) werden, um die von der Diagnose geladenen XML-Daten anzuzeigen.<p> |
| -107 |Das an den Überwachungs-Agent übergebene Ressourcenverzeichnis ist ungültig.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn der Überwachungs-Agent auf dem virtuellen Computer auf falsche Weise manuell aufgerufen wird.</p> |
| -108 |Die Diagnosekonfigurationsdatei kann nicht in die Konfigurationsdatei des Überwachungs-Agents konvertiert werden.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Diagnose-Plug-In manuell mit einer ungültigen Konfigurationsdatei aufgerufen wird. |
| -110 |Allgemeiner Fehler in der Diagnosekonfiguration.<p><p>Dies ist ein interner Fehler, der nur auftreten sollte, wenn das Diagnose-Plug-In manuell mit einer ungültigen Konfigurationsdatei aufgerufen wird. |
| -111 |Der Überwachungs-Agent kann nicht gestartet werden.<p><p>Lösung: Überprüfen Sie, ob genügend Systemressourcen verfügbar sind. |
| -112 |Allgemeiner Fehler |

## <a name="diagnostics-data-is-not-logged-to-azure-storage"></a>Diagnosedaten werden nicht in Azure Storage protokolliert
Azure-Diagnose speichert alle Daten in Azure Storage.

Wenn Ereignisdaten fehlen, liegt das in den meisten Fällen an falsch definierten Speicherkontoinformationen.

Lösung: Korrigieren Sie die Diagnosekonfigurationsdatei, und installieren Sie die Diagnose erneut.
Wenn das Problem weiterhin auftritt, nachdem Sie die Diagnoseerweiterung neu installiert haben, müssen Sie möglicherweise das Debuggen fortsetzen und den Überwachungs-Agent auf Fehler überprüfen. Bevor Ereignisdaten in Ihr Speicherkonto hochgeladen werden, werden sie in LocalResourceDirectory gespeichert.
Dieser Verzeichnispfad variiert.

Für Clouddienstrollen:

    C:\Resources\Directory\<CloudServiceDeploymentID>.<RoleName>.DiagnosticStore\WAD<DiagnosticsMajorandMinorVersion>\Tables

Für Virtual Machines:

    C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\WAD<DiagnosticsMajorandMinorVersion>\Tables

Wenn der LocalResourceDirectory-Ordner keine Dateien enthält, kann der Überwachungs-Agent nicht gestartet werden. Der Grund ist normalerweise eine ungültige Konfigurationsdatei, ein Ereignis, für das die Datei „CommandExecution.log“ einen entsprechenden Eintrag enthalten sollte.

Wenn der Überwachungs-Agent erfolgreich Ereignisdaten erfasst, werden für jedes in der Konfigurationsdatei definierte Ereignis TSF-Dateien angezeigt. Der Überwachungs-Agent protokolliert alle Fehler in der Datei „MaEventTable.tsf“. Um den Inhalt dieser Datei anzuzeigen, können Sie die TSF-Datei mithilfe der Anwendung table2csv in eine Datei mit durch Trennzeichen getrennten Werten (CSV) konvertieren:

Bei einer Clouddienstrolle:

    %SystemDrive%\Packages\Plugins\Microsoft.Azure.Diagnostics.PaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

*%SystemDrive%* entspricht bei einer Clouddienstrolle in der Regel dem Laufwerk D:

Auf einem virtuellen Computer:

    C:\Packages\Plugins\Microsoft.Azure.Diagnostics.IaaSDiagnostics\<DiagnosticsVersion>\Monitor\x64\table2csv maeventtable.tsf

Mit den vorherigen Befehlen wird die Protokolldatei *maeventtable.csv* generiert, die Sie öffnen und auf Fehlermeldungen prüfen können.    

## <a name="diagnostics-data-tables-not-found"></a>Tabellen mit Diagnosedaten nicht gefunden
Die Tabellen im Azure-Speicher, die Azure-Diagnosedaten enthalten, werden anhand des folgenden Codes benannt:

```csharp
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
        <EtwEventSourceProviderConfiguration provider="prov1">
          <Event id="1" />
          <Event id="2" eventDestination="dest1" />
          <DefaultEvents />
        </EtwEventSourceProviderConfiguration>
        <EtwEventSourceProviderConfiguration provider="prov2">
          <DefaultEvents eventDestination="dest2" />
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

