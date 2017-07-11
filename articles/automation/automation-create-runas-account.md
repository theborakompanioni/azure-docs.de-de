---
title: "Erstellen eines ausführenden Azure Automation-Kontos | Microsoft-Dokumentation"
description: "In diesem Artikel wird beschrieben, wie Sie mit PowerShell oder über das Portal Ihr Automation-Konto aktualisieren und ausführende Konten erstellen."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/29/2017
ms.author: magoedte
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: 09ddca83fc0f39d7911813e488317f9434fdcfc8
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---

<a id="update-your-automation-account-authentication-with-run-as-accounts" class="xliff"></a>

# Aktualisieren der Automation-Kontoauthentifizierung mit ausführenden Konten 
Sie können Ihr vorhandenes Automation-Konto über das Portal oder mit PowerShell aktualisieren, wenn folgende Voraussetzungen erfüllt werden:

* Sie erstellen ein Automation-Konto, aber lehnen die Erstellung des ausführenden Kontos ab.
* Sie verwenden bereits ein Automation-Konto zum Verwalten von Resource Manager-Ressourcen und möchten das Konto aktualisieren, um das ausführende Konto für die Runbook-Authentifizierung einzuschließen.
* Sie verwenden bereits ein Automation-Konto zum Verwalten klassischer Ressourcen und möchten es zur Verwendung des klassischen ausführenden Kontos aktualisieren, anstatt ein neues Konto zu erstellen und Ihre Runbooks und Assets zu diesem Konto zu migrieren.   
* Sie möchten ein ausführendes Konto und ein klassisches ausführendes Konto erstellen, indem Sie ein Zertifikat nutzen, das von Ihrer Unternehmenszertifizierungsstelle ausgestellt wurde.

<a id="prerequisites" class="xliff"></a>

## Voraussetzungen

* Das Skript kann nur unter Windows 10 und Windows Server 2016 mit Azure Resource Manager-Modul 3.0.0 und höher ausgeführt werden. Unter älteren Windows-Versionen wird das Skript nicht unterstützt.
* Azure PowerShell 1.0 und höher. Informationen zur PowerShell 1.0-Version finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Ein Automation-Konto, auf das im folgenden PowerShell-Skript als Wert für die Parameter *–AutomationAccountName* und *-ApplicationDisplayName* verwiesen wird.

Gehen Sie wie folgt vor, um die Werte für *SubscriptionID*, *ResourceGroup* und *AutomationAccountName* abzurufen. Hierbei handelt es sich um erforderliche Parameter für das Skript:

1. Wählen Sie im Azure-Portal auf dem Blatt **Automation-Konto** Ihr Automation-Konto aus, und wählen Sie dann die Option **Alle Einstellungen**.  
2. Wählen Sie auf dem Blatt **Alle Einstellungen** unter **Kontoeinstellungen** die Option **Eigenschaften**. 
3. Notieren Sie sich die Werte auf dem Blatt **Eigenschaften**.<br><br> ![Blatt „Eigenschaften“ des Automation-Kontos](media/automation-create-runas-account/automation-account-properties.png)  

<a id="required-permissions-to-update-your-automation-account" class="xliff"></a>

### Erforderliche Berechtigungen zum Aktualisieren Ihres Automation-Kontos
Zum Aktualisieren eines Automation-Kontos müssen Sie über die folgenden spezifischen Berechtigungen verfügen, um das Thema abschließen zu können.   
 
* Ihr AD-Benutzerkonto muss zu einer Rolle mit Berechtigungen hinzugefügt werden, die der Rolle „Mitwirkender“ für Microsoft.Automation-Ressourcen entspricht. Dies wird im Artikel [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md#contributor-role-permissions) erläutert.  
* Benutzer ohne Administratorrechte im Azure AD-Mandanten können [AD-Anwendungen registrieren](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions), wenn für die App-Registrierungseinstellung **Ja** festgelegt ist.  Wenn für die App-Registrierungen **Nein** festgelegt ist, muss der Benutzer, der diese Aktion ausführt, ein globaler Administrator in Azure AD sein. 

Wenn Sie kein Mitglied der Active Directory-Instanz des Abonnements sind, bevor Sie der Rolle „Globaler Administrator/Co-Administrator“ des Abonnements hinzugefügt werden, werden Sie Active Directory als Gast hinzugefügt. In diesem Fall erhalten Sie eine Warnung der Art „Sie haben keine Berechtigungen zum Erstellen von…“. auf dem Blatt **Automation-Konto hinzufügen**. Benutzer, die zuerst der Rolle des globalen Administrators/Co-Administrators hinzugefügt wurden, können aus der Active Directory-Instanz des Abonnements entfernt und erneut hinzugefügt werden, um sie als Vollbenutzer in Active Directory einzurichten. Sie können dies im Azure-Portal im Bereich **Azure Active Directory** überprüfen. Wählen Sie hierzu **Benutzer und Gruppen**, **Alle Benutzer** und nach Auswahl des jeweiligen Benutzers die Option **Profil**. Als Wert des Attributs **Benutzertyp** im Benutzerprofil darf nicht **Gast** angegeben sein.

<a id="create-run-as-account-from-the-portal" class="xliff"></a>

## Erstellen eines ausführenden Kontos über das Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um im Azure-Portal Ihr Azure Automation-Konto zu aktualisieren.  Sie können das ausführende Konto und das klassische ausführende Konto einzeln erstellen. Wenn Sie keine Ressourcen im klassischen Azure-Portal verwalten müssen, können Sie auch nur das ausführende Azure-Konto erstellen.  

Mit dem Verfahren werden die folgenden Elemente in Ihrem Automation-Konto erstellt.

**Für ausführende Konten:**

* Erstellt eine Azure AD-Anwendung mit einem selbstsignierten Zertifikat, erstellt ein Dienstprinzipalkonto für die Anwendung in Azure AD und weist die Rolle „Mitwirkender“ für das Konto in Ihrem aktuellen Abonnement zu. Sie können diese Einstellung auch in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält die applicationId, tenantId, subscriptionId und den Zertifikatfingerabdruck.

**Für klassische ausführende Konten:**

* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureClassicRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureClassicRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die subscriptionId und den Namen des Zertifikatassets.

1. Melden Sie sich mit einem Konto, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist, beim Azure-Portal an.
2. Wählen Sie auf dem Blatt des Automation-Kontos im Abschnitt **Kontoeinstellungen** die Option **Ausführende Konten** aus.  
3. Abhängig vom benötigten Konto wählen Sie **Ausführendes Azure-Konto** oder **Klassisches ausführendes Azure-Konto** aus.  Nach der Auswahl wird das Blatt **Ausführendes Azure-Konto hinzufügen** oder **Klassisches ausführendes Azure-Konto hinzufügen** angezeigt. Überprüfen Sie die Übersichtsinformationen, und klicken Sie auf **Erstellen**, um mit dem Erstellen des ausführenden Kontos fortzufahren.  
4. Während Azure das ausführende Konto erstellt, können Sie den Fortschritt unter **Benachrichtigungen** im Menü verfolgen. Außerdem wird ein Banner mit dem Hinweis angezeigt, dass das Konto erstellt wird.  Dieser Vorgang kann einige Minuten dauern.  

<a id="create-run-as-account-using-powershell-script" class="xliff"></a>

## Erstellen eines ausführenden Kontos mit einem PowerShell-Skript
Dieses PowerShell-Skript unterstützt folgende Konfigurationen:

* Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat
* Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud

Abhängig von der gewählten Konfigurationsoption werden mit dem Skript die folgenden Elemente erstellt.

**Für ausführende Konten:**

* Erstellt eine Azure AD-Anwendung, die entweder mit dem öffentlichen Schlüssel des selbstsignierten Zertifikats oder des Unternehmenszertifikats exportiert wird, und ein Dienstprinzipalkonto für die Anwendung in Azure AD. Außerdem wird die Rolle „Mitwirkender“ für das Konto in Ihrem aktuellen Abonnement zugewiesen. Sie können diese Einstellung auch in „Besitzer“ oder eine beliebige andere Rolle ändern. Weitere Informationen finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure Automation](automation-role-based-access-control.md).
* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Schlüssel des Zertifikats, der von der Azure AD-Anwendung verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält die applicationId, tenantId, subscriptionId und den Zertifikatfingerabdruck.

**Für klassische ausführende Konten:**

* Erstellt ein Automation-Zertifikatasset mit dem Namen *AzureClassicRunAsCertificate* im angegebenen Automation-Konto. Das Zertifikatasset enthält den privaten Zertifikatschlüssel, der vom Verwaltungszertifikat verwendet wird.
* Erstellt ein Automation-Verbindungsasset mit dem Namen *AzureClassicRunAsConnection* im angegebenen Automation-Konto. Das Verbindungsasset enthält den Abonnementnamen, die subscriptionId und den Namen des Zertifikatassets.

>[!NOTE]
> Wenn Sie eine Option zum Erstellen eines klassischen ausführenden Kontos wählen, müssen Sie nach dem Ausführen des Skripts das öffentliche Zertifikat (Dateinamenerweiterung „.cer“) in den Verwaltungsspeicher für das Abonnement hochladen, unter dem das Automation-Konto erstellt wurde.
> 

1. Speichern Sie das folgende Skript auf dem Computer. In diesem Beispiel speichern Sie es mit dem Dateinamen *New-RunAsAccount.ps1*.

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
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                       [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
            -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
            -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
            Sleep -s 10
            New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
            $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
            $Retries++;
        }
            return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
            return
        }

        Login-AzureRmAccount -Environment $EnvironmentName 
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
           $CertificateName = $AutomationAccountName+$CertifcateAssetName
           $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
           $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
           $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
           CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
             # Create a Run As account by using a service principal
             $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
             $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
             $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
             $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                     "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                     "Then click Upload and upload the .cer format of #CERT#"

              if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
              $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
              $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
              $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
              $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
              $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
              $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
              $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
              $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
              CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. Starten Sie auf Ihrem Computer **Windows PowerShell** über den **Startbildschirm** mit erhöhten Benutzerrechten.
3. Wechseln Sie von der Befehlszeilenshell mit erhöhten Rechten zu dem Ordner, der das in Schritt 1 erstellte Skript enthält.  
4. Führen Sie das Skript aus, indem Sie die Parameterwerte für die benötigte Konfiguration verwenden.

    **Erstellen eines ausführenden Kontos mit einem selbstsignierten Zertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem Unternehmenszertifikat**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    **Erstellen eines ausführenden Kontos und eines klassischen ausführenden Kontos mit einem selbstsignierten Zertifikat in der Azure Government-Cloud**  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > Nach dem Ausführen des Skripts werden Sie aufgefordert, sich gegenüber Azure zu authentifizieren. Melden Sie sich mit einem Konto an, das Mitglied der Rolle „Abonnement-Administratoren“ und Co-Administrator des Abonnements ist.
    >
    >

Beachten Sie nach dem erfolgreichen Ausführen des Skripts Folgendes:
* Falls Sie ein klassisches ausführendes Konto mit einem selbstsignierten öffentlichen Zertifikat (CER-Datei) erstellt haben, führt das Skript die Erstellung durch und speichert es im Ordner für temporäre Dateien auf Ihrem Computer unter dem Benutzerprofil *%USERPROFILE%\AppData\Local\Temp*, das Sie zum Ausführen der PowerShell-Sitzung verwendet haben.
* Wenn Sie ein klassisches ausführendes Konto mit einem öffentlichen Unternehmenszertifikat (CER-Datei) erstellt haben, sollten Sie dieses Zertifikat verwenden. Befolgen Sie die Anleitung zum [Hochladen eines Verwaltungs-API-Zertifikats in das klassische Azure-Portal](../azure-api-management-certs.md), und überprüfen Sie anschließend die Konfiguration der Anmeldeinformationen mit Ressourcen der klassischen Bereitstellung, indem Sie den [Beispielcode für die Authentifizierung mit Ressourcen der klassischen Azure-Bereitstellung](automation-verify-runas-authentication.md#classic-run-as-authentication) verwenden. 
* Falls Sie *kein* klassisches ausführendes Konto erstellt haben, können Sie die Authentifizierung mit Resource Manager-Ressourcen durchführen und die Konfiguration der Anmeldeinformationen überprüfen, indem Sie den [Beispielcode für die Authentifizierung mit Resource Manager-Ressourcen](automation-verify-runas-authentication.md#automation-run-as-authentication) verwenden.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
* Weitere Informationen zu Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md).
* Weitere Informationen zu Zertifikaten und Azure-Diensten finden Sie unter [Übersicht über Zertifikate für Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).
