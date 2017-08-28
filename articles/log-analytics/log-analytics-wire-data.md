---
title: "Wire Data-Lösung in Log Analytics | Microsoft Docs"
description: "Bei Wire Data handelt es sich um zusammengefasste Netzwerk- und Leistungsdaten von Computern mit OMS-Agents, z.B. Operations Manager und Agents mit Windows-Verbindung. Netzwerkdaten werden mit Ihren Protokolldaten kombiniert, um Ihnen das Korrelieren von Daten zu ermöglichen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: banders
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: ce8065d777bb315d4f9589d1b24a5152296facfe
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---

# <a name="wire-data-20-preview-solution-in-log-analytics"></a>Wire Data 2.0-Lösung (Vorschauversion) in Log Analytics

![Wire Data-Symbol](./media/log-analytics-wire-data/wire-data2-symbol.png)

Bei Wire Data handelt es sich um zusammengefasste Netzwerk- und Leistungsdaten von Computern mit OMS-Agents, z.B. Operations Manager, Agents mit Windows-Verbindung und Linux-Agents. Netzwerkdaten werden mit Ihren sonstigen Protokolldaten kombiniert, um Ihnen das Korrelieren von Daten zu ermöglichen.

Zusätzlich zu den OMS-Agents verwendet die Wire Data-Lösung Microsoft Dependency-Agents, die Sie auf Computern in Ihrer IT-Infrastruktur installieren. Dependency-Agents überwachen Netzwerkdaten, die für die Netzwerkschichten 2 und 3 des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model) an ihre Computer und von ihnen gesendet werden, z.B. die verschiedenen verwendeten Protokolle und Ports. Die Daten werden dann mit Agents an Log Analytics gesendet.

> [!NOTE]
> Sie können die vorherige Version der Wire Data-Lösung nicht neuen Arbeitsbereichen hinzufügen. Wenn Sie die ursprüngliche Wire Data-Lösung aktiviert haben, können Sie diese Lösung weiterhin verwenden. Um jedoch Wire Data 2.0 zu verwenden, müssen Sie zuerst die ursprüngliche Version entfernen.

Standardmäßig erfasst Log Analytics protokollierte Daten für CPU, Arbeitsspeicher, Datenträger und Netzwerk von Indikatoren, die in Windows integriert sind. Die Erfassung von Netzwerkdaten und anderen Daten wird für jeden Agent in Echtzeit durchgeführt, einschließlich der vom Computer verwendeten Subnetze und Anwendungsebenenprotokolle. Auf der Seite „Einstellungen“ können Sie auf der Registerkarte „Protokolle“ weitere Leistungsindikatoren hinzufügen.

Wenn Sie [sFlow](http://www.sflow.org/) oder andere Software mit dem [NetFlow-Protokoll von Cisco](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html) verwendet haben, werden Ihnen die in den Daten zur Kommunikation angezeigten Statistiken und Daten bekannt vorkommen.

Beispiele für die Typen von integrierten Abfragen von Protokollsuchen:

- Agents, die Wire Data bereitstellen
- IP-Adresse der Agents, die Wire Data bereitstellen
- Ausgehende Kommunikation über IP-Adressen
- Anzahl von gesendeten Bytes nach Anwendungsprotokollen
- Anzahl von gesendeten Bytes eines Anwendungsdiensts
- Von unterschiedlichen Protokollen empfangene Bytes
- Gesamte Bytes, die von einer IP-Version gesendet und empfangen wurden
- Durchschnittliche Latenz für Verbindungen mit zuverlässiger Messung
- Computerprozesse, die Netzwerk-Datenverkehr initiiert oder empfangen haben
- Menge des Netzwerk-Datenverkehrs für einen Prozess

Wenn Sie mit Wire Data eine Suche durchführen, können Sie Daten filtern und gruppieren, um Informationen zu den wichtigsten Agents und Protokollen anzuzeigen. Außerdem können Sie sich ansehen, wann bestimmte Computer (IP-Adressen/MAC-Adressen) miteinander kommuniziert haben, wie lange dies gedauert hat und wie viele Daten gesendet wurden. Im Grunde genommen zeigen Sie die Metadaten zum Netzwerkdatenverkehr an. Dies ist ein suchbasierter Vorgang.

Da Sie Metadaten anzeigen, ist der Vorgang nicht unbedingt gut für die eingehende Problembehandlung geeignet. Bei Wire Data in Log Analytics wird keine vollständige Erfassung aller Netzwerkdaten durchgeführt. Die eingehende Problembehandlung auf Paketebene ist also nicht vorgesehen. Der Vorteil einer Verwendung des Agents gegenüber anderen Erfassungsmethoden besteht darin, dass Sie keine Appliances installieren, Netzwerkswitches neu konfigurieren oder komplizierten Konfigurationen durchführen müssen. Wire Data ist einfach Agent-basiert. Sie installieren den Agent auf einem Computer, um damit den Netzwerkdatenverkehr zu überwachen. Ein weiterer Vorteil ergibt sich, wenn Sie Workloads überwachen möchten, die bei Cloudanbietern, Hostinganbietern oder unter Microsoft Azure ausgeführt werden und bei denen sich die Fabric-Ebene nicht im Besitz des Benutzers befindet.

## <a name="connected-sources"></a>Verbundene Quellen

Wire Data ruft ihre Daten vom Microsoft Dependency-Agent ab. Der Dependency-Agent ist abhängig vom OMS-Agent, da er dessen Verbindungen mit Log Analytics benötigt. Dies bedeutet, dass auf einem Server zunächst der OMS-Agent installiert und konfiguriert werden muss, bevor der Dependency-Agent installiert werden kann. In der folgenden Tabelle sind die verbundenen Quellen beschrieben, die von Wire Data unterstützt werden.

| **Verbundene Quelle** | **Unterstützt** | **Beschreibung** |
| --- | --- | --- |
| Windows-Agents | Ja | Wire Data analysiert und erfasst Daten von Windows-Agent-Computern. <br><br> Zusätzlich zum [OMS-Agent](log-analytics-windows-agents.md) erfordern Windows-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems). |
| Linux-Agents | Ja | Wire Data analysiert und erfasst Daten von Linux-Agent-Computern.<br><br> Zusätzlich zum [OMS-Agent](log-analytics-linux-agents.md) erfordern Linux-Agents den Microsoft Dependency-Agent. Eine vollständige Liste der Betriebssystemversionen finden Sie unter [Unterstützte Betriebssysteme](../operations-management-suite/operations-management-suite-service-map-configure.md#supported-operating-systems). |
| System Center Operations Manager-Verwaltungsgruppe | Ja | Wire Data analysiert und erfasst Daten von Windows- und Linux-Agents in einer verbundenen [System Center Operations Manager-Verwaltungsgruppe](log-analytics-om-agents.md). <br><br> Es ist eine direkte Verbindung des System Center Operations Manager-Agents mit Log Analytics erforderlich. Daten werden von der Verwaltungsgruppe an Log Analytics weitergeleitet. |
| Azure-Speicherkonto | Nein | Da Wire Data Daten von Agent-Computern erfasst, sind keine Daten aus Azure Storage zu erfassen. |

Unter Windows wird der Microsoft Monitoring Agent (MMA) von System Center Operations Manager und Log Analytics zum Erfassen und Senden von Daten verwendet. Dieser Agent wird je nach Kontext als System Center Operations Manager-Agent, OMS-Agent, Log Analytics-Agent, MMA oder Direkt-Agent bezeichnet. System Center Operations Manager und Log Analytics bieten leicht unterschiedliche Versionen der MMA. Jede dieser Versionen kann Berichte an System Center Operations Manager, Log Analytics oder beide senden.

Unter Linux erfasst der OMS-Agent für Linux Daten und sendet sie an Log Analytics. Sie können Wire Data auf Servern mit direkten OMS-Agents oder auf Servern verwenden, die über System Center Operations Manager-Verwaltungsgruppen Log Analytics zugeordnet sind.

In diesem Artikel werden alle Agents – ob unter Linux oder Windows, ob mit einer System Center Operations Manager-Verwaltungsgruppe oder direkt mit Log Analytics verbunden – als _OMS-Agent_ bezeichnet. Der spezielle Bereitstellungsname des Agents wird nur verwendet, wenn es der jeweilige Kontext erfordert.

Der Dependency-Agent selbst überträgt keine Daten und erfordert keine Änderungen an Firewalls oder Ports. Die Daten in Wire Data werden immer vom OMS-Agent an Log Analytics übertragen, entweder direkt oder über das OMS-Gateway.

![Agent-Diagramm](./media/log-analytics-wire-data/agents.png)

Wenn Sie ein System Center Operations Manager-Benutzer mit einer Verwaltungsgruppe sind, die mit Log Analytics verbunden ist:

- Wenn die System Center Operations Manager-Agents die Verbindung mit Log Analytics über das Internet herstellen können, ist keine weitere Konfiguration erforderlich.
- Wenn die System Center Operations Manager-Agents nicht über das Internet auf Log Analytics zugreifen können, müssen Sie das OMS-Gateway für System Center Operations Manager konfigurieren.

Wenn Sie den Direkt-Agent verwenden, müssen Sie den OMS-Agent für die Herstellung der Verbindung mit Log Analytics oder dem OMS-Gateway konfigurieren. Sie können OMS-Gateway vom [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=52666) herunterladen.

## <a name="prerequisites"></a>Voraussetzungen

- Das [Insight & Analytics](https://www.microsoft.com/cloud-platform/operations-management-suite-pricing)-Lösungsangebot ist erforderlich.
- Wenn Sie die vorherige Version der Wire Data-Lösung verwenden, müssen Sie sie zuerst entfernen. Alle über die ursprüngliche Wire Data-Lösung erfassten Daten stehen jedoch noch in Wire Data 2.0 und der Protokollsuche zur Verfügung.
- Zum Installieren oder Deinstallieren des Dependency-Agents sind Administratorrechte erforderlich.
- Der Dependency-Agent muss auf einem Computer mit einem 64-Bit-Betriebssystem installiert werden.

### <a name="operating-systems"></a>Betriebssysteme

In den folgenden Abschnitten sind die unterstützten Betriebssysteme für den Dependency-Agent aufgeführt. 32-Bit-Architekturen werden von Wire Data für kein Betriebssystem unterstützt.

#### <a name="windows-server"></a>Windows Server

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

#### <a name="windows-desktop"></a>Windows Desktop

- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

#### <a name="red-hat-enterprise-linux-centos-linux-and-oracle-linux-with-rhel-kernel"></a>Red Hat Enterprise Linux, CentOS Linux und Oracle Linux (mit RHEL-Kernel)

- Es werden nur die Standardversion und SMP-Version des Linux-Kernels unterstützt.
- Nicht-Standardversionen des Kernels, z.B. PAE und Xen, werden für keine Linux-Distribution unterstützt. Beispielsweise wird ein System mit der Versionszeichenfolge _2.6.16.21-0.8-xen_ nicht unterstützt.
- Benutzerdefinierte Kernels, einschließlich Neukompilierungen von Standardkernels, werden nicht unterstützt.
- Der CentOSPlus-Kernel wird nicht unterstützt.
- Der Unbreakable Enterprise Kernel (UEK) für Oracle wird in einem Abschnitt weiter unten erläutert.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 7.0 | 3.10.0-123 |
| 7,1 | 3.10.0-229 |
| 7.2 | 3.10.0-327 |
| 7.3 | 3.10.0-514 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
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

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 5.8 | 2.6.18-308 |
| 5.9 | 2.6.18-348 |
| 5.10 | 2.6.18-371 |
| 5.11 | 2.6.18-398 <br> 2.6.18-400 <br>2.6.18-402 <br>2.6.18-404 <br>2.6.18-406 <br> 2.6.18-407 <br> 2.6.18-408 <br> 2.6.18-409 <br> 2.6.18-410 <br> 2.6.18-411 <br> 2.6.18-412 <br> 2.6.18-416 <br> 2.6.18-417 <br> 2.6.18-419 |

#### <a name="oracle-enterprise-linux-with-unbreakable-enterprise-kernel"></a>Oracle Enterprise Linux mit Unbreakable Enterprise Kernel

#### <a name="oracle-linux-6"></a>Oracle Linux 6

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 6.2 | Oracle 2.6.32-300 (UEK R1) |
| 6.3 | Oracle 2.6.39-200 (UEK R2) |
| 6.4. | Oracle 2.6.39-400 (UEK R2) |
| 6,5 | Oracle 2.6.39-400 (UEK R2 i386) |
| 6.6 | Oracle 2.6.39-400 (UEK R2 i386) |

#### <a name="oracle-linux-5"></a>Oracle Linux 5

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 5.8 | Oracle 2.6.32-300 (UEK R1) |
| 5.9 | Oracle 2.6.39-300 (UEK R2) |
| 5.10 | Oracle 2.6.39-400 (UEK R2) |
| 5.11 | Oracle 2.6.39-400 (UEK R2) |

#### <a name="suse-linux-enterprise-server"></a>SUSE Linux Enterprise Server

#### <a name="suse-linux-11"></a>SUSE Linux 11

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 11 | 2.6.27 |
| 11 SP1 | 2.6.32 |
| 11 SP2 | 3.0.13 |
| 11 SP3 | 3.0.76 |
| 11 SP4 | 3.0.101 |

#### <a name="suse-linux-10"></a>SUSE Linux 10

| **Betriebssystemversion** | **Kernelversion** |
| --- | --- |
| 10 SP4 | 2.6.16.60 |

#### <a name="dependency-agent-downloads"></a>Dependency-Agent – Downloads

| **Datei** | **Betriebssystem** | **Version** | **SHA-256** |
| --- | --- | --- | --- |
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.0.5 | 73B3F6A2A76A08D58F72A550947FF839B588591C48E6EDDD6DDF73AA3FD82B43 |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.0.5 | A1BAD0B36EBF79F2B69113A07FCF48C68D90BD169C722689F9C83C69FC032371 |



## <a name="configuration"></a>Konfiguration

Führen Sie die folgenden Schritte aus, um die Wire Data-Lösung für Ihre Arbeitsbereiche zu konfigurieren.

1. Aktivieren Sie die Log Analytics-Lösung für Aktivitätsprotokolle in [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.WireData2OMS?tab=Overview) oder entsprechend den unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Schritten.
2. Installieren Sie den Dependency-Agent auf jedem Computer, von dem Sie Daten abrufen möchten. Der Dependency-Agent kann Verbindungen zu unmittelbaren Nachbarn überwachen, sodass Sie nicht auf jedem Computer einen Agent benötigen.

### <a name="install-the-dependency-agent-on-windows"></a>Installieren des Dependency-Agents unter Windows

Zum Installieren oder Deinstallieren des Agent sind Administratorrechte erforderlich.

Der Dependency-Agent wird auf Windows-Computern mithilfe von „InstallDependencyAgent-Windows.exe“ installiert. Wenn Sie diese ausführbare Datei ohne Optionen ausführen, wird ein Assistent gestartet, mit dem Sie die Installation interaktiv ausführen können.

Führen Sie die folgenden Schritte aus, um den Dependency-Agent auf jedem Windows-Computer zu installieren:

1. Stellen Sie mithilfe der Anweisungen unter [Verbinden von Windows-Computern mit dem Log Analytics-Dienst in Azure](log-analytics-windows-agents.md) sicher, dass der OMS-Agent installiert ist.
2. Laden Sie den Windows-Agent über den Link aus dem vorherigen Abschnitt herunter, und führen Sie ihn dann mithilfe des folgenden Befehls aus: „InstallDependencyAgent-Windows.exe“.
3. Folgen Sie den Anweisungen des Assistenten, um den Assistenten zu installieren.
4. Wenn der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen ausführliche Fehlerinformationen. Für Windows-Agents lautet das Protokollverzeichnis „%Programfiles%\Microsoft Dependency Agent\logs“.

#### <a name="windows-command-line"></a>Windows-Befehlszeile

Verwenden Sie Optionen aus der folgenden Tabelle, um über die Befehlszeile zu installieren. Um eine Liste der Installationsflags anzuzeigen, führen Sie das Installationsprogramm wie folgt mit dem Flag „/?“ aus.

InstallDependencyAgent-Windows.exe /?

| **Flag** | **Beschreibung** |
| --- | --- |
| <code>/?</code> | Ruft eine Liste der Befehlszeilenoptionen ab. |
| <code>/S</code> | Führt eine automatische Installation ohne Benutzereingaben aus. |

Der Standardspeicherort von Dateien des Dependency-Agents für Windows lautet „C:\Programme\Microsoft Dependency Agent“.

### <a name="install-the-dependency-agent-on-linux"></a>Installieren des Dependency-Agents unter Linux

Zum Installieren oder Konfigurieren des Agent ist Root-Zugriff erforderlich.

Der Dependency-Agent wird auf Linux-Computern mit „InstallDependencyAgent-Linux64.bin“ installiert, einem Shellskript mit einer selbstextrahierenden Binärdatei. Sie können die Datei mit _sh_ ausführen oder der Datei selbst Ausführungsberechtigungen hinzufügen.

Führen Sie die folgenden Schritte aus, um den Dependency-Agent auf jedem Linux-Computer zu installieren:

1. Installieren Sie den AMS-Agent mithilfe der Anweisungen unter [Verbinden Ihrer Linux-Computer mit Operations Management Suite (OMS)](log-analytics-agent-linux.md).
2. Laden Sie den Linux-Dependency-Agent über den Link aus dem vorherigen Abschnitt herunter, und installieren Sie ihn dann mithilfe des folgenden Befehls als Root: „InstallDependencyAgent-Linux64.bin“.
3. Wenn der Dependency-Agent nicht gestartet wird, suchen Sie in den Protokollen ausführliche Fehlerinformationen. Für Linux-Agents lautet das Protokollverzeichnis: „/var/opt/microsoft/dependency-agent/log“.

Um eine Liste der Installationsflags anzuzeigen, führen Sie das Installationsprogramm wie folgt mit dem `-help`-Flag aus.

```
InstallDependencyAgent-Linux64.bin -help
```

| **Flag** | **Beschreibung** |
| --- | --- |
| <code>-help</code> | Ruft eine Liste der Befehlszeilenoptionen ab. |
| <code>-s</code> | Führt eine automatische Installation ohne Benutzereingaben aus. |
| <code>--check</code> | Überprüft Berechtigungen und das Betriebssystem, ohne den Agent zu installieren. |

Dateien für den Dependency-Agent befinden sich in den folgenden Verzeichnissen:

| **Dateien** | **Standort** |
| --- | --- |
| Hauptdateien | /opt/microsoft/dependency-agent |
| Protokolldateien | /var/opt/microsoft/dependency-agent/log |
| Konfigurationsdateien | /etc/opt/microsoft/dependency-agent/config |
| Ausführbare Dienstdateien | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br><br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Binäre Speicherdateien | /var/opt/microsoft/dependency-agent/storage |

### <a name="installation-script-examples"></a>Beispiele für Installationsskripts

Der Dependency-Agent kann mithilfe eines Skripts auf mehreren Servern gleichzeitig bereitgestellt werden. Sie können die folgenden Skriptbeispiele verwenden, um den Dependency-Agent herunterzuladen und unter Windows oder Linux zu installieren.

#### <a name="powershell-script-for-windows"></a>PowerShell-Skript für Windows

```PowerShell

Invoke-WebRequest &quot;https://aka.ms/dependencyagentwindows&quot; -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S

```

#### <a name="shell-script-for-linux"></a>Shellskript für Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
```

```
sh InstallDependencyAgent-Linux64.bin -s
```

### <a name="desired-state-configuration"></a>Desired State Configuration

Zum Bereitstellen des Dependency-Agents über Desired State Configuration können Sie das Modul „xPSDesiredStateConfiguration“ und ein paar Codezeilen wie die folgenden verwenden:

```
Import-DscResource -ModuleName xPSDesiredStateConfiguration

$DAPackageLocalPath = &quot;C:\InstallDependencyAgent-Windows.exe&quot;



Node $NodeName

{

    # Download and install the Dependency Agent

    xRemoteFile DAPackage

    {

        Uri = &quot;https://aka.ms/dependencyagentwindows&quot;

        DestinationPath = $DAPackageLocalPath

        DependsOn = &quot;[Package]OI&quot;

    }

    xPackage DA

    {

        Ensure=&quot;Present&quot;

        Name = &quot;Dependency Agent&quot;

        Path = $DAPackageLocalPath

        Arguments = '/S'

        ProductId = &quot;&quot;

        InstalledCheckRegKey = &quot;HKEY\_LOCAL\_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent&quot;

        InstalledCheckRegValueName = &quot;DisplayName&quot;

        InstalledCheckRegValueData = &quot;Dependency Agent&quot;

    }

}

```
### <a name="uninstall-the-dependency-agent"></a>Deinstallieren des Dependency-Agents

In den folgenden Abschnitten erfahren Sie, wie Sie den Dependency-Agent entfernen können.

#### <a name="uninstall-the-dependency-agent-on-windows"></a>Deinstallieren des Dependency-Agents unter Windows

Ein Administrator kann den Dependency-Agent für Windows über die Systemsteuerung deinstallieren.

Ein Administrator kann auch „%Programfiles%\Microsoft Dependency Agent\Uninstall.exe“ ausführen, um den Dependency-Agent zu deinstallieren.

#### <a name="uninstall-the-dependency-agent-on-linux"></a>Deinstallieren des Dependency-Agents unter Linux

Wenn Sie den Dependency-Agent unter Linux vollständig deinstallieren möchten, müssen Sie den Agent selbst und den Connector entfernen, der automatisch mit dem Agent installiert wird. Mit dem folgenden einzelnen Befehl können Sie beide Komponenten deinstallieren:

```
rpm -e dependency-agent dependency-agent-connector
```

## <a name="management-packs"></a>Management Packs

Wenn Wire Data in einem Log Analytics-Arbeitsbereich aktiviert ist, wird an alle Windows-Server in diesem Arbeitsbereich ein Management Pack von 300 KB gesendet. Wenn Sie System Center Operations Manager-Agents in einer [verbundenen Verwaltungsgruppe](log-analytics-om-agents.md) verwenden, wird das Dependency Monitor-Management Pack von System Center Operations Manager bereitgestellt. Wenn die Agents direkt verbunden sind, wird das Management Pack von Log Analytics bereitgestellt.

Der Name des Management Packs lautet „Microsoft.IntelligencePacks.ApplicationDependencyMonitor“. Es wird in das Verzeichnis „%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\“ geschrieben. Die vom Management Pack verwendete Datenquelle lautet „%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources&lt;AutoGeneratedID&gt;\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll“.

## <a name="using-the-solution"></a>Verwenden der Lösung

**Installieren und Konfigurieren der Lösung**

Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Mit der Wire Data-Lösung werden Daten von Computern erfasst, auf denen Windows Server 2012 R2, Windows 8.1 und neuere Betriebssysteme ausgeführt werden.
- Microsoft .NET Framework 4.0 oder höher ist auf Computern erforderlich, von denen Wire Data-Daten erfasst werden sollen.
- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Prozesses die Wire Data-Lösung Ihrem Log Analytics-Arbeitsbereich hinzu. Es ist keine weitere Konfiguration erforderlich.
- Wenn Sie Wire Data-Daten für eine bestimmte Lösung anzeigen möchten, muss die Lösung bereits Ihrem Arbeitsbereich hinzugefügt worden sein.

Nachdem Sie Agents installiert haben, und Sie die Lösung installieren, wird die Wire Data 2.0-Kachel im Arbeitsbereich angezeigt.

> [!NOTE]
> Derzeit müssen Sie das OMS-Portal zum Anzeigen von Wire Data verwenden. Sie können das Azure-Portal nicht zum Anzeigen von Wire Data verwenden.

![Wire Data-Kachel](./media/log-analytics-wire-data/wire-data-tile.png)

## <a name="using-the-wire-data-20-solution"></a>Verwenden der Wire Data 2.0-Lösung

Klicken Sie im OMS-Portal auf die **Wire Data 2.0**-Kachel, um das Wire Data-Dashboard zu öffnen. Das Dashboard enthält die Blätter, die in der folgenden Tabelle angegeben sind. Auf jedem Blatt sind bis zu 10 Einträge aufgeführt, die die Kriterien des Blatts für den angegebenen Bereich und Zeitraum erfüllen. Sie können eine Protokollsuche durchführen, mit der alle Einträge zurückgegeben werden, indem Sie unten auf dem Blatt auf **Alle anzeigen** oder auf die Blattüberschrift klicken.

| **Blatt** | **Beschreibung** |
| --- | --- |
| Agents, die Netzwerkdatenverkehr erfassen | Zeigt die Anzahl der Agents an, die Netzwerkdatenverkehr erfassen, und listet die 10 Computer auf, die am meisten Datenverkehr erfassen. Klicken Sie auf die Zahl, um eine Protokollsuche für <code>Type:WireData &#124; measure Sum(TotalBytes) by Computer &#124; top 500000</code> durchzuführen. Klicken Sie auf einen Computer in der Liste, um eine Protokollsuche durchzuführen, die die Gesamtanzahl erfasster Bytes zurückgibt. |
| Lokale Subnetze | Zeigt die Anzahl von lokalen Subnetzen an, die Agents ermittelt haben.  Klicken Sie auf die Anzahl, um eine Protokollsuche für <code>Type:WireData &#124; Measure Sum(TotalBytes) by LocalSubnet</code> durchzuführen, die alle Subnetze mit der Anzahl von Bytes auflistet, die über jedes einzelne gesendet werden. Klicken Sie auf ein Subnetz in der Liste, um eine Protokollsuche durchzuführen, die die Gesamtanzahl über das Subnetz gesendeter Bytes zurückgibt. |
| Protokolle auf Anwendungsebene | Zeigt die von Agents ermittelte Anzahl der auf Anwendungsebene verwendeten Protokolle. Klicken Sie auf die Zahl, um eine Protokollsuche für <code>Type:WireData &#124; Measure Sum(TotalBytes) by ApplicationProtocol</code> durchzuführen. Klicken Sie auf ein Protokoll, um eine Protokollsuche durchzuführen, die die Gesamtanzahl mithilfe des Protokolls gesendeter Bytes zurückgibt. |

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![Wire Data-Dashboard](./media/log-analytics-wire-data/wire-data-dash.png)

Mit dem Blatt **Agents, die Netzwerkdatenverkehr erfassen** können Sie bestimmen, wie viel Netzwerkbandbreite von Computern in Anspruch genommen wird. Mit diesem Blatt können Sie leicht den _„geschwätzigsten“_ Computer in Ihrer Umgebung ermitteln. Solche Computer könnten überlastet sein, nicht normal arbeiten oder mehr Netzwerkressourcen als normal verwenden.

![Beispiel: Protokollsuche](./media/log-analytics-wire-data/log-search-example01.png)

Auf ähnliche Weise können Sie das Blatt **Lokale Subnetze** verwenden, um zu bestimmen, wie viel Netzwerkdatenverkehr über die Subnetze verschoben wird. Benutzer definieren Subnetze oft um kritische Bereiche für ihre Anwendungen herum. Dieses Blatt bietet einen Einblick in die Bereiche.

![Beispiel: Protokollsuche](./media/log-analytics-wire-data/log-search-example02.png)

Das Blatt **Protokolle auf Anwendungsebene** ist nützlich, da es hilfreich ist, zu wissen, welche Protokolle verwendet werden. Sie könnten z.B. erwarten, dass SSH nicht in Ihrer Netzwerkumgebung verwendet wird. Die im Blatt angezeigten Informationen können Ihre Erwartungen schnell bestätigen oder widerlegen.

![Beispiel: Protokollsuche](./media/log-analytics-wire-data/log-search-example03.png)

In diesem Beispiel könnten Sie anhand von SSH-Einzelheiten erkennen, welche Computer SSH verwenden, und viele andere Details zur Kommunikation.

![SSH-Suchergebnisse](./media/log-analytics-wire-data/ssh-details.png)

Es ist auch hilfreich zu wissen, ob der Protokolldatenverkehr mit der Zeit steigt oder sinkt. Wenn z.B. die Menge der von einer Anwendung übertragenen Daten steigt, dann könnte dies ein Aspekt sein, über den Sie informiert sein müssen, oder den Sie erwähnenswert finden.

## <a name="input-data"></a>Eingabedaten

Wire Data sammelt Metadaten über den Netzwerkverkehr mit den Agents, die Sie aktiviert haben. Jeder Agent sendet alle 15 Sekunden Daten.

## <a name="output-data"></a>Ausgabedaten

Ein Datensatz mit dem Typ _WireData_ wird für jeden Eingabedatentyp erstellt. Die Eigenschaften der WireData-Datensätze sind in der folgenden Tabelle aufgeführt:

| Eigenschaft | Beschreibung |
|---|---|
| Computer | Name des Computers, auf dem Daten gesammelt wurden |
| TimeGenerated | Uhrzeit des Datensatzes |
| LocalIP | IP-Adresse des lokalen Computers |
| SessionState | Verbunden oder getrennt |
| ReceivedBytes | Summe empfangener Bytes |
| ProtocolName | Name des verwendeten Netzwerkprotokolls |
| IPVersion | IP-Version |
| Richtung | Eingehend oder ausgehend |
| MaliciousIP | IP-Adresse einer bekannten schädlichen Quelle |
| Severity | Vermuteter Malwareschweregrad |
| RemoteIPCountry | Land der Remote-IP-Adresse |
| ManagementGroupName | Name der Operations Manager-Verwaltungsgruppe |
| SourceSystem | Quelle, auf der Daten gesammelt wurden |
| SessionStartTime | Startzeit der Sitzung |
| SessionEndTime | Endzeit der Sitzung |
| LocalSubnet | Subnetz, in dem Daten gesammelt wurden |
| LocalPortNumber | Lokale Portnummer |
| RemoteIP | Vom Remotecomputer verwendete Remote-IP-Adresse |
| RemotePortNumber | Von der Remote-IP-Adresse verwendete Portnummer |
| SessionID | Ein eindeutiger Wert, der die Kommunikationssitzung zwischen zwei IP-Adressen identifiziert |
| SentBytes | Anzahl der gesendeten Bytes |
| TotalBytes | Gesamtanzahl der während der Sitzung gesendeten Bytes |
| ApplicationProtocol | Typ des verwendeten Netzwerkprotokolls   |
| ProcessID | Windows-Prozess-ID |
| ProcessName | Pfad und Dateiname des Prozesses |
| RemoteIPLongitude | IP-Längengradwert |
| RemoteIPLatitude | IP-Breitengradwert |


## <a name="next-steps"></a>Nächste Schritte

- [Durchsuchen von Protokollen](log-analytics-log-searches.md) und darüber, wie Sie ausführliche Wire Data-Suchdatensätze anzeigen.

