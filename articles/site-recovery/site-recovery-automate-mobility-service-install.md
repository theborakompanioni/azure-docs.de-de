---
title: Bereitstellen von Site Recovery Mobility Service mithilfe von Azure Automation DSC | Microsoft-Dokumentation
description: "Beschreibt, wie Sie Azure Automation DSC verwenden, um den Azure Site Recovery Mobility Service und den Azure-Agent automatisch für die Replikation von VMware-VMs und physischen Servern in Azure bereitzustellen."
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: krnese
ms.translationtype: Human Translation
ms.sourcegitcommit: 2b9a8b5d7b889926f1cdbb6787fea1579dd21190
ms.openlocfilehash: 941bc07fa9be765403e8fe541caf660eef15585e
ms.contentlocale: de-de
ms.lasthandoff: 02/22/2017


---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Bereitstellen des Mobility Service in Azure Automation DSC für die VM-Replikation
In Operations Management Suite bieten wir Ihnen eine umfassende Lösung zur Sicherung und Notfallwiederherstellung, die Sie im Rahmen Ihres Geschäftskontinuitätsplans nutzen können.

Zunächst haben wir dafür Hyper-V und Hyper-V-Replikate bereitgestellt. Aber wir haben dieses Angebot erweitert, um ein heterogenes Setup zu unterstützen, da Kunden über mehrere Hypervisoren und Plattformen in ihren Clouds verfügen.

Wenn Sie heute VMware-Workloads und/oder physische Server ausführen, werden auf einem Verwaltungsserver alle Azure Site Recovery-Komponenten in Ihrer Umgebung ausgeführt, um die Kommunikation und die Datenreplikation mit Azure abzuwickeln, wenn Azure das Ziel ist.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Bereitstellen von Site Recovery Mobility Service mithilfe von Automation DSC
Lassen Sie uns mit einem kurzen Blick auf die Funktion dieses Verwaltungsservers beginnen.

Auf dem Verwaltungsserver werden mehrere Serverrollen ausgeführt. Eine der Rollen ist *Konfiguration*. Sie dient zum Koordinieren der Kommunikation und zum Verwalten der Datenreplikations- und Wiederherstellungsvorgänge.

Darüber hinaus fungiert die *Prozessrolle* als Replikationsgateway. Diese Rolle empfängt Replikationsdaten von geschützten Quellcomputern, optimiert sie durch Zwischenspeicherung, Komprimierung und Verschlüsselung und sendet sie an ein Azure-Speicherkonto. Zu den Funktionen der Prozessrolle zählen auch die Abwicklung der Pushinstallation des Mobility Service auf geschützten Computern und die automatische Ermittlung von VMware-VMs.

Bei einem Failback von Azure übernimmt die *Masterzielrolle* die Verarbeitung der Replikationsdaten im Rahmen dieses Vorgangs.

Für die geschützten Computer wird der *Mobility Service*verwendet. Diese Komponente wird auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt, den Sie in Azure replizieren möchten. Sie erfasst Datenschreibvorgänge auf dem Computer und leitet sie an den Verwaltungsserver (Prozessrolle) weiter.

Im Zusammenhang mit der Geschäftskontinuität ist es wichtig, Ihre Workloads, Ihre Infrastruktur und die beteiligten Komponenten zu verstehen. Dann können Sie die Anforderungen für Recovery Time Objective (RTO) und Recovery Point Objective (RPO) erfüllen. In diesem Kontext ist der Mobility Service von entscheidender Bedeutung. Er stellt sicher, dass Ihre Workloads wie erwartet geschützt werden.

Wie also können Sie mithilfe einiger Operations Management Suite-Komponenten auf optimierte Weise sicherstellen, dass Sie über ein zuverlässiges geschütztes Setup verfügen?

Dieser Artikel enthält ein Beispiel dafür, wie Sie Azure Automation DSC (Desired State Configuration) zusammen mit Site Recovery verwenden können, um Folgendes sicherzustellen:

* Der Mobility Service und der Azure-VM-Agent werden auf den Windows-Computern bereitgestellt, die Sie schützen möchten.
* Der Mobility Service und der Azure-VM-Agent werden immer ausgeführt, wenn Azure das Replikationsziel ist.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Repository zum Speichern des erforderlichen Setups
* Ein Repository zum Speichern der erforderlichen Passphrase zur Registrierung beim Verwaltungsserver

  > [!NOTE]
  > Für jeden Verwaltungsserver wird eine eindeutige Passphrase generiert. Falls Sie mehrere Verwaltungsserver bereitstellen, müssen Sie sicherstellen, dass die richtige Passphrase in der Datei „passphrase.txt“ gespeichert wird.
  >
  >
* Windows Management Framework (WMF) 5.0 auf den Computern, die Sie schützen möchten (eine Voraussetzung für Automation DSC)

  > [!NOTE]
  > Informationen zum Verwenden von DSC für Windows-Computer, auf denen WMF 4.0 installiert ist, finden Sie im Abschnitt [Verwenden von DSC in getrennten Umgebungen](#Use DSC in disconnected environments).
  >
  >

Der Mobility Service kann über die Befehlszeile installiert werden und akzeptiert mehrere Argumente. Daher müssen Sie die Binärdateien (nachdem sie aus dem Setup extrahiert wurden) an einem Ort speichern, von dem Sie sie mit einer DSC-Konfiguration abrufen können.

## <a name="step-1-extract-binaries"></a>Schritt 1: Extrahieren der Binärdateien
1. Um die Dateien zu extrahieren, die Sie für diese Konfiguration benötigen, wechseln Sie in das folgende Verzeichnis auf dem Verwaltungsserver:

    **\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    In diesem Ordner sollte eine MSI-Datei mit folgendem Namen vorhanden sein:

    **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**

    Verwenden Sie den folgenden Befehl, um das Installationsprogramm zu extrahieren:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Wählen Sie alle Dateien aus, und senden Sie sie an einen komprimierten (gezippten) Ordner.

Jetzt haben Sie die Binärdateien, die Sie zum Automatisieren des Mobility Service-Setups mit Automation DSC benötigen.

### <a name="passphrase"></a>Passphrase
Als Nächstes müssen Sie festlegen, wo der ZIP-komprimierte Ordner gespeichert werden soll. Sie können ein Azure-Speicherkonto verwenden, wie weiter unten dargestellt, um die für das Setup erforderliche Passphrase zu speichern. Der Agent wird dann während des Prozesses beim Verwaltungsserver registriert.

Die Passphrase, die Sie beim Bereitstellen des Verwaltungsservers erhalten haben, kann in einer Textdatei namens „passphrase.txt“ gespeichert werden.

Legen Sie den ZIP-Ordner und die Passphrase in einem dedizierten Container im Azure-Speicherkonto ab.

![Speicherort des Ordners](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Falls Sie diese Dateien lieber auf einer Freigabe in Ihrem Netzwerk speichern möchten, ist dies ebenfalls möglich. Sie müssen nur sicherstellen, dass die DSC-Ressource, die Sie später verwenden, Zugriff hat und das Setup sowie die Passphrase abrufen kann.

## <a name="step-2-create-the-dsc-configuration"></a>Schritt 2: Erstellen der DSC-Konfiguration
Für das Setup ist WMF 5.0 erforderlich. Damit der Computer die Konfiguration erfolgreich über Automation DSC anwenden kann, muss WMF 5.0 installiert sein.

Die folgende DSC-Beispielkonfiguration wird in der Umgebung verwendet:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Die Konfiguration führt folgende Schritte aus:

* Die Variablen weisen die Konfiguration an, von wo die Binärdateien für den Mobility Service und den Azure-VM-Agent sowie die Passphrase abgerufen werden müssen und wo die Ausgabe gespeichert werden soll.
* Die Konfiguration importiert die DSC-Ressource „xPSDesiredStateConfiguration“, sodass Sie `xRemoteFile` zum Herunterladen der Dateien aus dem Repository verwenden können.
* Die Konfiguration erstellt ein Verzeichnis zum Speichern der Binärdateien.
* Die Ressource „Archive“ extrahiert die Dateien aus dem ZIP-Ordner.
* Die Ressource „Package Install“ installiert den Mobility Service über das Installationsprogramm UNIFIEDAGENT.EXE mit bestimmten Argumenten. (Die Variablen, aus denen die Argumente bestehen, müssen für Ihre Umgebung entsprechend geändert werden.)
* Die Ressource „Package AzureAgent“ installiert den Azure-VM-Agent, der auf jedem virtuellen Computer vorhanden sein sollte, der in Azure ausgeführt wird. Der Azure-VM-Agent ermöglicht außerdem das Hinzufügen von Erweiterungen zum virtuellen Computer nach einem Failover.
* Die Ressourcen „Service“ stellen sicher, dass die zugehörigen Mobility Services und Azure-Dienste immer ausgeführt werden.

Speichern Sie die Konfiguration als **ASRMobilityService**.

> [!NOTE]
> Denken Sie daran, die CSIP in Ihrer Konfiguration durch die IP-Adresse des tatsächlichen Verwaltungsservers zu ersetzen, damit der Agent unter Verwendung der richtigen Passphrase korrekt verbunden wird.
>
>

## <a name="step-3-upload-to-automation-dsc"></a>Schritt 3: Hochladen in Automation DSC
Da die erstellte DSC-Konfiguration ein erforderliches DSC-Ressourcenmodul (xPSDesiredStateConfiguration) importiert, müssen Sie dieses Modul vor dem Hochladen der DSC-Konfiguration in Automation importieren.

Melden Sie sich bei Ihrem Automation-Konto an, navigieren Sie zu **Ressourcen** > **Module**, und klicken Sie auf **Katalog durchsuchen**.

Hier können Sie nach dem Modul suchen und es in Ihr Konto importieren.

![Modul importieren](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Importieren Sie anschließend auf dem Computer, auf dem die Azure Resource Manager-Module installiert sind, die neu erstellte DSC-Konfiguration.

### <a name="import-cmdlets"></a>Import-Cmdlets
Melden Sie sich in PowerShell bei Ihrem Azure-Abonnement an. Ändern Sie die Cmdlets entsprechend Ihrer Umgebung, und erfassen Sie die Informationen für Ihr Automation-Konto in einer Variablen:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Laden Sie die Konfiguration mit dem folgenden Cmdlet in Automation DSC hoch:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Kompilieren der Konfiguration in Automation DSC
Als Nächstes müssen Sie die Konfiguration in Automation DSC kompilieren, damit Sie mit dem Registrieren von Knoten beginnen können. Dazu führen Sie das folgende Cmdlet aus:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Dies kann einige Minuten dauern, da Sie im Grunde die Konfiguration im gehosteten DSC-Pulldienst bereitstellen.

Nach dem Kompilieren der Konfiguration können Sie die Auftragsinformationen entweder mithilfe von PowerShell (Get-AzureRmAutomationDscCompilationJob) oder mit dem [Azure-Portal](https://portal.azure.com/)abrufen.

![Auftrag abrufen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Damit haben Sie die DSC-Konfiguration erfolgreich in Automation DSC veröffentlicht und hochgeladen.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Schritt 4: Integrieren von Computern in Automation DSC
> [!NOTE]
> Eine der Voraussetzungen für dieses Szenario ist die Aktualisierung Ihrer Windows-Computer mit der neuesten WMF-Version. Sie können die richtige Version für Ihre Plattform aus dem [Download Center](https://www.microsoft.com/download/details.aspx?id=50395)herunterladen und installieren.
>
>

Sie erstellen jetzt eine Metakonfiguration für DSC, die Sie auf die Knoten anwenden. Dazu müssen Sie die Endpunkt-URL und den Primärschlüssel für das ausgewählte Automation-Konto in Azure abrufen. Diese Werte finden Sie im Automation-Konto auf dem Blatt **Alle Einstellungen** unter **Schlüssel**.

![Schlüsselwerte](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In diesem Beispiel verwenden Sie einen physischen Windows Server 2012 R2-Server, der mit Site Recovery geschützt werden soll.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Überprüfen der Registrierung auf ausstehende Dateiumbenennungsvorgänge
Bevor Sie den Server dem Automation DSC-Endpunkt zuordnen, empfiehlt es sich, in der Registrierung zu überprüfen, ob ausstehende Dateiumbenennungsvorgänge vorliegen. Sie können verhindern, dass das Setup aufgrund eines ausstehenden Neustarts beendet wird.

Führen Sie das folgende Cmdlet aus, um sicherzustellen, dass auf dem Server kein Neustart aussteht:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Wenn die Ausgabe leer ist, können Sie fortfahren. Andernfalls sollten Sie den Server während eines Wartungsfensters neu starten.

Starten Sie PowerShell Integrated Scripting Environment (ISE), und führen Sie das folgende Skript aus, um die Konfiguration auf dem Server anzuwenden. Dies ist im Grunde eine lokale DSC-Konfiguration, die das lokale Konfigurations-Manager-Modul anweist, die Registrierung beim Automation DSC-Dienst vorzunehmen und die spezifische Konfiguration (ASRMobilityService.localhost) abzurufen.

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Diese Konfiguration führt dazu, dass sich das lokale Konfigurations-Manager-Modul selbst bei Automation DSC registriert. Sie bestimmt auch, wie das Modul ausgeführt werden soll, was im Fall einer Konfigurationsabweichung geschehen soll (ApplyAndAutoCorrect) und wie die Konfiguration fortgesetzt werden soll, falls ein Neustart erforderlich ist.

Nachdem Sie dieses Skript ausgeführt haben, sollte der Knoten die Registrierung bei Automation DSC starten.

![Knotenregistrierung wird ausgeführt](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Wenn Sie zum Azure-Portal zurückkehren, sehen Sie, dass der neu registrierte Knoten jetzt im Portal angezeigt wird.

![Registrierter Knoten im Portal](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Auf dem Server können Sie das folgende PowerShell-Cmdlet ausführen, um sicherzustellen, dass der Knoten korrekt registriert wurde:

```powershell
Get-DscLocalConfigurationManager
```

Sobald die Konfiguration mithilfe von Pull übertragen und auf den Server angewendet wurde, können Sie den Konfigurationsstatus mit dem folgenden Cmdlet überprüfen:

```powershell
Get-DscConfigurationStatus
```

Die Ausgabe zeigt, dass der Server seine Konfiguration erfolgreich abgerufen hat:

![Ausgabe](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Darüber hinaus verfügt das Mobility Service-Setup über ein eigenes Protokoll, das unter „*Systemlaufwerk*\ProgramData\ASRSetupLogs“ gespeichert wird.

Das ist alles. Sie haben nun erfolgreich den Mobility Service auf dem Computer bereitgestellt und registriert, den Sie mit Site Recovery schützen möchten. DSC stellt sicher, dass die erforderlichen Dienste immer ausgeführt werden.

![Erfolgreiche Bereitstellung](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Sobald die erfolgreiche Bereitstellung vom Verwaltungsserver erkannt wurde, können Sie mit Site Recovery auf dem Computer den Schutz konfigurieren und die Replikation aktivieren.

## <a name="use-dsc-in-disconnected-environments"></a>Verwenden von DSC in getrennten Umgebungen
Auch wenn Ihre Computer nicht mit dem Internet verbunden sind, können Sie DSC verwenden, um den Mobility Service für zu schützende Workloads bereitzustellen und zu konfigurieren.

Sie können Ihren eigenen DSC-Pullserver in Ihrer Umgebung instanziieren, um im Wesentlichen die gleichen Funktionen bereitzustellen, die Automation DSC bietet. Die Clients übertragen also die Konfiguration (nach der Registrierung) mithilfe von Pull an den DSC-Endpunkt. Eine weitere Option ist jedoch, die DSC-Konfiguration manuell mithilfe von Push auf Ihre Computer (lokal oder remote) zu übertragen.

Beachten Sie, dass in diesem Beispiel ein Parameter für den Computernamen hinzugefügt wurde. Die Remotedateien befinden sich jetzt auf einer Remotefreigabe, auf die die zu schützenden Computer Zugriff haben müssen. Am Ende des Skripts startet das Skript die Konfiguration und beginnt damit, die DSC-Konfiguration auf den Zielcomputer anzuwenden.

### <a name="prerequisites"></a>Voraussetzungen
Stellen Sie sicher, dass das PowerShell-Modul „xPSDesiredStateConfiguration“ installiert ist. Für Windows-Computer, auf denen WMF 5.0 installiert ist, können Sie das Modul „xPSDesiredStateConfiguration“ installieren, indem Sie das folgende Cmdlet auf den Zielcomputern ausführen:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Sie können das Modul auch herunterladen und speichern, falls Sie es an Windows-Computer verteilen müssen, die WMF 4.0 verwenden. Führen Sie dieses Cmdlet auf einem Computer aus, auf dem PowerShellGet (WMF 5.0) vorhanden ist:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Stellen Sie für WMF 4.0 zudem sicher, dass das [Windows 8.1-Update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) auf den Computern installiert ist.

Die folgende Konfiguration kann mithilfe von Push auf Windows-Computer mit WMF 5.0 und WMF 4.0 übertragen werden:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Informationen zum Instanziieren eines eigenen DSC-Pullservers innerhalb des Unternehmensnetzwerks, der die gleichen Funktionen wie Automation DSC bietet, finden Sie unter [Einrichten eines DSC-Webpullservers](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Optional: Bereitstellen der DSC-Konfiguration mithilfe einer Azure Resource Manager-Vorlage
In diesem Artikel lag der Fokus bisher darauf, wie Sie Ihre eigene DSC-Konfiguration erstellen, um den Mobility Service und den Azure-VM-Agent automatisch bereitzustellen und sicherzustellen, dass sie auf den zu schützenden Computern ausgeführt werden. Es gibt auch eine Azure Resource Manager-Vorlage, die diese DSC-Konfiguration in einem neuen oder vorhandenen Azure Automation-Konto bereitstellt. Die Vorlage verwendet Eingabeparameter um Automation-Ressourcen zu erstellen, die Variablen für Ihre Umgebung enthalten.

Nach der Bereitstellung der Vorlage können Sie einfach mit Schritt 4 dieses Leitfadens fortfahren, um Ihre Computer zu integrieren.

Die Vorlage führt folgende Schritte aus:

1. Verwenden eines vorhandenen Automation-Kontos oder Erstellen eines neuen Kontos
2. Akzeptieren der Eingabeparameter für Folgendes:
   * ASRRemoteFile: Speicherort des Mobility Service-Setups
   * ASRPassphrase: Speicherort der Datei „passphrase.txt“
   * ASRCSEndpoint: IP-Adresse des Verwaltungsservers
3. Importieren des PowerShell-Moduls „xPSDesiredStateConfiguration“
4. Erstellen und Kompilieren der DSC-Konfiguration

Die obigen Schritte werden in der richtigen Reihenfolge ausgeführt, sodass Sie mit dem Integrieren Ihrer zu schützenden Computer beginnen können.

Die Vorlage mit Anleitungen für die Bereitstellung finden Sie auf [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Stellen Sie die Vorlage mithilfe von PowerShell bereit:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie die Mobility Service-Agents bereitgestellt haben, können Sie die [Replikation](site-recovery-vmware-to-azure.md#enable-replication) für die virtuellen Computer aktivieren.

