---
title: Konfigurieren von Service Map in der Operations Management Suite (OMS) | Microsoft Docs
description: "Service Map ist eine Lösung in der Operations Management Suite (OMS), die automatisch Anwendungskomponenten auf Windows- und Linux-Systemen ermittelt und die Kommunikation zwischen Diensten abbildet.  Dieser Artikel enthält Informationen zum Bereitstellen von Service Map in Ihrer Umgebung und zur Verwendung der Lösung in einer Vielzahl von Szenarien."
services: operations-management-suite
documentationcenter: 
author: daveirwin1
manager: jwhit
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/18/2016
ms.author: daseidma;bwren;dairwin
translationtype: Human Translation
ms.sourcegitcommit: 450755b94d7abab9ab7fa626e02f404a73c97865
ms.openlocfilehash: ac17a567092b8ecf4908d28e6743ba0816fa214e
ms.lasthandoff: 02/22/2017


---
# <a name="configuring-service-map-solution-in-operations-management-suite-oms"></a>Konfigurieren von Service Map in der Operations Management Suite (OMS)
Service Map ermittelt automatisch Anwendungskomponenten auf Windows- und Linux-Systemen und stellt die Kommunikation zwischen Diensten dar. In dieser Lösung können Sie die Server ihrer Funktion gemäß anzeigen – als verbundene Systeme, die wichtige Dienste bereitstellen.  Service Map zeigt Verbindungen zwischen Servern, Prozessen und Ports über die gesamte TCP-Verbindungsarchitektur an. Außer der Installation eines Agents ist keine weitere Konfiguration erforderlich.

In diesem Artikel werden das Konfigurieren von Service Map und das Onboarding von Agents beschrieben.  Weitere Informationen zum Verwenden von Service Map finden Sie unter [Using Service Map solution in Operations Management Suite (OMS)](operations-management-suite-service-map.md) (Verwenden von Service Map in der Operations Management Suite [OMS]).


## <a name="connected-sources"></a>Verbundene Quellen
Die Dienstzuordnung ruft ihre Daten vom Microsoft Dependency-Agent ab.  Der Dependency-Agent ist abhängig vom OMS-Agent, da er dessen Verbindungen mit OMS benötigt.  Dies bedeutet, dass auf einem Server zunächst der OMS-Agent installiert und konfiguriert werden muss, bevor der Dependency-Agent installiert werden kann.  In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von Service Map unterstützt werden.

| Verbundene Quelle | Unterstützt | Beschreibung |
|:--|:--|:--|
| Windows-Agents | Ja | Service Map analysiert und erfasst Daten von Windows-Agent-Computern.  <br><br>Zusätzlich zum [OMS-Agent](../log-analytics/log-analytics-windows-agents.md) erfordern Windows-Agents den Microsoft Dependency-Agent.  Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| Linux-Agents | Ja | Service Map analysiert und erfasst Daten von Linux-Agent-Computern.  <br><br>Zusätzlich zum [OMS-Agent](../log-analytics/log-analytics-linux-agents.md) erfordern Linux-Agents den Microsoft Dependency-Agent.  Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](#supported-operating-systems). |
| SCOM-Verwaltungsgruppe | Ja | Service Map analysiert und erfasst Daten von Windows- und Linux-Agents in einer verbundenen [System Center Operations Manager-Verwaltungsgruppe (SCOM)](../log-analytics/log-analytics-om-agents.md). <br><br>Es ist keine direkte Verbindung vom SCOM-Agent-Computer zu OMS erforderlich. Die Daten werden von der Verwaltungsgruppe direkt an das OMS-Repository weitergeleitet.|
| Azure-Speicherkonto | Nein | Da Service Map Daten von Agent-Computern erfasst, sind keine Daten aus dem Azure-Speicher zu erfassen. |

Beachten Sie, dass Service Map nur 64-Bit-Plattformen unterstützt.

Unter Windows wird von SCOM und OMS zum Erfassen und Senden von Überwachungsdaten Microsoft Monitoring Agent (MMA) verwendet.  (Dieser Agent wird je nach Kontext als SCOM-Agenten, OMS-Agent, MMA oder Direct Agent bezeichnet.)  SCOM und OMS bieten unterschiedliche vorkonfigurierte Versionen von MMA, jede dieser Version kann jedoch Berichte an SCOM oder OMS oder an SCOM und OMS senden.  Unter Linux erfasst der OMS-Agent für Linux Überwachungsdaten und sendet sie an OMS.  Sie können Service Map auf Servern mit direkten OMS-Agents oder auf Servern, die über SCOM-Verwaltungsgruppen OMS zugeordnet sind, verwenden.  In dieser Dokumentation werden alle diese Agents – ob unter Linux oder Windows, ob mit einer SCOM-Verwaltungsgruppe oder direkt mit OMS verbunden – als „OMS-Agent“ bezeichnet, es sei denn, für den Kontext wird der spezielle Bereitstellungsname des Agent benötigt.

Der Service Map-Agent selbst überträgt keine Daten und erfordert keine Änderungen an Firewalls oder Ports.  Die Daten von Service Map werden immer vom OMS-Agent an OMS übertragen, entweder direkt oder über das OMS-Gateway.

![Service Map-Agents](media/oms-service-map/agents.png)

Wenn Sie ein SCOM-Kunde mit einer Verwaltungsgruppe sind, die mit OMS verbunden ist:

- Wenn die SCOM-Agents die Verbindung mit OMS über das Internet herstellen können, ist keine weitere Konfiguration erforderlich.  
- Wenn die SCOM-Agents nicht über das Internet auf OMS zugreifen können, müssen Sie das OMS-Gateway für SCOM konfigurieren.
  
Wenn Sie den Direct Agent von OMS verwenden, müssen Sie den OMS-Agent für die Herstellung der Verbindung mit OMS oder mit dem OMS-Gateway konfigurieren.  Sie können das OMS-Gateway von [https://www.microsoft.com/en-us/download/details.aspx?id=52666](https://www.microsoft.com/en-us/download/details.aspx?id=52666) herunterladen.


### <a name="avoiding-duplicate-data"></a>Vermeiden von doppelten Daten

Wenn Sie ein SCOM-Kunde sind, sollten Sie die SCOM-Agents nicht für die direkte Kommunikation mit OMS konfigurieren, da andernfalls die Daten zweimal in Berichten erfasst werden.  In Service Map führt dies dazu, dass Computer zweimal in der Computerliste aufgeführt werden.

OMS sollte nur an einer der folgenden Stellen konfiguriert werden:

- In der SCOM-Konsole im Bereich „Operations Management Suite“ für verwaltete Computer
- In den MMA-Eigenschaften der Konfiguration von Azure Operational Insights

Wenn beide Konfigurationen mit jeweils dem *gleichen* Arbeitsbereich verwendet werden, führt dies zur Duplizierung von Daten. Wenn beide Konfigurationen mit *unterschiedlichen* Arbeitsbereichen verwendet werden, kann dies zu einem Konflikt zwischen den Konfigurationen führen (Service Map in einer Konfiguration aktiviert und in der anderen Konfiguration nicht aktiviert), der bewirkt, dass Daten nicht vollständig nach Service Map übertragen werden.  

Auch wenn der Computer selbst in der OMS-Konfiguration der SCOM-Konsole nicht angegeben ist, kann der Computer die OMS-Konfiguration per SCOM empfangen, wenn eine Instanzengruppe, z.B. „Gruppe der Windows Server-Instanzen“, aktiv ist.



## <a name="management-packs"></a>Management Packs
Wenn Service Map in einem OMS-Arbeitsbereich aktiviert ist, wird an alle Microsoft Monitoring Agents in diesem Arbeitsbereich ein Management Pack von 300KB gesendet.  Wenn Sie SCOM-Agents in einer [verbundenen Verwaltungsgruppe](../log-analytics/log-analytics-om-agents.md) verwenden, wird das Service Map-Management Pack von SCOM bereitgestellt.  Wenn die Agents direkt verbunden sind, wird das Management Pack von OMS bereitgestellt.

Der Name des Management Packs lautet „Microsoft.IntelligencePacks.ApplicationDependencyMonitor“*.  Es wird in das Verzeichnis *%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\* geschrieben.  Die vom Management Pack verwendete Datenquelle lautet *%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll*.



## <a name="configuration"></a>Konfiguration
Auf Computern mit Windows oder Linux muss nicht nur ein Agent installiert und mit OMS verbunden sein, sondern Sie müssen auch das Dependency-Agent-Installationsprogramm von der Service Map-Lösung herunterladen und auf jedem verwalteten Server als Administrator bzw. mit Root-Berechtigungen installieren.  Nachdem der Service Map-Agent auf einem Server installiert wurde, der Berichte an OMS sendet, werden innerhalb von 10 Minuten Service Map-Abhängigkeitsstrukturen angezeigt.


### <a name="migrating-from-bluestripe-factfinder"></a>Migrieren von BlueStripe FactFinder
Service Map stellt BlueStripe-Technologie phasenweise in OMS bereit. FactFinder wird für Bestandskunden weiterhin unterstützt, ist jedoch nicht mehr käuflich erhältlich.  Diese Vorschauversion des Dependency-Agent kann nur mit OMS kommunizieren.  Wenn Sie derzeit FactFinder-Kunde sind, bestimmen Sie eine Gruppe von Testservern für Service Map, die nicht von FactFinder verwaltet werden. 

### <a name="download-the-dependency-agent"></a>Herunterladen des Dependency-Agent
Auf allen Computern, die von Service Map analysiert werden, muss zusätzlich zum Microsoft-Verwaltungs-Agent (Microsoft Management Agent, MMA) und OMS-Linux-Agent, die die Verbindung zwischen dem Computer und OMS bereitstellen, der Dependency-Agent installiert sein.  Unter Linux muss vor dem Dependency-Agent der OMS-Agent für Linux installiert werden. 

![Service Map-Kachel](media/oms-service-map/additional_configuration.png)

Um den Dependency-Agent herunterzuladen, klicken Sie in der Kachel **Service Map** auf **Lösung konfigurieren**, um das Blatt **Dependency-Agent** zu öffnen.  Das Blatt „Dependency-Agent“ enthält Links für den Windows-Agent und Linux-Agent. In den folgenden Abschnitten finden Sie Informationen zum Installieren des Agent auf unterschiedlichen Systemen.

### <a name="install-the-dependency-agent"></a>Installieren des Dependency-Agent

#### <a name="microsoft-windows"></a>Microsoft Windows
Zum Installieren oder Deinstallieren des Agent sind Administratorrechte erforderlich.

Der Dependency-Agent wird auf Windows-Computern mithilfe von „InstallDependencyAgent-Windows.exe“ installiert. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Assistent gestartet, mit dem Sie die Installation interaktiv ausführen können.  

Führen Sie die folgenden Schritte aus, um den Dependency-Agent auf jedem Windows-Computer zu installieren:

1.    Stellen Sie mithilfe der Anweisungen zum direkten Verbinden von Computern mit OMS sicher, dass der OMS-Agent installiert ist.
2.    Laden Sie den Windows-Agent herunter, und starten Sie ihn mit dem folgenden Befehl: <br>*InstallDependencyAgent-Windows.exe*
3.    Folgen Sie den Anweisungen des Assistenten, um den Assistenten zu installieren.
4.    Wenn der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen ausführliche Fehlerinformationen. Für Windows-Agents lautet das Protokollverzeichnis *C:\Programme\Microsoft Dependency Agent\logs*. 

Der Dependency-Agent für Windows kann von einem Administrator über die Systemsteuerung deinstalliert werden.


#### <a name="linux"></a>Linux
Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.

Der Dependency-Agent wird auf Linux-Computern mit „InstallDependencyAgent-Linux64.bin“ installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit „sh“ ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.
 
Führen Sie die folgenden Schritte aus, um den Dependency-Agent auf jedem Linux-Computer zu installieren:

1.    Stellen Sie mithilfe der Anweisungen unter [Verbinden von Linux-Computern mit Log Analytics  ](https://technet.microsoft.com/library/mt622052.aspx) sicher, dass der OMS-Agent installiert ist. Der OMS-Agent muss vor dem Dependency-Agent für Linux installiert werden.
2.    Installieren Sie mit Root-Berechtigungen den Dependency-Agent für Linux mit dem folgenden Befehl:<br>*sh InstallDependencyAgent-Linux64.bin*.
3.    Wenn der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen ausführliche Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis */var/opt/microsoft/dependency-agent/log*.

### <a name="uninstalling-the-dependency-agent-on-linux"></a>Deinstallieren des Dependency-Agent für Linux
Wenn Sie den Dependency-Agent unter Linux vollständig deinstallieren möchten, müssen Sie den Agent selbst und den Connector entfernen, der automatisch mit dem Agent installiert wird.  Mit dem folgenden einzelnen Befehl können Sie beide Komponenten deinstallieren:

    rpm -e dependency-agent dependency-agent-connector


### <a name="installing-from-a-command-line"></a>Installieren über die Befehlszeile
Der vorherige Abschnitt enthält Hinweise zum Installieren des Dependency-Agent mit den Standardoptionen.  In den folgenden Abschnitten wird erläutert, wie der Agent mit benutzerdefinierten Optionen über die Befehlszeile installiert wird.

#### <a name="windows"></a>Windows
Verwenden Sie Optionen aus der folgenden Tabelle, um über die Befehlszeile zu installieren. Um eine Liste der Installationsflags anzuzeigen, führen Sie das Installationsprogramm wie folgt mit dem Flag „/?“ aus.

    InstallDependencyAgent-Windows.exe /?

| Flag | Beschreibung |
|:--|:--|
| /S | Führt eine automatische Installation ohne Benutzereingaben aus. |

Der Standardspeicherort von Dateien des Dependency-Agent für Windows lautet *C:\Programme\Microsoft Dependency Agent*.


#### <a name="linux"></a>Linux
Verwenden Sie Optionen aus der folgenden Tabelle zur Installation. Um eine Liste der Installationsflags anzuzeigen, führen Sie das Installationsprogramm wie folgt mit dem Flag „-help“ aus.

    InstallDependencyAgent-Linux64.bin -help

| Flag    Beschreibung
|:--|:--|
| -s | Führt eine automatische Installation ohne Benutzereingaben aus. |
| --check | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Dateien für den Dependency-Agent befinden sich in den folgenden Verzeichnissen:

| Dateien | Standort |
|:--|:--|
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |



## <a name="troubleshooting"></a>Problembehandlung
Wenn bei Service Map Probleme auftreten, können Sie anhand der folgenden Angaben Problembehandlungsinformationen aus mehreren Komponenten sammeln.

### <a name="windows-agents"></a>Windows-Agents

#### <a name="microsoft-dependency-agent"></a>Microsoft Dependency-Agent
Um Problembehandlungsdaten aus dem Dependency-Agent zu generieren, öffnen Sie als Administrator eine Eingabeaufforderung, und führen Sie mit dem folgenden Befehl das Skript „CollectDependencyAgentData.vbs“ aus.  Sie können das Flag „--help“ hinzufügen, um zusätzliche Optionen anzuzeigen.

    cd C:\Program Files\Microsoft Dependency Agent\scripts
    cscript CollectDependencyData.vbs

Das Supportdatenpaket wird im Verzeichnis „%USERPROFILE%“ für den aktuellen Benutzer gespeichert.  Mit der Option „--file <filename>“ können Sie das Paket in einem anderen Verzeichnis speichern.

#### <a name="microsoft-dependency-agent-management-pack-for-mma"></a>Microsoft Dependency-Agent-Management Pack für MMA
Das Dependency-Agent-Management Pack wird im Microsoft-Verwaltungs-Agent ausgeführt.  Es empfängt Daten vom Dependency-Agent und leitet sie an den Service Map-Clouddienst weiter.
  
Überprüfen Sie mit den folgenden Schritten, ob das Management Pack heruntergeladen wurde:

1.    Suchen Sie in „C:\Programme\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs“ nach der Datei „Microsoft.IntelligencePacks.ApplicationDependencyMonitor.mp“.  
2.    Wenn die Datei nicht vorhanden ist und der Agent mit einer SCOM-Verwaltungsgruppe verbunden ist, überprüfen Sie, ob sie in SCOM importiert wurde. Überprüfen Sie hierzu „Management Packs“ im Arbeitsbereich „Administration“ der Operations Manager-Konsole.

Das Service Map-Management Pack schreibt Ereignisse in das Windows-Ereignisprotokoll von Operations Manager.  Das Protokoll kann über das Systemprotokoll, in dem Sie festlegen können, welche Protokolldateien hochgeladen werden sollen, [in OMS gesucht](../log-analytics/log-analytics-log-searches.md) werden.  Wenn Debugereignisse aktiviert sind, werden sie in das Anwendungsereignisprotokoll geschrieben, wobei die Ereignisquelle *ADMConnector* lautet.

#### <a name="microsoft-monitoring-agent"></a>Microsoft Monitoring Agent
Öffnen Sie eine Eingabeaufforderung als Administrator, und führen Sie den folgenden Befehl aus, um Diagnoseablaufverfolgungen zu sammeln: 

    cd \Program Files\Microsoft Monitoring Agent\Agent\Tools
    net stop healthservice 
    StartTracing.cmd ERR
    net start healthservice

Ablaufverfolgungen werden in „C:\Windows\Logs\OpsMgrTrace“ geschrieben.  Mit „StopTracing.cmd“ können Sie die Ablaufverfolgung beenden.


### <a name="linux-agents"></a>Linux-Agents

#### <a name="microsoft-dependency-agent"></a>Microsoft Dependency-Agent
Melden Sie sich mit einem Konto an, das über sudo- oder Root-Berechtigungen verfügt, und führen Sie den folgenden Befehl aus, um Problembehandlungsdaten aus dem Dependency-Agent zu generieren.  Sie können das Flag „--help“ hinzufügen, um zusätzliche Optionen anzuzeigen.

    /opt/microsoft/dependency-agent/lib/scripts/collect-dependency-agent-data.sh

Das Supportdatenpaket wird unter dem Installationsverzeichnis des Agent in „/var/opt/microsoft/dependency-agent/log“ (wenn der Benutzer über Root-Berechtigungen verfügt) oder im Basisverzeichnis des Benutzers, der das Skript ausführt (wenn der Benutzer nicht über Root-Berechtigungen verfügt), gespeichert.  Mit der Option „--file <filename>“ können Sie das Paket in einem anderen Verzeichnis speichern.

#### <a name="microsoft-dependency-agent-fluentd-plug-in-for-linux"></a>Microsoft Dependency-Agent-Fluentd-Plug-In für Linux
Das Dependency-Agent-Fluentd-Plug-In wird im OMS-Agent für Linux ausgeführt.  Es empfängt Daten vom Dependency-Agent und leitet sie an den Service Map-Clouddienst weiter.  

Protokolle werden in die folgenden beiden Dateien geschrieben.

- /var/opt/microsoft/omsagent/log/omsagent.log
- /var/log/messages

#### <a name="oms-agent-for-linux"></a>OMS-Agent für Linux
Die folgende Webseite bietet eine Problembehandlungsressource zum Verbinden von Linux-Servern mit OMS: [https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md](https://github.com/Microsoft/OMS-Agent-for-Linux/blob/master/docs/Troubleshooting.md) 

Die Protokolle für den OMS-Agent für Linux befinden sich in */var/opt/microsoft/omsagent/log/*.  

Die Protokolle für omsconfig (Agent-Konfiguration) befinden sich in */var/opt/microsoft/omsconfig/log/*.
 
Die Protokolle für die Komponenten OMI und SCX, die Daten von Leistungsmetriken liefern, befinden sich in */var/opt/omi/log/* und */var/opt/microsoft/scx/log*.


## <a name="data-collection"></a>Datensammlung
Je nach Komplexität der Systemabhängigkeiten können Sie davon ausgehen, dass jeder Agent etwa 25MB pro Tag überträgt.  Die Service Map-Abhängigkeitsdaten werden von jedem Agent alle 15 Sekunden gesendet.  

Vom Dependency-Agent werden i.d.R. 0,1% des Systemspeichers und 0,1% der System-CPU-genutzt.


## <a name="supported-operating-systems"></a>Unterstützte Betriebssysteme
In den folgenden Abschnitten sind die unterstützten Betriebssysteme für den Dependency-Agent aufgeführt.   32-Bit-Architekturen werden für kein Betriebssystem unterstützt.

### <a name="windows-server"></a>Windows Server
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Windows Desktop
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux und Oracle Linux (mit RHEL-Kernel)
- Es werden nur die Standardversion und SMP-Version des Linux-Kernels unterstützt.
- Nicht-Standardversionen des Kernels, z. B. PAE und Xen, werden für keine Linux-Distribution unterstützt. Beispielsweise wird ein System mit der Versionszeichenfolge „2.6.16.21-0.8-xen“ nicht unterstützt.
- Benutzerdefinierte Kernels, einschließlich Neukompilierungen von Standardkernels, werden nicht unterstützt.
- Der CentOSPlus-Kernel wird nicht unterstützt.
- Der Unbreakable Enterprise Kernel (UEK) für Oracle Linux wird in einem Abschnitt weiter unten erläutert.


#### <a name="red-hat-linux-7"></a>Red Hat Linux 7
| Betriebssystemversion | Kernelversion |
|:--|:--|
| 7.0 | 3.10.0-123 |
| 7,1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6
| Betriebssystemversion | Kernelversion |
|:--|:--|
| 6,0 | 2.6.32-71 |
| 6.1 | 2.6.32-131 |
| 6.2 | 2.6.32-220 |
| 6.3 | 2.6.32-279 |
| 6.4. | 2.6.32-358 |
| 6,5 | 2.6.32-431 |
| 6.6 | 2.6.32-504 |
| 6.7 | 2.6.32-573 |
| 6,8 | 2.6.32-642 |

#### <a name="red-hat-linux-5"></a>Red Hat Linux 5
| Betriebssystemversion | Kernelversion |
|:--|:--|
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398<br>2.6.18-400<br>2.6.18-402<br>2.6.18-404<br>2.6.18-406<br>2.6.18-407<br>2.6.18-408<br>2.6.18-409<br>2.6.18-410<br>2.6.18-411<br>2.6.18-412<br>2.6.18-416<br>2.6.18-417 |

#### <a name="oracle-enterprise-linux-w-unbreakable-kernel-uek"></a>Oracle Enterprise Linux mit Unbreakable Enterprise Kernel (UEK)

#### <a name="oracle-linux-6"></a>Oracle Linux 6
| Betriebssystemversion | Kernelversion
|:--|:--|
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4. | Oracle 2.6.39-400 (UEK R2) |
| 6,5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| Betriebssystemversion | Kernelversion
|:--|:--|
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11
| Betriebssystemversion | Kernelversion
|:--|:--|
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10
| Betriebssystemversion | Kernelversion
|:--|:--|
| 10 SP4 | 2.6.16.60 |

## <a name="diagnostic-and-usage-data"></a>Diagnose- und Nutzungsdaten
Wenn Sie den Service Map-Dienst verwenden, sammelt Microsoft automatisch Nutzungs- und Leistungsdaten. Microsoft verwendet diese Daten, um die Qualität, Sicherheit und Integrität des Service Map-Diensts sicherzustellen und zu verbessern. Zu den Daten gehören Informationen zur Konfiguration Ihrer Software, z.B. Betriebssystem und Betriebssystemversion, sowie IP-Adresse, DNS-Name und Name der Arbeitsstation, um exakte und effiziente Funktionen für die Problembehandlung bereitzustellen. Wir erfassen weder Namen noch Adressen oder andere Kontaktinformationen.

Weitere Informationen zur Sammlung und Nutzung von Daten finden Sie in den [Datenschutzbestimmungen für Onlinedienste von Microsoft](https://go.microsoft.com/fwlink/?LinkId=512132).



## <a name="next-steps"></a>Nächste Schritte
- Erfahren Sie, wie Sie nach Bereitstellung und Konfiguration [Service Map verwenden](operations-management-suite-service-map.md) können.

