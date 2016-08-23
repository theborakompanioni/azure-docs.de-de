<properties
	pageTitle="Replizieren von virtuellen VMware-Computern in Azure mithilfe von Site Recovery mit Azure Automation DSC | Microsoft Azure"
	description="Beschreibt, wie Sie Azure Automation DSC verwenden, um den ASR Mobility Service und den Azure-Agent automatisch für virtuelle/physische Computer in Azure bereitzustellen."
	services="site-recovery"
	documentationCenter=""
	authors="krnese"
	manager="lorenr"
	editor=""/>

<tags
	ms.service="site-recovery"
	ms.workload="backup-recovery"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/26/2016"
	ms.author="krnese"/>

# Replizieren von VMware-VMs in Azure mithilfe von Site Recovery mit Azure Automation DSC

In OMS bieten wir Ihnen eine umfassende Lösung zur Sicherung und Notfallwiederherstellung, die Sie im Rahmen Ihres Geschäftskontinuitätsplans nutzen können.

Die Entwicklung dieser Lösung begann zusammen mit Hyper-V und Hyper-V-Replikat. Da wir uns bewusst sind, dass Kunden in ihren Clouds über mehrere Hypervisoren und Plattformen verfügen, haben wir sie jedoch zur Unterstützung eines heterogenen Setups erweitert.

Wenn Sie heute VMware-Workloads und/oder physische Server ausführen, verwenden wir einen Verwaltungsserver, der alle Site Recovery-Komponenten in Ihrer Umgebung ausführt, zum Abwickeln der Kommunikation und Datenreplikation mit Azure, wenn Azure das Replikationsziel ist.

## Bereitstellen des ASR Mobility Service mithilfe von OMS Automation DSC
Lassen Sie uns mit einem kurzen Blick auf die Funktion dieses Verwaltungsservers beginnen:

Der Verwaltungsserver führt mehrere Serverrollen aus (z.B. **Konfiguration**), um die Kommunikation zu koordinieren sowie die Datenreplikations- und Wiederherstellungsprozesse zu verwalten.

Zudem fungiert die Rolle **Prozess** als ein Replikationsgateway, das Replikationsdaten von geschützten Quellcomputern empfängt, diese durch Zwischenspeicherung, Komprimierung und Verschlüsselung optimiert und sie anschließend an ein Azure-Speicherkonto sendet. Zu den Funktionen der Prozessrolle zählen auch die Abwicklung der Pushinstallation des Mobiliy Service auf geschützten Computern und die automatische Ermittlung von virtuellen VMware-VMs.

Bei einem Failback von Azure übernimmt die Rolle **Masterziel** die Verantwortung und verarbeitet die Replikationsdaten im Rahmen dieses Vorgangs.

Für die geschützten Computer stützen wir uns auf den **Mobility Service** – eine Komponente, die auf jedem Computer (VMware-VM oder physischer Server) bereitgestellt wird, den Sie in Azure replizieren möchten. Seine Rolle besteht darin, Datenschreibvorgänge auf dem Computer zu erfassen und an den Verwaltungsserver (Prozessrolle) weiterzuleiten.

Im Zusammenhang mit der Geschäftskontinuität ist es wichtig, Ihre Workloads, Ihre Infrastruktur und auch die zum Erfüllen der Anforderungen für Ihre RTO (Recovery Time Objective) und RPO (Recovery Point Objective) eingesetzten Komponenten zu verstehen. In diesem Kontext ist der Mobility Service von entscheidender Bedeutung. Er stellt sicher, dass Ihre Workloads wie erwartet geschützt werden.

Wie also können wir mithilfe einiger OMS-Komponenten auf optimierte Weise sicherstellen, dass wir über ein zuverlässiges geschütztes Setup verfügen?

In diesem Artikel erläutern wir anhand eines Beispiels, wie Sie OMS Automation DSC zusammen mit OMS Site Recovery nutzen können, um sicherzustellen, dass der Mobility Service und der Azure-VM-Agent auf den zu schützenden Windows-Computern bereitgestellt und immer ausgeführt werden, wenn Azure das Replikationsziel ist.

## Voraussetzungen

- Ein Repository zum Speichern des erforderlichen Setups
- Ein Repository zum Speichern der erforderlichen Passphrase zur Registrierung beim Verwaltungsserver
- Windows Management Framework 5.0 auf den Computern, die Sie schützen möchten (Voraussetzung für OMS Automation DSC)

Wenn Sie DSC für Ihre Windows-Computer mit WMF 4.0 verwenden möchten, finden Sie im Abschnitt „Verwenden von DSC in getrennten Umgebungen“ weitere Informationen.

(Für jeden Verwaltungsserver wird eine eindeutige Passphrase generiert. Falls Sie mehrere Verwaltungsserver bereitstellen, müssen Sie sicherstellen, dass die richtige Passphrase in der Datei „Passphrase.txt“ gespeichert wird.)

Der Mobility Service kann über die Befehlszeile installiert werden und akzeptiert mehrere Argumente.

Daher müssen wir die Binärdateien (nachdem sie aus dem Setup extrahiert wurden) an einem Ort speichern, von dem sie mit einer DSC-Konfiguration abgerufen werden können.

## Schritt 1: Extrahieren der Binärdateien

1. Navigieren Sie zum Extrahieren der Dateien für dieses Setup zum folgenden Verzeichnis auf dem Verwaltungsserver: **\\Microsoft Azure Site Recovery\\home\\svsystems\\pushinstallsvc\\repository**

    In diesem Ordner sollte eine MSI-Datei mit folgendem Namen vorhanden sein:

    **Microsoft-ASR\_UA\_<Version>_Windows\_GA_<Datum>\_Release.exe**

    Verwenden Sie das folgende Cmdlet, um den Installer zu extrahieren:

    .\\Microsoft-ASR\_UA\_9.1.0.0_Windows\_GA_02May2016\_release.exe /q /x:C:\\Users\\Administrator\\Desktop\\Mobility\_Service\\Extract

2. Wählen Sie alle Dateien aus, und senden Sie sie an einen komprimierten (gezippten) Ordner.

Fertig! Jetzt haben Sie die Binärdateien, die Sie zum Automatisieren des Mobility Service-Setups mit OMS Automation DSC benötigen.

### Passphrase
Als Nächstes müssen Sie festlegen, wo der ZIP-komprimierte Ordner gespeichert werden soll. Ich verwende in diesem Beispiel ein Speicherkonto in Azure. In diesem Konto habe ich auch die Passphrase gespeichert, die ich für das Setup benötige, damit der Agent im Rahmen des Prozesses beim Verwaltungsserver registriert wird.

Die Passphrase, die Sie beim Bereitstellen des Verwaltungsservers erhalten haben, kann auch als TXT-Datei „passphrase.txt“ gespeichert werden.

Ich habe sowohl den ZIP-Ordner als auch die Passphrase in einem dedizierten Container in meinem Azure-Speicherkonto abgelegt.

![Speicherort des Ordners](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Falls Sie diese Dateien lieber auf einer Freigabe in Ihrem Netzwerk speichern möchten, ist dies ebenfalls möglich. Sie müssen nur sicherstellen, dass die DSC-Ressource, die wir später verwenden, Zugriff hat und das Setup sowie die Passphrase abrufen kann.

## Schritt 2 – Erstellen der DSC-Konfiguration
Für mein Setup ist WMF 5.0 erforderlich. Damit der Computer die Konfiguration erfolgreich über OMS Automation DSC anwenden kann, muss daher WMF 5.0 installiert sein.

Die folgende DSC-Konfiguration wird in meiner Umgebung verwendet:

```
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
    $Arguments = '/Role ' + '"' + $Role + '"' + ' /InstallLocation ' + '"' + $Install + '"' + ' /CSEndpoint ' + '"' + $CSEndpoint + '"' + ' /PassphraseFilePath ' +  '"' +$LocalPassphrase + '"'  

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        xRemoteFile Setup {
           URI = $RemoteFile
           DestinationPath = $TempDestination
           DependsOn = "[File]Directory"
            }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = "[File]Directory"
            }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = "[File]Directory"
            }

        File Directory {
            DestinationPath = "C:\Temp\ASRSetup"
            Type = "Directory"            
            }

        Archive ASRzip {
           Path = $TempDestination
           Destination = "C:\Temp\ASRSetup"
           DependsOn = "[xRemotefile]Setup"
           }

        Package Install {
            Path = "C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE"
            Ensure = "Present"
            Name = "Microsoft Azure Site Recovery mobility Service/Master Target Server"
            ProductId = "275197FC-14FD-4560-A5EB-38217F80CBD1"
            Arguments = $Arguments
            DependsOn = "[Archive]ASRzip"
            }

        Package AzureAgent {
            Path = "C:\Temp\AzureVmAgent.msi"
            Ensure = "Present"
            Name = "Windows Azure VM Agent - 2.7.1198.735"
            ProductId = "5CF4D04A-F16C-4892-9196-6025EA61F964"
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = "[Package]Install"
            }

        Service ASRvx {
            Name = "svagents"
            Ensure = "Present"
            State = "Running"
            DependsOn = "[Package]Install"
            }

        Service ASR {
            Name = "InMage Scout Application Service"
            Ensure = "Present"
            State = "Running"
            DependsOn = "[Package]Install"
            }

        Service AzureAgentService {
            Name = "WindowsAzureGuestAgent"
            Ensure = "Present"
            State = "Running"
            DependsOn = "[Package]AzureAgent"
            }

        Service AzureTelemetry {
            Name = "WindowsAzureTelemetryService"
            Ensure = "Present"
            State = "Running"
            DependsOn = "[Package]AzureAgent"
            }
    }
}
```
Die Konfiguration führt folgende Schritte aus:

- Die Variablen weisen die Konfiguration an, von wo die Binärdateien für den Mobility Service und den Azure-VM-Agent sowie die Passphrase abgerufen werden müssen und wo die Ausgabe gespeichert werden soll.
- Die DSC-Ressource (DscResource) „xPSDesiredStateConfiguration“ wird importiert, sodass wir „xRemoteFile“ zum Herunterladen der Dateien aus dem Repository verwenden können.
- Es wird ein Verzeichnis zum Speichern der Binärdateien erstellt.
- Die Ressource „Archive“ extrahiert die Dateien aus dem ZIP-Ordner.
- Die Paketressource „Install“ installiert den Mobility Service mithilfe des Installers UNIFIEDAGENT. EXE mit bestimmten Argumenten (die Variablen zum Erstellen der Argumente müssen entsprechend Ihrer Umgebung geändert werden).
- Das Paket „AzureAgent“ installiert den Azure-VM-Agent (es wird empfohlen, ihn auf jeder in Azure ausgeführten VM zu installieren) und ermöglicht es zudem, der VM nach einem Failover Erweiterungen hinzuzufügen.
- Die Ressourcen „Service“ stellen sicher, dass die zugehörigen Mobility Services und Azure-Dienste immer ausgeführt werden.

Ich habe die Konfiguration unter dem Namen **ASRMobilityService** in einem Ordner auf meinem Computer gespeichert.

(Denken Sie daran, die CSIP in Ihrer Konfiguration durch die IP-Adresse des tatsächlichen Verwaltungsservers zu ersetzen, damit der Agent unter Verwendung der richtigen Passphrase korrekt verbunden wird).

## Schritt 3 – Hochladen in OMS Automation DSC

Da die erstellte DSC-Konfiguration ein erforderliches DSC-Ressourcenmodul (xPSDesiredStateConfiguration) importiert, müssen Sie dieses Modul vor dem Hochladen der DSC-Konfiguration in OMS Automation importieren.

Melden Sie sich mit Ihrem Automation-Konto an, navigieren Sie zu „Ressourcen“ > „Module“, und klicken Sie auf „Katalog durchsuchen“.

Hier können Sie nach dem Modul suchen und es in Ihr Konto importieren.

![Modul importieren](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

Importieren Sie anschließend auf dem Computer, auf dem die Azure RM-Module installiert sind, die neu erstellte DSC-Konfiguration.

### Import-Cmdlets

Melden Sie sich in PowerShell bei Ihrem Azure-Abonnement an, und ändern Sie die Cmdlets entsprechend Ihrer Umgebung.

Zunächst lade ich die Konfiguration mit dem folgenden Cmdlet in OMS Automation DSC hoch:

```
Import-AzureRmAutomationDscConfiguration `
                                        -AutomationAccountName KNOMSAA `
                                        -ResourceGroupName KNOMS `
                                        -SourcePath C:\ASR\ASRMobilityService.ps1 `
                                        -Published `
                                        -Description "DSC Config for Mobility Service"
```

Als Nächstes müssen wir die Konfiguration in OMS Automation DSC kompilieren, damit wir mit dem Registrieren von Knoten beginnen können.

### Kompilieren der Konfiguration in OMS Automation DSC

Dazu führen wir das folgende Cmdlet aus:

```
Start-AzureRmAutomationDscCompilationJob `
                                        -AutomationAccountName KNOMSAA `
                                        -ResourceGroupName KNOMS `
                                        -ConfigurationName ASRMobilityService
```

Die Ausführung des Cmdlets kann einige Minuten dauern, da wir im Grunde die Konfiguration im gehosteten DSC-Pull-Dienst bereitstellen.

Danach können Sie die Auftragsinformationen entweder mithilfe von PowerShell (Get-AzureRmAutomationDscCompilationJob) abrufen oder „portal.azure.com“ verwenden.

![Auftrag abrufen](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Damit haben wir unsere DSC-Konfiguration erfolgreich in OMS Automation DSC veröffentlicht und hochgeladen.

## Schritt 4 – Onboarding von Computern in OMS Automation DSC
*Eine der Voraussetzungen für dieses Szenario ist die Aktualisierung Ihrer Windows-Computer mit der neuesten WMF-Version. Sie können die richtige Version für Ihre Plattform unter der folgenden URL herunterladen und installieren: https://www.microsoft.com/download/details.aspx?id=50395*

Wir erstellen jetzt eine Metakonfiguration für DSC, die wir auf unsere Knoten anwenden. Dazu müssen Sie die Endpunkt-URL und den Primärschlüssel für das ausgewählte Automation-Konto in Azure abrufen.

Diese Werte finden Sie im Automation-Konto auf dem Blatt „Alle Einstellungen“ unter „Schlüssel“.

![Schlüsselwerte](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

In meiner Umgebung habe ich einen physischen Windows Server 2012 R2-Server, den ich mit OMS Site Recovery schützen möchte.

Bevor wir den Server dem Automation DSC-Endpunkt zuordnen, empfiehlt es sich, in der Registrierung zu überprüfen, ob ausstehende Dateiumbenennungsvorgänge vorliegen, da diese dazu führen können, dass das Setup aufgrund eines ausstehenden Neustarts nicht abgeschlossen wird.

### Überprüfen der Registrierung auf ausstehende Dateiumbenennungsvorgänge

Führen Sie das folgende Cmdlet aus, um sicherzustellen, dass auf dem Server kein Neustart aussteht:

```
Get-ItemProperty "HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager" | select PendingFileRenameOperations
```
Wenn die Ausgabe leer ist, können Sie fortfahren. Andernfalls sollten Sie den Server während eines Wartungsfensters neu starten.

Ich starte PowerShell ISE und führe das folgende Skript aus, um die Konfiguration auf dem Server anzuwenden. Dies ist im Grunde eine lokale DSC-Konfiguration, die das lokale Konfigurations-Manager-Modul anweist, die Registrierung beim OMS Automation DSC-Dienst vorzunehmen und die spezifische Konfiguration (ASRMobilityService.localhost) abzurufen.

```
[DSCLocalConfigurationManager()]

Configuration metaconfig
{
    node localhost
    {

        Settings
        {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }
                ResourceRepositoryWeb AzureAutomationDSC
            {
                ServerURL = 'https://we-agentservice-prod-1.azure-automation.net/accounts/4d54e439-8540-4c8f-8e0f-3d7afdda6746'
                RegistrationKey = 'KaTg5zTtI/gf8OGiN28L9FT2gMbowzOv3P9rouQfhxcR+42Mac80QKxOFXYLMdKxRdz8QtJiXLb7noG1U4zJ6g=='
            }
                ConfigurationRepositoryWeb AzureAutomationDSC
            {
                ServerURL = 'https://we-agentservice-prod-1.azure-automation.net/accounts/4d54e439-8540-4c8f-8e0f-3d7afdda6746'
                RegistrationKey = 'KaTg5zTtI/gf8OGiN28L9FT2gMbowzOv3P9rouQfhxcR+42Mac80QKxOFXYLMdKxRdz8QtJiXLb7noG1U4zJ6g=='
                ConfigurationNames = 'ASRMobilityService.localhost'
            }
                ReportServerWeb AzureAutomationDSC
            {
                ServerURL = 'https://we-agentservice-prod-1.azure-automation.net/accounts/4d54e439-8540-4c8f-8e0f-3d7afdda6746'
                RegistrationKey = 'KaTg5zTtI/gf8OGiN28L9FT2gMbowzOv3P9rouQfhxcR+42Mac80QKxOFXYLMdKxRdz8QtJiXLb7noG1U4zJ6g=='
            }

    }
}
metaconfig

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Diese Konfiguration konfiguriert den lokalen Konfigurations-Manager dazu, sich selbst bei OMS Automation DSC zu registrieren, und bestimmt, wie das Modul ausgeführt werden soll, d.h. was im Fall einer Konfigurationsabweichung geschehen soll (ApplyAndAutoCorrect) und dass, falls ein Neustart erforderlich ist, die Konfiguration anschließend fortgesetzt wird.

Nachdem Sie diese Konfiguration ausgeführt haben, sollte der Knoten die Registrierung bei Automation DSC starten.

![Knoten registrieren](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Wenn wir zu „portal.azure.com“ zurückkehren, sehen wir, dass der neu registrierte Knoten jetzt im Portal angezeigt wird.

![Knoten registrieren](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Auf dem Server können wir das folgende PowerShell-Cmdlet ausführen, um sicherzustellen, dass der Knoten korrekt registriert wurde:

```
Get-DscLocalConfigurationManager
```

Sobald die Konfiguration mithilfe von Pull übertragen und auf den Server angewendet wurde, können Sie den Konfigurationsstatus mit dem folgenden Befehl überprüfen:

```
Get-DscConfigurationStatus
```

Die Ausgabe zeigt, dass der Server seine Konfiguration erfolgreich abgerufen hat:

![Knoten registrieren](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Darüber hinaus verfügt das Mobility Service-Setup über ein eigenes Protokoll, das unter „<SystemDrive>\\ProgramData\\ASRSetupLogs“ gespeichert wird.

Das ist schon alles. Wir haben den Mobility Service erfolgreich auf dem Computer, den wir mit Site Recovery schützen möchten, bereitgestellt und registriert, und können dank DSC sicher sein, dass die erforderlichen Dienste immer ausgeführt werden.

![Knoten registrieren](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

Sobald dies vom Verwaltungsserver erkannt wurde, können Sie den Schutz konfigurieren und die Replikation auf dem Computer mit Site Recovery aktivieren.

## Verwenden von DSC in getrennten Umgebungen

Auch wenn Ihre Computer nicht mit dem Internet verbunden sind, können Sie DSC verwenden, um den Mobility Service für die zu schützenden Workloads bereitzustellen und zu konfigurieren.

Eine Möglichkeit besteht darin, einen eigenen DSC-Pull-Server, der im Wesentlichen die gleichen Funktionen wie OMS Automation DSC bietet, in Ihrer Umgebung zu instanziieren. In diesem Fall rufen die Clients die Konfiguration ab, sobald sie beim DSC-Endpunkt registriert wurden. Eine weitere Möglichkeit ist die Verwendung von Push. In diesem Fall übertragen Sie die DSC-Konfiguration lokal oder remote manuell mithilfe von Push auf die Computer.

Beachten Sie, dass in diesem Beispiel ein Parameter für den Computernamen hinzugefügt wurde und sich die Remotedateien jetzt auf einer Remotefreigabe befinden, auf die die zu schützenden Computer Zugriff haben müssen. Am Ende des Skripts starten wir die Konfiguration und beginnen damit, die DSC-Konfiguration auf den Zielcomputer anzuwenden.

### Voraussetzungen

· Das PowerShell-Modul „xPSDesiredStateConfiguration“ muss installiert sein.

Für Windows-Computer, auf denen WMF 5.0 installiert ist, können Sie das Modul „xPSDesiredStateConfiguration“ einfach installieren, indem Sie das folgende Cmdlet auf den Zielcomputern ausführen:

```
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Falls Sie das Modul an Windows-Computer mit WMF 4.0 verteilen müssen, können Sie es auch herunterladen und speichern, indem Sie das folgende Cmdlet auf einem Computer ausführen, auf dem PowerShellGet (WMF 5.0) vorhanden ist:

```
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Stellen Sie auf Computern mit WMF 4.0 außerdem sicher, dass das folgende Update installiert ist:

https://www.microsoft.com/download/details.aspx?id=40749

Die folgende Konfiguration kann mithilfe von Push auf Windows-Computer mit WMF 5.0 und 4.0 übertragen werden.

```
configuration ASRMobilityService {

    param (
    [Parameter(Mandatory=$true)]
    $computername
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = "C:\Temp\Mobility_service\passphrase.txt"
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role ' + '"' + $Role + '"' + ' /InstallLocation ' + '"' + $Install + '"' + ' /CSEndpoint ' + '"' + $CSEndpoint + '"' + ' /PassphraseFilePath ' +  '"' +$LocalPassphrase + '"'  

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $computername {      

        xRemoteFile Setup {
           URI = $RemoteFile
           DestinationPath = $TempDestination
           DependsOn = "[File]Directory"
            }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = "[File]Directory"
            }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = "[File]Directory"
            }

        File Directory {
            DestinationPath = "C:\Temp\ASRSetup"
            Type = "Directory"            
            }

        Archive ASRzip {
           Path = $TempDestination
           Destination = "C:\Temp\ASRSetup"
           DependsOn = "[xRemotefile]Setup"
           }

        Package Install {
            Path = "C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE"
            Ensure = "Present"
            Name = "Microsoft Azure Site Recovery mobility Service/Master Target Server"
            ProductId = "275197FC-14FD-4560-A5EB-38217F80CBD1"
            Arguments = $Arguments
            DependsOn = "[Archive]ASRzip"
            }

        Package AzureAgent {
            Path = "C:\Temp\AzureVmAgent.msi"
            Ensure = "Present"
            Name = "Windows Azure VM Agent - 2.7.1198.735"
            ProductId = "5CF4D04A-F16C-4892-9196-6025EA61F964"
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = "[Package]Install"
            }

        Service ASRvx {
            Name = "svagents"
            State = "Running"
            DependsOn = "[Package]Install"
            }

        Service ASR {
            Name = "InMage Scout Application Service"
            State = "Running"
            DependsOn = "[Package]Install"
            }

        Service AzureAgentService {
            Name = "WindowsAzureGuestAgent"
            State = "Running"
            DependsOn = "[Package]AzureAgent"
            }

        Service AzureTelemetry {
            Name = "WindowsAzureTelemetryService"
            State = "Running"
            DependsOn = "[Package]AzureAgent"
            }
    }
}
ASRMobilityService

Start-DscConfiguration -ComputerName $computername .\ASRMobilityService -wait -force -Verbose
```

Informationen zum Instanziieren eines eigenen DSC-Pull-Servers innerhalb des Unternehmensnetzwerks, der die gleichen Funktionen wie OMS Automation DSC bietet, finden Sie im folgenden Leitfaden: https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396

## Optional: Bereitstellen der DSC-Konfiguration mithilfe einer Azure Resource Manager-Vorlage

In diesem Artikel haben wir uns bisher damit beschäftigt, wie Sie eine eigene DSC-Konfiguration erstellen, um den Mobility Service und den Azure-VM-Agent automatisch bereitzustellen und sicherzustellen, dass sie auf den zu schützenden Computern ausgeführt werden. Zusätzlich ist eine Azure Resource Manager-Vorlage verfügbar, die diese DSC-Konfiguration in einem neuen oder vorhandenen Azure Automation-Konto bereitstellt. Dabei werden Automation-Ressourcen erstellt, die die Variablen für Ihre Umgebung in Form von Eingabeparametern in der Vorlage enthalten.

Nach der Bereitstellung können Sie einfach wie in Schritt 4 dieses Leitfadens beschrieben mit dem Onboarding Ihrer Computer fortfahren.

Die Vorlage führt folgende Schritte aus:

· Sie verwendet ein vorhandenes OMS Automation-Konto oder erstellt ein neues Konto.

· Sie akzeptiert Eingabeparameter für Folgendes:

	· ASRRemoteFile – the location where you have stored the Mobility Service setup
	· ASRPassphrase – the location where you have stored the passphrase.txt file
	· ASRCSEndpoint – the IP address of your Management server
	· Import xPSDesiredStateConfiguration PowerShell module
	· Create and compile the DSC configuration


Die obigen Schritte werden alle in der richtigen Reihenfolge ausgeführt, sodass Sie ganz einfach mit dem Onboarding Ihrer zu schützenden Computer beginnen können.

Die Vorlage mit Anleitungen für die Bereitstellung finden Sie unter folgendem Link:

https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC

Führen Sie die Bereitstellung mit PowerShell durch:

```
$OMSAutomationRGname = 'KNOMS'

$DSCJobGuid = (New-Guid).Guid

New-AzureRmResourceGroupDeployment `
                                  -Name "DSC3" `
                                  -ResourceGroupName $OMSAutomationRGname `
                                  -TemplateFile https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json `
                                  -OMSAutomationAccountName KNOMSAA `
                                  -ASRRemoteFile "https://knrecstor01.blob.core.windows.net/asr/ASR.zip" `
                                  -ASRRemotePassphrase "https://knrecstor01.blob.core.windows.net/asr/passphrase.txt" `
                                  -ASRCSEndpoint '10.0.0.115' `
                                  -DSCJobGuid $DSCJobGuid `
                                  -Verbose
```

## Nächste Schritte:

Nachdem Sie die Mobility Service-Agents bereitgestellt haben, können Sie mit dem [Aktivieren der Replikation](site-recovery-vmware-to-azure.md#step-6-replicate-applications) für die virtuellen Computer fortfahren.

<!---HONumber=AcomDC_0810_2016-->