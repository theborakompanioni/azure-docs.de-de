<properties
    pageTitle="Konfigurieren eines ausführenden Kontos in Azure | Microsoft Azure"
    description="Mit diesem Tutorial werden Sie schrittweise durch die Erstellung, das Testen und eine Beispielverwendung der Authentifizierung per Sicherheitsprinzipal in Azure Automation geführt."
    services="automation"
    documentationCenter=""
    authors="mgoedtel"
    manager="jwhit"
    editor=""
	keywords="Dienstprinzipalname, setspn, Azure-Authentifizierung"/>
<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/09/2016"
    ms.author="magoedte"/>

# Authentifizieren von Runbooks mit der Azure-Option „Ausführendes Konto“
In diesem Thema wird veranschaulicht, wie Sie ein Automation-Konto im Azure-Portal konfigurieren. Hierfür verwenden Sie das neue Feature „Ausführendes Konto“ (auch als Dienstprinzipal bezeichnet), um in Ihrem Abonnement mit Automation-Runbooks auf Azure Resource Manager-Ressourcen zuzugreifen. Wenn Sie im Azure-Portal ein neues Automation-Konto erstellen, wird standardmäßig automatisch ein neuer Dienstprinzipal erstellt und der Rolle „Mitwirkender“ der rollenbasierten Zugriffssteuerung (RBAC) im Abonnement zugewiesen. Dies vereinfacht den Prozess für Sie und ermöglicht Ihnen das schnelle Erstellen und Bereitstellen von Runbooks als Unterstützung für Ihre Anforderungen an die Automation.

Mit einem Dienstprinzipal haben Sie folgende Möglichkeiten:

* Bereitstellen eines Standardverfahrens zum Authentifizieren für Azure beim Verwalten von Azure Resource Manager-Ressourcen mit Runbooks
* Automatisieren der Verwendung globaler Runbooks, die in Azure Alerts konfiguriert sind


>[AZURE.NOTE] Für das Azure-Feature zum [Integrieren von Warnungen](../azure-portal/insights-receive-alert-notifications.md) mit globalen Automation-Runbooks ist ein Automation-Konto erforderlich, das mit einem Dienstprinzipal konfiguriert ist. Sie können entweder ein Automation-Konto auswählen, für das bereits ein Dienstprinzipalbenutzer definiert ist, oder Sie können ein neues Konto erstellen.



Hier wird gezeigt, wie Sie das Automation-Konto über das Azure-Portal erstellen und ein Konto mit einem ausführenden Konto per Azure PowerShell aktualisieren. Außerdem wird beschrieben, wie Sie diesen Dienstprinzipal für die Authentifizierung in Ihren Runbooks verwenden.

## Erstellen eines neuen Automation-Kontos über das Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um im Azure-Portal ein neues Azure Automation-Konto und einen Dienstprinzipal zu erstellen.

>[AZURE.NOTE] Der Benutzer, der diese Schritte ausführt, *muss* Mitglied der Rolle „Abonnement-Administratoren“ sein.

1. Melden Sie sich im Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie die Option **Automation-Konten**.
3. Klicken Sie im Blatt „Automation-Konten“ auf **Hinzufügen**.<br>![Automation-Konto hinzufügen](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. Geben Sie im Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement für das neue Konto sowie eine neue oder vorhandene **Ressourcengruppe** und den **Speicherort** eines Azure-Rechenzentrums an.
6. Stellen Sie sicher, dass **Ja** für die Option **Ausführendes Azure-Konto erstellen** ausgewählt ist, und klicken Sie auf die Schaltfläche **Erstellen**.  

    ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/add-account-decline-create-runas-msg.png)

    >[AZURE.NOTE] Wenn Sie das ausführende Konto nicht erstellen möchten und die Option **Nein** wählen, wird im Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt. Das Konto wird zwar erstellt und der Rolle **Mitwirkender** des Abonnements zugewiesen, es verfügt aber über keine entsprechende Authentifizierungsidentität in Ihrem Abonnementverzeichnisdienst und somit auch nicht über Zugriffsressourcen in Ihrem Abonnement. So wird verhindert, dass Runbooks, die auf dieses Konto verweisen, sich authentifizieren und Aufgaben für Azure Resource Manager-Ressourcen durchführen können.

    ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

    >[AZURE.NOTE] Wenn Sie eine Fehlermeldung der Art „Zugriff verweigert“ erhalten, nachdem Sie auf die Schaltfläche **Erstellen** geklickt haben, liegt dies daran, dass Ihr Konto kein Mitglied der Rolle „Abonnement-Administratoren“ ist.

7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

### Enthaltene Ressourcen
Nach Abschluss der Erstellung des Automation-Kontos werden automatisch verschiedene Ressourcen erstellt. Diese sind in der folgenden Tabelle zusammengefasst:

Ressource|Beschreibung 
----|----
AzureAutomationTutorial-Runbook|Ein Beispielrunbook, das die Authentifizierung mithilfe des ausführenden Kontos und die Anzeige der ersten zehn Azure-VMs in Ihrem Abonnement veranschaulicht.
AzureRunAsCertificate|Die Zertifikatressource, die erstellt wird, wenn Sie sich entweder bei der Erstellung des Automation-Kontos für die Erstellung des ausführenden Kontos entschieden haben oder das folgende PowerShell-Skript für ein vorhandenes Konto verwenden. Dieses Zertifikat ist ein Jahr lang gültig. 
AzureRunAsConnection|Die Verbindungsressource, die erstellt wird, wenn Sie sich entweder bei der Erstellung des Automation-Kontos für die Erstellung des ausführenden Kontos entschieden haben oder das folgende PowerShell-Skript für ein vorhandenes Konto verwenden.
Module|15 Module mit Cmdlets für Azure, PowerShell und Automation, die Sie umgehend in Ihren Runbooks verwenden können.  

## Aktualisieren eines Automation-Kontos mit PowerShell
Mit dem unten angegebenen Verfahren wird ein vorhandenes Automation-Konto aktualisiert und der Dienstprinzipal mit PowerShell erstellt. Dieses Verfahren ist erforderlich, wenn Sie ein Konto erstellt, die Erstellung von „Ausführendes Konto“ aber abgelehnt haben.

Stellen Sie vor dem Fortfahren Folgendes sicher:

1. Sie haben [Windows Management Framework (WMF) 4.0](https://www.microsoft.com/download/details.aspx?id=40855) heruntergeladen und installiert, wenn Sie Windows 7 ausführen. Wenn Sie Windows Server 2012 R2, Windows Server 2012, Windows 2008 R2, Windows 8.1 und Windows 7 SP1 ausführen, ist [Windows Management Framework 5.0](https://www.microsoft.com/download/details.aspx?id=50395) für die Installation verfügbar.
2. Azure PowerShell 1.0. Informationen zu dieser Version und deren Installation finden Sie unter [Gewusst wie: Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md). 
3. Sie haben ein Automation-Konto erstellt. Dieses Konto wird unten im Skript als Wert für die Parameter -AutomationAccountName und -ApplicationDisplayName angegeben.


Mit dem PowerShell-Skript wird Folgendes konfiguriert:

* Eine Azure AD-Anwendung, die mit dem selbstsignierten Zertifikat authentifiziert wird, und ein Dienstprinzipalkonto für diese Anwendung in Azure AD. Außerdem wird die Rolle „Mitwirkender“ (kann auch in „Besitzer“ oder eine andere Rolle geändert werden) für dieses Konto in Ihrem aktuellen Abonnement zugewiesen. Weitere Informationen finden Sie im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](../automation/automation-role-based-access-control.md).  
* Ein Automation-Zertifikatobjekt im angegebenen Automation-Konto mit dem Namen **AzureRunAsCertificate**, in dem das im Dienstprinzipal verwendete Zertifikat enthalten ist.
* Ein Automation-Verbindungsobjekt im angegebenen Automation-Konto mit dem Namen **AzureRunAsConnection**, das die Elemente „applicationId“, „tenantId“, „subscriptionId“ und den Zertifikatfingerabdruck enthält.  


### Ausführen des PowerShell-Skripts

1. Speichern Sie das folgende Skript auf dem Computer. Speichern Sie es in diesem Beispiel unter dem Dateinamen **New-AzureServicePrincipal.ps1**.  

    ```
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
    Export-PfxCertificate -Cert ("Cert:\localmachine\my" + $Cert.Thumbprint) -FilePath $CertPath -Password $CertPassword -Force | Write-Verbose

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
    While ($NewRole -eq $null -and $Retries -le 2)
    {
      # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
      Sleep 5
      New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
      Sleep 5
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
    ```
<br>
2. Starten Sie **Windows PowerShell** auf Ihrem Computer über den **Startbildschirm** mit erhöhten Benutzerrechten.
3. Navigieren Sie über die PowerShell-Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das in Schritt 1 erstellte Skript enthält, und führen Sie das Skript aus. Ändern Sie dabei die Werte für die Parameter *–ResourceGroup*, *-AutomationAccountName*, *-ApplicationDisplayName*, *-SubscriptionId* und *-CertPlainPassword*.<br>

    ```
    .\New-AzureServicePrincipal.ps1 -ResourceGroup <ResourceGroupName> 
     -AutomationAccountName <NameofAutomationAccount> `
     -ApplicationDisplayName <DisplayNameofAutomationAccount> `
     -SubscriptionId <SubscriptionId> `
     -CertPlainPassword "<StrongPassword>"
    ```   
<br>

    >[AZURE.NOTE] Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich *unbedingt* mit einem Konto an, das im Abonnement als Dienstadministrator eingerichtet ist. <br>
4. Nachdem das Skript erfolgreich abgeschlossen wurde, können Sie mit dem nächsten Abschnitt fortfahren, um die Konfiguration der neuen Anmeldeinformationen zu testen und zu überprüfen.

### Überprüfen der Authentifizierung
Als Nächstes führen wir einen kleinen Test durch, um zu bestätigen, dass Sie sich mit dem neuen Dienstprinzipal erfolgreich authentifizieren können. Wenn Sie sich nicht erfolgreich authentifizieren können, müssen Sie wieder mit Schritt 1 beginnen und alle Schritte erneut durchführen.

1. Öffnen Sie im Azure-Portal das zuvor erstellte Automation-Konto.  
2. Klicken Sie auf die Kachel **Runbooks**, um die Liste mit den Runbooks zu öffnen.
3. Erstellen Sie ein neues Runbook, indem Sie auf die Schaltfläche **Runbook hinzufügen** klicken, und wählen Sie dann auf dem Blatt **Runbook hinzufügen** die Option **Neues Runbook erstellen** aus.
4. Geben Sie dem Runbook den Namen *Test-SecPrin-Runbook*, und wählen Sie unter **Runbooktyp** die Option „PowerShell“ aus. Klicken Sie auf **Erstellen**, um das Runbook zu erstellen.
5. Fügen Sie auf dem Blatt **PowerShell-Runbook bearbeiten** im Zeichenbereich den folgenden Code hinzu:<br>

    ```
     $Conn = Get-AutomationConnection -Name AzureRunAsConnection 
     Add-AzureRMAccount -ServicePrincipal -Tenant $Conn.TenantID `
     -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint
    ```  
<br>
6. Klicken Sie auf **Speichern**, um das Runbook zu speichern.
7. Klicken Sie auf **Testbereich**, um das Blatt **Test** zu öffnen.
8. Klicken Sie auf **Starten**, um den Test zu starten.
9. Ein [Runbookauftrag](automation-runbook-execution.md) wird erstellt, und der dazugehörige Status wird angezeigt.  
10. Der Auftrag besitzt zunächst den Status *In Warteschlange*, um anzugeben, dass der Auftrag darauf wartet, dass in der Cloud ein Runbook Worker verfügbar wird. Wird der Auftrag von einem Worker übernommen, wechselt der Status zu *Wird gestartet...* und anschließend zu *Wird ausgeführt...*, wenn die Ausführung des Runbooks tatsächlich gestartet wurde.  
11. Nach Abschluss des Runbookauftrags wird die Ausgabe angezeigt. In unserem Fall sollte der Status **Abgeschlossen** angezeigt werden.<br> ![Sicherheitsprinzipal-Runbooktest](media/automation-sec-configure-azure-runas-account/runbook-test-results.png)<br>
12. Schließen Sie das Blatt **Test**, um zum Zeichenbereich zurückzukehren.
13. Schließen Sie das Blatt **PowerShell-Runbook bearbeiten**.
14. Schließen Sie das Blatt **Test-SecPrin-Runbook**.

## Beispielcode für die Authentifizierung mit Resource Manager-Ressourcen

Mit dem weiter unten angegebenen aktualisierten Beispielcode aus dem AzureAutomationTutorial-Beispielrunbook können Sie für die Authentifizierung das Ausführungskonto verwenden und Resource Manager-Ressourcen mit Ihren Runbooks verwalten.

   ```
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
   ```

Das Skript enthält zwei zusätzliche Codezeilen, um Verweise auf einen Abonnementkontext zu unterstützen und so die problemlose Verwendung mehrerer Abonnements zu ermöglichen. Eine Variablenressource namens „SubscriptionId“ enthält die ID des Abonnements, und nach der Add-AzureRmAccount-Cmdlet-Anweisung wird das [Set-AzureRmContext-Cmdlet](https://msdn.microsoft.com/library/mt619263.aspx) mit dem Parametersatz *-SubscriptionId* angegeben. Sollte der Variablenname zu allgemein sein, können Sie den Namen der Variablen mit einem Präfix versehen oder eine andere Namenskonvention anwenden, um einen aussagekräftigeren Variablennamen zu erhalten. Alternativ können Sie anstelle von „-SubscriptionId“ auch den Parametersatz „-SubscriptionName“ mit einer entsprechenden Variablenressource verwenden.

## Nächste Schritte
- Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md).
- Weitere Informationen zur rollenbasierten Zugriffssteuerung in Azure Automation finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](../automation/automation-role-based-access-control.md).

<!---HONumber=AcomDC_0615_2016-->