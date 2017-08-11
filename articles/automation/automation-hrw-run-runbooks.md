---
title: "Ausführen von Runbooks für Hybrid Runbook Worker in Azure Automation | Microsoft-Dokumentation"
description: "Dieser Artikel enthält Informationen zur Ausführung von Runbooks auf Computern in Ihrem lokalen Datencenter oder bei Ihrem Cloudanbieter mit der Hybrid Runbook Worker-Rolle."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: 06227cda-f3d1-47fe-b3f8-436d2b9d81ee
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/22/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 993bc3ea480a329541ca4ae825189cdb5a2b4a8b
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---

# <a name="running-runbooks-on-a-hybrid-runbook-worker"></a>Ausführen von Runbooks auf einem Hybrid Runbook Worker 
Es gibt keine Unterschiede in der Struktur von Runbooks, die in Azure Automation oder auf einem Hybrid-Runbook-Worker ausgeführt werden. Die jeweils verwendeten Runbooks unterscheiden sich aber meist deutlich, da Runbooks für einen Hybrid Runbook Worker Ressourcen normalerweise auf dem lokalen Computer selbst oder in der lokalen Umgebung der Bereitstellung verwalten, während Runbooks in Azure Automation Ressourcen in der Regel in der Azure-Cloud verwalten.

Sie können ein Runbook für Hybrid-Runbook-Worker in Azure Automation bearbeiten, es ist jedoch möglicherweise schwierig, das Runbook im Editor zu testen.  Die PowerShell-Module für den Zugriff auf die lokalen Ressourcen sind möglicherweise nicht in Ihrer Azure Automation-Umgebung installiert, sodass beim Testen Fehler auftreten.  Wenn Sie die erforderlichen Module installieren, wird das Runbook ausgeführt, der Test kann aufgrund des fehlenden Zugriffs auf die lokalen Ressourcen jedoch nicht vollständig ausgeführt werden.

## <a name="starting-a-runbook-on-hybrid-runbook-worker"></a>Starten eines Runbooks auf einem Hybrid Runbook Worker
[Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md) werden die verschiedenen Methoden zum Starten eines Runbooks beschrieben.  Hybrid-Runbook-Worker fügt die Option **RunOn** hinzu, mit der Sie den Namen einer Hybrid-Runbook-Workergruppe angeben können.  Beim Angeben einer Gruppe wird das Runbook von den Workern in dieser Gruppe abgerufen und ausgeführt.  Wird diese Option nicht angegeben, erfolgt die Ausführung in normaler Weise in Azure Automation.

Wenn Sie ein Runbook im Azure-Portal starten, wird die Option **Ausführen auf** angezeigt. Sie können entweder **Azure** oder **Hybrid Worker** auswählen.  Bei Auswahl von **Hybrid-Worker** können Sie anschließend aus einer Dropdownliste die gewünschte Gruppe auswählen.

Verwenden Sie den Parameter **RunOn**.  Mit dem folgenden Befehl wird mithilfe von Windows PowerShell ein Runbook namens „Test-Runbook“ auf einer Hybrid Runbook Worker-Gruppe mit dem Namen „MyHybridGroup“ gestartet.

    Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

> [!NOTE]
> Der Parameter **RunOn** wurde dem Cmdlet **Start-AzureAutomationRunbook** in Version 0.9.1 von Microsoft Azure PowerShell hinzugefügt.  Sie sollten die [neueste Version herunterladen](https://azure.microsoft.com/downloads/) , wenn auf Ihrem System eine Vorgängerversion installiert ist.  Sie müssen diese Version nur auf der Arbeitsstation installieren, auf der Sie das Runbook aus Windows PowerShell starten.  Eine Installation auf dem Workercomputer ist nur erforderlich, wenn Sie Runbooks von diesem Computer starten möchten.  Sie können ein Runbook auf einem Hybrid-Runbook-Worker derzeit nicht aus einem anderen Runbook starten, da hierzu die neueste Version von Azure PowerShell in Ihrem Automation-Konto installiert sein müsste.  Die neueste Version wird automatisch in Azure Automation aktualisiert und kurz darauf automatisch per Push auf die Worker übertragen.
>
>

## <a name="runbook-permissions"></a>Runbookberechtigungen
Für Runbooks, die auf einem Hybrid Runbook Worker ausgeführt werden, kann nicht dieselbe Methode verwendet werden, mit der Runbooks üblicherweise bei Azure-Ressourcen authentifiziert werden, da diese Runbooks auf Ressourcen außerhalb von Azure zugreifen.  Das Runbook kann entweder eine eigene Authentifizierung für lokale Ressourcen bereitstellen, oder Sie können ein „RunAs“-Konto angeben, um einen Benutzerkontext für alle Runbooks bereitzustellen.

### <a name="runbook-authentication"></a>Authentifizierung von Runbooks
Runbooks werden standardmäßig im Kontext des lokalen Systemkontos auf dem lokalen Computer ausgeführt. Deshalb müssen sie sich bei Ressourcen authentifizieren, auf die sie zugreifen.  

Sie können die [Anmeldeinformationsobjekte](http://msdn.microsoft.com/library/dn940015.aspx) und [Zertifikatobjekte](http://msdn.microsoft.com/library/dn940013.aspx) in Ihrem Runbook gemeinsam mit Cmdlets zur Angabe von Anmeldeinformationen verwenden, um eine Authentifizierung für verschiedene Ressourcen durchzuführen.  Das folgende Beispiel zeigt einen Teil eines Runbooks, mit dem ein Computer neu gestartet wird.  Es werden Anmeldeinformationen aus einem Anmeldeinformationsobjekt und der Name des Computers aus einem Variablenobjekt abgerufen, anschließend werden diese Werte im Cmdlet "Restart-Computer" eingesetzt.

    $Cred = Get-AzureRmAutomationCredential -ResourceGroupName "ResourceGroup01" -Name "MyCredential"
    $Computer = Get-AzureRmAutomationVariable -ResourceGroupName "ResourceGroup01" -Name  "ComputerName"

    Restart-Computer -ComputerName $Computer -Credential $Cred

Sie können auch [InlineScript](automation-powershell-workflow.md#inlinescript) verwenden, mit dem Sie Codeblöcke mit den im [allgemeinen PSCredential-Parameter](http://technet.microsoft.com/library/jj129719.aspx) angegebenen Anmeldeinformationen auf einem anderen Computer ausführen können.

### <a name="runas-account"></a>„RunAs“-Konto
Um zu vermeiden, dass Runbooks ihre eigene Authentifizierung für lokale Ressourcen bereitstellen müssen, können Sie für eine Hybrid Worker-Gruppe ein **RunAs**-Konto angeben.  Sie geben ein [Anmeldeinformationsobjekt](automation-credentials.md) mit Zugriff auf lokale Ressourcen an. Alle Runbooks verwenden anschließend diese Anmeldeinformationen, wenn die Ausführung auf einem Hybrid Runbook Worker in der Gruppe erfolgt.  

Der Benutzername für die Anmeldeinformationen muss eines der folgenden Formate haben:

* Domäne\Benutzername
* username@domain
* Benutzername (für Konten, die für den lokalen Computer lokal sind)

Verwenden Sie das folgende Verfahren, um ein „RunAs“-Konto für eine Hybrid Worker Gruppe anzugeben:

1. Erstellen Sie ein [Anmeldeinformationsobjekt](automation-credentials.md) mit Zugriff auf lokale Ressourcen.
2. Wählen Sie im Azure-Portal das Automation-Konto aus.
3. Wählen Sie die Kachel **Hybrid Worker-Gruppen** und dann die Gruppe aus.
4. Wählen Sie **Alle Einstellungen** und dann **Einstellungen für Hybrid Worker-Gruppe** aus.
5. Ändern Sie **Ausführen als** von **Standard** in **Benutzerdefiniert**.
6. Wählen Sie die Anmeldeinformationen aus, und klicken Sie auf **Speichern**.

### <a name="automation-run-as-account"></a>Ausführendes Automation-Konto
Im Rahmen Ihres automatisierten Buildprozesses für die Bereitstellung von Ressourcen in Azure ist es unter Umständen erforderlich, auf lokale Systeme zuzugreifen, um einen Task oder eine Reihe von Schritten in Ihrer Bereitstellungssequenz zu unterstützen.  Um die Authentifizierung bei Azure mit einem ausführenden Konto zu unterstützen, müssen Sie das Zertifikat für das ausführende Konto installieren.  

Das folgende PowerShell-Runbook, *Export-RunAsCertificateToHybridWorker*, exportiert das Zertifikat für das ausführende Konto aus Ihrem Azure Automation-Konto, lädt es herunter und importiert es in den Zertifikatspeicher für den lokalen Computer auf einem Hybrid Worker, der mit dem gleichen Konto verbunden ist.  Sobald dieser Schritt abgeschlossen ist, überprüft das Runbook, ob der Worker sich unter Verwendung des ausführenden Kontos erfolgreich bei Azure authentifizieren kann.

    <#PSScriptInfo
    .VERSION 1.0
    .GUID 3a796b9a-623d-499d-86c8-c249f10a6986
    .AUTHOR Azure Automation Team
    .COMPANYNAME Microsoft
    .COPYRIGHT 
    .TAGS Azure Automation 
    .LICENSEURI 
    .PROJECTURI 
    .ICONURI 
    .EXTERNALMODULEDEPENDENCIES 
    .REQUIREDSCRIPTS 
    .EXTERNALSCRIPTDEPENDENCIES 
    .RELEASENOTES
    #>

    <#  
    .SYNOPSIS  
    Exports the Run As certificate from an Azure Automation account to a hybrid worker in that account. 
  
    .DESCRIPTION  
    This runbook exports the Run As certificate from an Azure Automation account to a hybrid worker in that account.
    Run this runbook in the hybrid worker where you want the certificate installed.
    This allows the use of the AzureRunAsConnection to authenticate to Azure and manage Azure resources from runbooks running in the hybrid worker.

    .EXAMPLE
    .\Export-RunAsCertificateToHybridWorker

    .NOTES
    AUTHOR: Azure Automation Team 
    LASTEDIT: 2016.10.13
    #>

    [OutputType([string])] 

    # Set the password used for this certificate
    $Password = "YourStrongPasswordForTheCert"

    # Stop on errors
    $ErrorActionPreference = 'stop'

    # Get the management certificate that will be used to make calls into Azure Service Management resources
    $RunAsCert = Get-AutomationCertificate -Name "AzureRunAsCertificate"
       
    # location to store temporary certificate in the Automation service host
    $CertPath = Join-Path $env:temp  "AzureRunAsCertificate.pfx"
   
    # Save the certificate
    $Cert = $RunAsCert.Export("pfx",$Password)
    Set-Content -Value $Cert -Path $CertPath -Force -Encoding Byte | Write-Verbose 

    Write-Output ("Importing certificate into $env:computername local machine root store from " + $CertPath)
    $SecurePassword = ConvertTo-SecureString $Password -AsPlainText -Force
    Import-PfxCertificate -FilePath $CertPath -CertStoreLocation Cert:\LocalMachine\My -Password $SecurePassword -Exportable | Write-Verbose

    # Test that authentication to Azure Resource Manager is working
    $RunAsConnection = Get-AutomationConnection -Name "AzureRunAsConnection" 
    
    Add-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $RunAsConnection.TenantId `
      -ApplicationId $RunAsConnection.ApplicationId `
      -CertificateThumbprint $RunAsConnection.CertificateThumbprint | Write-Verbose

    Set-AzureRmContext -SubscriptionId $RunAsConnection.SubscriptionID | Write-Verbose

    # List automation accounts to confirm Azure Resource Manager calls are working
    Get-AzureRmAutomationAccount | Select AutomationAccountName

Speichern Sie das *Export-RunAsCertificateToHybridWorker*-Runbook mit der Erweiterung `.ps1` auf Ihrem Computer.  Importieren Sie das Runbook in Ihr Automation-Konto, und bearbeiten Sie es. Ändern Sie dabei den Wert der Variablen `$Password` in Ihr eigenes Kennwort.  Veröffentlichen Sie das Runbook, und führen Sie es dann mit der Hybrid Worker-Gruppe als Ziel aus, die Runbooks über das ausführende Konto ausführt und authentifiziert.  Der Auftragsdatenstrom meldet den Versuch, das Zertifikat in den lokalen Computerspeicher zu importieren. Im Bericht folgen mehrere Zeilen, je nachdem, wie viele Automation-Konten in Ihrem Abonnement definiert sind und ob die Authentifizierung erfolgreich ist.  

## <a name="troubleshooting-runbooks-on-hybrid-runbook-worker"></a>Problembehandlung bei Runbooks auf Hybrid Runbook Worker
Protokolle werden lokal auf jedem Hybridworker unter "C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes" gespeichert.  Außerdem zeichnet der Hybrid Worker Fehler und Ereignisse im Windows-Ereignisprotokoll unter **Anwendungs- und Dienstprotokolle\Microsoft-SMA\Betriebsbereit** auf.  Auf Runbooks bezogene Ereignisse, die auf dem Worker ausgeführt werden, werden in **Anwendungs- und Dienstprotokolle\Microsoft-Automation\Betriebsbereit** geschrieben.  Das Protokoll **Microsoft-SMA** enthält viele weitere Ereignisse zu dem Runbookauftrag, der an den Worker übermittelt wird, und zur Verarbeitung des Runbooks.  Das Ereignisprotokoll **Microsoft-Automation** verfügt zwar nicht über viele Ereignisse mit Details zur Runbookausführung, die für die Problembehandlung hilfreich sind, aber es enthält wenigstens die Ergebnisse des Runbookauftrags.  

[Runbookausgabe und -meldungen](automation-runbook-output-and-messages.md) werden von Hybridworkern an Azure Automation gesendet, genau wie Runbookaufträge, die in der Cloud ausgeführt werden.  Sie können auch die Verbose- und Progress-Streams auf dieselbe Weise aktivieren wie für andere Runbooks.  

Wenn Ihre Runbooks nicht erfolgreich ausgeführt werden und in der Auftragsübersicht der Status **Angehalten**angezeigt wird, lesen Sie den Artikel zur Problembehandlung [Hybrid Runbook Worker: Ein Runbookauftrag wird mit dem Status „Angehalten“ beendet](automation-troubleshooting-hybrid-runbook-worker.md#a-runbook-job-terminates-with-a-status-of-suspended).   

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu den verschiedenen Methoden, die zum Starten eines Runbooks verwendet werden können, finden Sie unter [Starten eines Runbooks in Azure Automation](automation-starting-a-runbook.md).  
* Informationen zu den verschiedenen Verfahren, wie Sie in Azure Automation mit PowerShell und PowerShell-Workflow-Runbooks unter Verwendung des Text-Editors arbeiten können, finden Sie unter [Bearbeiten eines Runbooks in Azure Automation](automation-edit-textual-runbook.md)
