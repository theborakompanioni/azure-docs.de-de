---
title: "Konfigurieren eines ausführenden Kontos in Azure | Microsoft Docs"
description: "Mit diesem Tutorial werden Sie schrittweise durch die Erstellung, das Testen und eine Beispielverwendung der Authentifizierung per Sicherheitsprinzipal in Azure Automation geführt."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
keywords: Dienstprinzipalname, setspn, Azure-Authentifizierung
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/17/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 32afda1861c2c558f8bb9ffedf897cd8d1df0b5c


---
# <a name="authenticate-runbooks-with-azure-run-as-account"></a>Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“
In diesem Thema wird erläutert, wie Sie im Azure-Portal ein Automation-Konto mit dem Feature „Als Konto ausführen“ konfigurieren, um Runbooks zur Verwaltung von Ressourcen in Azure Resource Manager oder Azure Service Management zu authentifizieren.

Wenn Sie ein neues Automation-Konto im Azure-Portal erstellen, wird automatisch Folgendes erstellt:

* Ein ausführendes Konto, das einen neuen Dienstprinzipal in Azure Active Directory sowie ein Zertifikat erstellt und die zum Verwalten von Resource Manager-Ressourcen mit Runbooks verwendete Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) zuweist.   
* Ein klassisches ausführendes Konto, indem ein Verwaltungszertifikat hochgeladen wird, das zum Verwalten von Azure Service Management-Ressourcen oder klassischen Ressourcen mit Runbooks verwendet wird.  

Dies vereinfacht den Prozess für Sie und ermöglicht Ihnen das schnelle Erstellen und Bereitstellen von Runbooks als Unterstützung für Ihre Anforderungen an die Automation.      

Ein ausführendes Konto und ein klassisches ausführendes Konto ermöglichen Ihnen Folgendes:

* Bereitstellen eines Standardverfahrens für die Authentifizierung bei Azure, wenn Azure Resource Manager- oder Azure Service Management-Ressourcen mithilfe von Runbooks im Azure-Portal verwaltet werden  
* Automatisieren der Verwendung globaler Runbooks, die in Azure Alerts konfiguriert sind

> [!NOTE]
> Für das Azure-Feature zum [Integrieren von Warnungen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) mit globalen Automation-Runbooks ist ein Automation-Konto erforderlich, das mit einem ausführenden Konto und einem klassischen ausführenden Konto konfiguriert ist. Sie können entweder ein Automation-Konto auswählen, für das bereits ein ausführendes Konto und ein klassisches ausführendes Konto definiert sind, oder ein neues Konto erstellen.
> 
> 

Im Folgenden erfahren Sie, wie Sie das Automation-Konto im Azure-Portal erstellen, ein Automation-Konto mit PowerShell aktualisieren und die Authentifizierung in Ihren Runbooks verwenden.

Beachten Sie Folgendes, bevor Sie fortfahren.

1. Die hier beschriebenen Schritte haben keine Auswirkung auf vorhandene Automation-Konten, die bereits im klassischen Bereitstellungsmodell oder Resource Manager-Bereitstellungsmodell erstellt wurden.  
2. Die hier beschriebenen Schritte funktionieren nur für über das Azure-Portal erstellte Automation-Konten.  Wenn Sie versuchen, ein Konto über das klassische Portal zu erstellen, wird die Konfiguration des ausführenden Kontos nicht repliziert.
3. Wenn Sie momentan über Runbooks und Assets (d.h. Zeitpläne, Variablen usw.) verfügen, die zuvor zur Verwaltung klassischer Ressourcen erstellt wurden, und diese Runbooks mit dem neuen klassischen ausführenden Konto authentifiziert werden sollen, müssen Sie sie zum neuen Automation-Konto migrieren oder Ihr vorhandenes Konto mit dem unten stehenden PowerShell-Skript aktualisieren.  
4. Zur Authentifizierung mit dem neuen ausführenden Konto und klassischen ausführenden Automation-Konto müssen Sie Ihre vorhandenen Runbooks mit dem folgenden Beispielcode ändern.  **Beachten Sie** , dass das ausführende Konto zur Authentifizierung gegenüber Resource Manager-Ressourcen anhand des zertifikatbasierten Dienstprinzipals dient und das klassische ausführende Konto zur Authentifizierung gegenüber Service Management-Ressourcen mit dem Verwaltungszertifikat.     

## <a name="create-a-new-automation-account-from-the-azure-portal"></a>Erstellen eines neuen Automation-Kontos über das Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um im Azure-Portal ein neues Azure Automation-Konto zu erstellen.  Dadurch werden sowohl das ausführende als auch das klassische ausführende Konto erstellt.  

> [!NOTE]
> Der Benutzer, der diese Schritte ausführt, *muss* ein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements sein, das dem Benutzer Zugriff auf das Abonnement gewährt.  Zudem muss der Benutzer der Active Directory-Standardinstanz dieses Abonnements als Benutzer hinzugefügt werden. Das Konto muss keiner privilegierten Rolle zugewiesen werden.
> 
> 

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Wählen Sie die Option **Automation-Konten**.
3. Klicken Sie auf dem Blatt „Automation-Konten“ auf **Hinzufügen**.<br>![Automation-Konto hinzufügen](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)
   
   > [!NOTE]
   > Wenn auf dem Blatt **Automation-Konto hinzufügen** die folgende Warnung angezeigt wird, ist Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Admin des Abonnements.<br>![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   > 
   > 
4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie ein Abonnement für das neue Konto sowie eine neue oder vorhandene **Ressourcengruppe** und den **Standort** eines Azure-Datencenters an.
6. Stellen Sie sicher, dass **Ja** für die Option **Ausführendes Azure-Konto erstellen** ausgewählt ist, und klicken Sie auf die Schaltfläche **Erstellen**.  
   
   > [!NOTE]
   > Wenn Sie **Nein** auswählen und das ausführende Konto nicht erstellen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt.  Das Konto wird zwar im Azure-Portal erstellt, verfügt aber nicht über eine entsprechende Authentifizierungsidentität in Ihrem klassischen oder Resource Manager-Abonnementverzeichnisdienst und hat somit keinen Zugriff auf Ressourcen in Ihrem Abonnement.  Runbooks, die auf dieses Konto verweisen, können sich daher nicht authentifizieren und keine Aufgaben für Ressourcen in diesen Bereitstellungsmodellen ausführen.
   > 
   > ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)<br>
   >  Wenn der Dienstprinzipal nicht erstellt wird, wird die Rolle „Mitwirkender“ nicht zugewiesen.
   > 
   > 
7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

### <a name="resources-included"></a>Enthaltene Ressourcen
Nach der erfolgreichen Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt.  In der folgenden Tabelle sind die Ressourcen für das ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureAutomationTutorial-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureAutomationTutorialScript-Runbook |Ein PowerShell-Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos veranschaulicht und alle Resource Manager-Ressourcen abruft. |
| AzureRunAsCertificate |Ein Zertifikatasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem unten stehenden PowerShell-Skript für ein vorhandenes Konto erstellt wird.  Das Zertifikat ermöglicht die Authentifizierung bei Azure, sodass Sie Azure Resource Manager-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureRunAsConnection |Ein Verbindungsasset, das während der Erstellung des Automation-Kontos automatisch oder mit dem unten stehenden PowerShell-Skript für ein vorhandenes Konto erstellt wird. |

In der folgenden Tabelle sind die Ressourcen für das klassische ausführende Konto zusammengefasst.<br>

| Ressource | Beschreibung |
| --- | --- |
| AzureClassicAutomationTutorial-Runbook |Ein Beispielrunbook, das alle klassischen VMs in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicAutomationTutorialScript-Runbook |Ein Beispielrunbook, das alle klassischen VMs in einem Abonnement mithilfe des klassischen ausführenden Kontos (Zertifikat) abruft und anschließend den VM-Namen und -Status ausgibt. |
| AzureClassicRunAsCertificate |Ein automatisch erstelltes Zertifikatasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können.  Dieses Zertifikat ist ein Jahr lang gültig. |
| AzureClassicRunAsConnection |Ein automatisch erstelltes Verbindungsasset, das zur Authentifizierung bei Azure verwendet wird, sodass Sie klassische Azure-Ressourcen über Runbooks verwalten können. |

## <a name="verify-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem ausführenden Konto
Als Nächstes führen wir einen kleinen Test durch, um sicherzustellen, dass Sie sich mit dem neuen ausführenden Konto authentifizieren können.     

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)<br>
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Im Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste aller verfügbaren Ressourcen in der Ressourcengruppe zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbook-Blatt **AzureAutomationTutorialScript**.

## <a name="verify-classic-run-as-authentication"></a>Überprüfen der Authentifizierung mit dem klassischen ausführenden Konto
Als Nächstes führen wir einen kleinen Test durch, um sicherzustellen, dass Sie sich mit dem neuen klassischen ausführenden Konto authentifizieren können.     

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Wählen Sie das Runbook **AzureClassicAutomationTutorialScript** aus, und klicken Sie auf **Starten**, um das Runbook zu starten.  Sie werden gefragt, ob Sie das Runbook starten möchten.
4. Es wird ein [Runbook-Auftrag](automation-runbook-execution.md) erstellt, das Blatt „Auftrag“ geöffnet und der Auftragsstatus auf der Kachel **Auftragszusammenfassung** angezeigt.  
5. Der Auftrag besitzt zunächst den Status *In Warteschlange* , um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet* und anschließend zu *Wird ausgeführt*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
6. Wenn der Runbook-Auftrag abgeschlossen ist, sollte der Status **Abgeschlossen** lauten.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
7. Um die detaillierten Ergebnisse des Runbooks anzuzeigen, klicken Sie auf die Kachel **Ausgabe** .
8. Auf dem Blatt **Ausgabe** sollte angezeigt werden, dass die Authentifizierung erfolgreich war und eine Liste mit allen klassischen VMs im Abonnement zurückgegeben wurde.
9. Schließen Sie das Blatt **Ausgabe**, um zum Blatt **Auftragszusammenfassung** zurückzukehren.
10. Schließen Sie das Blatt **Auftragszusammenfassung** und das entsprechende Runbook-Blatt **AzureClassicAutomationTutorialScript**.

## <a name="update-an-automation-account-using-powershell"></a>Aktualisieren eines Automation-Kontos mit PowerShell
Wenn Folgendes zutrifft, können Sie Ihr vorhandenes Automation-Konto mit PowerShell aktualisieren:

1. Sie haben ein Automation-Konto erstellt, die Erstellung des ausführenden Kontos jedoch abgelehnt.
2. Sie haben bereits ein Automation-Konto zum Verwalten von Resource Manager-Ressourcen und möchten es aktualisieren, um das ausführende Konto für die Runbook-Authentifizierung einzuschließen.
3. Sie haben bereits ein Automation-Konto zum Verwalten klassischer Ressourcen und möchten es zur Verwendung des klassischen ausführenden Kontos aktualisieren, anstatt ein neues Konto zu erstellen und Ihre Runbooks und Assets zu diesem Konto zu migrieren.   

Stellen Sie vor dem Fortfahren Folgendes sicher:

1. Sie haben [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) heruntergeladen und installiert, wenn Sie Windows 7 ausführen.   
    Wenn Sie Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 oder Windows 7 SP1 verwenden, ist [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) zur Installation verfügbar.
2. Azure PowerShell 1.0. Informationen zu dieser Version und ihrer Installation finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
3. Sie haben ein Automation-Konto erstellt.  Dieses Konto wird in den beiden unten stehenden Skripts als Wert für die Parameter „-AutomationAccountName“ und „-ApplicationDisplayName“ verwendet.

Wählen Sie zum Ermitteln der Werte für *SubscriptionID*, *ResourceGroup* und *AutomationAccountName* (erforderliche Parameter für die Skripts) im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto und anschließend die Option **Alle Einstellungen** aus.  Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften** aus.  Die Werte werden auf dem Blatt **Eigenschaften** angezeigt.<br> ![Automation-Konto – Eigenschaften](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-run-as-account-powershell-script"></a>PowerShell-Skript zum Erstellen des ausführenden Kontos
Das unten stehende PowerShell-Skript konfiguriert Folgendes:

* Eine Azure AD-Anwendung, die mit dem selbstsignierten Zertifikat authentifiziert wird, und ein Dienstprinzipalkonto für diese Anwendung in Azure AD. Außerdem wird die Rolle „Mitwirkender“ (kann auch in „Besitzer“ oder eine andere Rolle geändert werden) für dieses Konto in Ihrem aktuellen Abonnement zugewiesen.  Weitere Informationen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Ein Automation-Zertifikatsset im angegebenen Automation-Konto mit dem Namen **AzureRunAsCertificate**, das das vom Dienstprinzipal verwendete Zertifikat enthält.
* Ein Automation-Verbindungsasset im angegebenen Automation-Konto mit dem Namen **AzureRunAsConnection**, das die Anwendungs-ID (applicationId), Mandanten-ID (tenantId), Abonnement-ID (subscriptionId) und den Zertifikatfingerabdruck enthält.    

Folgen Sie zum Ausführen des Skripts den unten stehenden Schritten.

1. Speichern Sie das folgende Skript auf dem Computer.  In diesem Beispiel speichern Sie es mit dem Dateinamen **New-AzureServicePrincipal.ps1**.  
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        Select-AzureRmSubscription -SubscriptionId $SubscriptionId
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
   
        $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate -ArgumentList @($CertPath, $CertPlainPassword)
        $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())
   
        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= $EndDate
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.Type = "AsymmetricX509Cert"
        $KeyCredential.Usage = "Verify"
        $KeyCredential.Value = $KeyValue
   
        # Use Key credentials
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $ApplicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $keyCredential
   
        New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId | Write-Verbose
        Get-AzureRmADServicePrincipal | Where {$_.ApplicationId -eq $Application.ApplicationId} | Write-Verbose
   
        $NewRole = $null
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
           Sleep 5
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           Sleep 10
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
   
        # Get the tenant id for this subscription
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
   
        # Create the automation resources
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name AzureRunAsCertificate -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        $ConnectionAssetName = "AzureRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"ApplicationId" = $Application.ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Cert.Thumbprint; "SubscriptionId" = $SubscriptionId}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureServicePrincipal -ConnectionFieldValues $ConnectionFieldValues
2. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.
3. Navigieren Sie über die PowerShell-Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das in Schritt 1 erstellte Skript enthält, und führen Sie das Skript aus. Ändern Sie dabei die Werte für die Parameter *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* und *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Sie müssen sich mit einem Konto anmelden, das ein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Admin des Abonnements ist.
   > 
   > 
   
        .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"  
   <br>

Nachdem das Skript erfolgreich ausgeführt wurde, können Sie den unten stehenden [Beispielcode](#sample-code-to-authenticate-with-resource-manager-resources) zum Authentifizieren gegenüber Resource Manager-Ressourcen und Überprüfen der Anmeldeinformationen verwenden.

### <a name="create-classic-run-as-account-powershell-script"></a>PowerShell-Skript zum Erstellen des klassischen ausführenden Kontos
Das unten stehende PowerShell-Skript konfiguriert Folgendes:

* Ein Automation-Zertifikatasset im angegebenen Automation-Konto mit dem Namen **AzureClassicRunAsCertificate**, das das zum Authentifizieren Ihrer Runbooks verwendete Zertifikat enthält.
* Ein Automation-Verbindungsasset im angegebenen Automation-Konto mit dem Namen **AzureClassicRunAsConnection**, das den Abonnementnamen, die Abonnement-ID (subscriptionId) und den Namen des Zertifikatassets enthält.

Das Skript erstellt ein selbstsigniertes Verwaltungszertifikat und speichert es im Ordner für temporäre Dateien auf Ihrem Computer unter dem zum Ausführen der PowerShell-Sitzung verwendeten Benutzerprofil: *%BENUTZERPROFIL%\AppData\Local\Temp*.  Nach der Ausführung des Skripts müssen Sie das Azure-Verwaltungszertifikat in den Verwaltungsspeicher für das Abonnement hochladen, in dem das Automation-Konto erstellt wurde.  Folgen Sie zum Ausführen des Skripts und Hochladen des Zertifikats den unten stehenden Schritten.  

1. Speichern Sie das folgende Skript auf dem Computer.  In diesem Beispiel speichern Sie es mit dem Dateinamen **New-AzureClassicRunAsAccount.ps1**.
   
        #Requires -RunAsAdministrator
        Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,
   
        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,
   
        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,
   
        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,
   
        [Parameter(Mandatory=$true)]
        [String] $CertPlainPassword,
   
        [Parameter(Mandatory=$false)]
        [int] $NoOfMonthsUntilExpired = 12
        )
   
        Login-AzureRmAccount
        Import-Module AzureRM.Resources
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId
        $SubscriptionName = $subscription.Subscription.SubscriptionName
   
        $CurrentDate = Get-Date
        $EndDate = $CurrentDate.AddMonths($NoOfMonthsUntilExpired)
        $KeyId = (New-Guid).Guid
        $CertPath = Join-Path $env:TEMP ($ApplicationDisplayName + ".pfx")
        $CertPathCer = Join-Path $env:TEMP ($ApplicationDisplayName + ".cer")
   
        $Cert = New-SelfSignedCertificate -DnsName $ApplicationDisplayName -CertStoreLocation cert:\LocalMachine\My -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider"
   
        $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $CertPathCer -Type CERT | Write-Verbose
   
        # Create the automation resources
        $ClassicCertificateAssetName = "AzureClassicRunAsCertificate"
        New-AzureRmAutomationCertificate -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Path $CertPath -Name $ClassicCertificateAssetName  -Password $CertPassword -Exportable | write-verbose
   
        # Create a Automation connection asset named AzureClassicRunAsConnection in the Automation account. This connection uses the ClassicCertificateAssetName.
        $ConnectionAssetName = "AzureClassicRunAsConnection"
        Remove-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -Force -ErrorAction SilentlyContinue
        $ConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicCertificateAssetName}
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $AutomationAccountName -Name $ConnectionAssetName -ConnectionTypeName AzureClassicCertificate -ConnectionFieldValues $ConnectionFieldValues
   
        Write-Host -ForegroundColor red "Please upload the cert $CertPathCer to the Management store by following the steps below."
        Write-Host -ForegroundColor red "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates."
        Write-Host -ForegroundColor red "Then click Upload and upload the certificate $CertPathCer"
2. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.  
3. Navigieren Sie über die PowerShell-Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das in Schritt 1 erstellte Skript enthält, und führen Sie das Skript aus. Ändern Sie dabei die Werte für die Parameter *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* und *-CertPlainPassword*.<br>
   
   > [!NOTE]
   > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Sie müssen sich mit einem Konto anmelden, das ein Mitglied der Rolle „Abonnement-Administratoren“ und Co-Admin des Abonnements ist.
   > 
   > 
   
        .\New-AzureClassicRunAsAccount.ps1 -ResourceGroup <ResourceGroupName>
        -AutomationAccountName <NameofAutomationAccount> `
        -ApplicationDisplayName <DisplayNameofAutomationAccount> `
        -SubscriptionId <SubscriptionId> `
        -CertPlainPassword "<StrongPassword>"

Nachdem das Skript ausgeführt wurde, müssen Sie das im Ordner **Temp** Ihres Benutzerprofils erstellte Zertifikat kopieren.  Führen Sie die Schritte zum [Hochladen eines Verwaltungszertifikats für die Verwaltungs-API](../azure-api-management-certs.md) in das klassische Azure-Portal durch, und verwenden Sie anschließend den unten stehenden [Beispielcode](#sample-code-to-authenticate-with-service-management-resources), um die Anmeldeinformationen für Service Management-Ressourcen zu überprüfen.

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a>Beispielcode für die Authentifizierung mit Resource Manager-Ressourcen
Sie können den unten stehenden aktualisierten Beispielcode aus dem Beispielrunbook **AzureAutomationTutorialScript** verwenden, um sich zum Verwalten von Resource Manager-Ressourcen mit Ihren Runbooks mit dem ausführenden Konto zu authentifizieren.   

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Logging in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }


Das Skript enthält zwei zusätzliche Codezeilen, um Verweise auf einen Abonnementkontext zu unterstützen und so die problemlose Verwendung mehrerer Abonnements zu ermöglichen. Ein Variablenasset namens „SubscriptionId“ enthält die ID des Abonnements, und nach der Cmdlet-Anweisung „Add-AzureRmAccount“ wird das [Set-AzureRmContext-Cmdlet](https://msdn.microsoft.com/library/mt619263.aspx) mit dem Parametersatz *-SubscriptionId*angegeben. Sollte der Variablenname zu allgemein sein, können Sie den Namen der Variablen mit einem Präfix versehen oder eine andere Namenskonvention anwenden, um einen aussagekräftigeren Variablennamen zu erhalten. Alternativ können Sie anstelle von „-SubscriptionId“ auch den Parametersatz „-SubscriptionName“ mit einem entsprechenden Variablenasset verwenden.  

Beachten Sie das zur Authentifizierung im Runbook verwendete Cmdlet – **Add-AzureRmAccount**verwendet den Parametersatz *ServicePrincipalCertificate* .  Die Authentifizierung wird nicht mit Anmeldeinformationen vorgenommen, sondern mit einem Dienstprinzipalzertifikat.  

## <a name="sample-code-to-authenticate-with-service-management-resources"></a>Beispielcode für die Authentifizierung mit Service Management-Ressourcen
Sie können den unten stehenden aktualisierten Beispielcode aus dem Beispielrunbook **AzureClassicAutomationTutorialScript** verwenden, um sich zum Verwalten von klassischen Ressourcen mit Ihren Runbooks mit dem klassischen ausführenden Konto zu authentifizieren.

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID


## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md).
* Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure Automation finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md)




<!--HONumber=Dec16_HO2-->


