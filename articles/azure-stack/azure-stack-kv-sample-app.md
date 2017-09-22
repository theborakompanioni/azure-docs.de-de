---
title: Abruf von Azure Stack Key Vault-Geheimnissen durch Anwendung zulassen | Microsoft-Dokumentation
description: Verwenden einer Beispiel-App zum Arbeiten mit Azure Stack Key Vault
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 3748b719-e269-4b48-8d7d-d75a84b0e1e5
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/26/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: f586cba0684a0720e82c4bce823adbb5ca13404b
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="sample-application-that-uses-keys-and-secrets-stored-in-a-key-vault"></a>Beispielanwendung, die in einem Schlüsseltresor gespeicherte Schlüssel und Geheimnisse verwendet

In diesem Leitfaden führen Sie eine Beispielanwendung (HelloKeyVault) aus, die Schlüssel und Geheimnisse aus einem Schlüsseltresor in Azure Stack abruft.

## <a name="prerequisites"></a>Voraussetzungen 

Führen Sie die folgenden erforderlichen Schritte entweder über das [Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder auf einem Windows-basierten externen Client aus, wenn Sie [über VPN verbunden](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) sind:

* Installieren Sie [mit Azure Stack kompatible Azure PowerShell-Module](azure-stack-powershell-install.md).  
* Laden Sie die [Tools herunter, die für die Arbeit mit Azure Stack benötigt werden](azure-stack-powershell-download.md). 

## <a name="create-and-get-the-key-vault-and-application-settings"></a>Erstellen und Abrufen der Schlüsseltresor- und Anwendungseinstellungen

Zuerst müssen Sie eine Key Vault-Instanz in Azure Stack erstellen und eine Anwendung in Active Directory registrieren. Die Erstellung und Registrierung kann über das Portal oder PowerShell erfolgen. In diesem Thema wird die Verwendung von PowerShell gezeigt. Dieses PowerShell-Skript erstellt standardmäßig eine neue Anwendung in Active Directory, Sie können jedoch auch eine Ihrer vorhandenen Anwendungen nutzen. Achten Sie darauf, Werte für die Variablen `aadTenantName` und `applicationPassword` anzugeben. Wenn Sie keinen Wert für die Variable `applicationPassword` angeben, generiert dieses Skript ein zufälliges Kennwort. 

```powershell
$vaultName           = 'myVault'
$resourceGroupName   = 'myResourceGroup'
$applicationName     = 'myApp'
$location            = 'local' 

# Password for the application, if not specified, this script will generate a random password during app creation.
$applicationPassword = '' 
                         
# Function to generate a random password for the application.
Function GenerateSymmetricKey()
{
    $key = New-Object byte[](32)
    $rng = [System.Security.Cryptography.RNGCryptoServiceProvider]::Create()
    $rng.GetBytes($key)
    return [System.Convert]::ToBase64String($key)
}

Write-Host 'Please log into your Azure Stack user environment' -foregroundcolor Green

$tenantARM = "https://management.local.azurestack.external"
$aadTenantName = "PLEASE FILL THIS IN WITH YOUR AAD TENANT NAME. FOR EXAMPLE: myazurestack.onmicrosoft.com"

# Configure the Azure Stack operator’s PowerShell environment.
Add-AzureRMEnvironment `
  -Name "AzureStackUser" `
  -ArmEndpoint $tenantARM

Set-AzureRmEnvironment `
  -Name "AzureStackAdmin" `
  -GraphAudience "https://graph.windows.net/"

$TenantID = Get-AzsDirectoryTenantId `
  -AADTenantName $aadTenantName `
  -EnvironmentName AzureStackUser

# Sign-in to the user portal.
Login-AzureRmAccount `
  -EnvironmentName "AzureStackUser" `
  -TenantId $TenantID `
  
$now = [System.DateTime]::Now
$oneYearFromNow = $now.AddYears(1)

$applicationPassword = GenerateSymmetricKey
    
# Create a new AD application.
$identifierUri = [string]::Format("http://localhost:8080/{0}",[Guid]::NewGuid().ToString("N"))
$homePage = "http://contoso.com"

Write-Host "Creating a new AAD Application"
$ADApp = New-AzureRmADApplication `
  -DisplayName $applicationName `
  -HomePage $homePage `
  -IdentifierUris $identifierUri `
  -StartDate $now `
  -EndDate $oneYearFromNow `
  -Password $applicationPassword

Write-Host "Creating a new AAD service principal"
$servicePrincipal = New-AzureRmADServicePrincipal `
  -ApplicationId $ADApp.ApplicationId

# Create a new resource group and a key vault within that resource group.
New-AzureRmResourceGroup `
  -Name $resourceGroupName `
  -Location $location   

Write-Host "Creating vault $vaultName"
$vault = New-AzureRmKeyVault -VaultName $vaultName `
  -ResourceGroupName $resourceGroupName `
  -Sku standard `
  -Location $location

# Specify full privileges to the vault for the application.
Write-Host "Setting access policy"
Set-AzureRmKeyVaultAccessPolicy -VaultName $vaultName `
  -ObjectId $servicePrincipal.Id `
  -PermissionsToKeys all `
  -PermissionsToSecrets all

Write-Host "Paste the following settings into the app.config file for the HelloKeyVault project:"
'<add key="VaultUrl" value="' + $vault.VaultUri + '"/>'
'<add key="AuthClientId" value="' + $servicePrincipal.ApplicationId + '"/>'
'<add key="AuthClientSecret" value="' + $applicationPassword + '"/>'
Write-Host

``` 

Der folgende Screenshot zeigt die Ausgabe des vorhergehenden Skripts:

![app config](media/azure-stack-kv-sample-app/settingsoutput.png)

Notieren Sie die vom Skript zurückgegeben Werte für VaultUrl, AuthClientId und AuthClientSecret. Diese Werte verwenden Sie zum Ausführen der Anwendung „HelloKeyVault“.

## <a name="download-and-run-the-sample-application"></a>Herunterladen und Ausführen der Beispiel-Anwendung

Laden Sie den Beispiel-Schlüsseltresor von der Seite mit den [Azure Key Vault-Clientbeispielen](https://www.microsoft.com/en-us/download/details.aspx?id=45343) herunter. Extrahieren Sie den Inhalt der ZIP-Datei auf Ihrer Entwicklungsarbeitsstation. Der Beispielordner enthält zwei Beispiele. In diesem Thema verwenden wir das Beispiel „HelloKeyVault“. Navigieren Sie zu Microsoft.Azure.KeyVault.Samples > Beispiele > Ordner „HelloKeyVault“, und öffnen Sie die HelloKeyVault-Anwendung in Visual Studio. 

Öffnen Sie die Datei „HelloKeyVault\App.config“, und ersetzen Sie die Werte des <appSettings>-Element durch die VaultUrl-, AuthClientId-, und AuthClientSecret-Werte aus dem vorherigen Skript. Beachten Sie, dass „App.config“ standardmäßig Platzhalter für **AuthCertThumbprint** enthält; Sie verwenden jedoch stattdessen **AuthClientSecret**. Nachdem Sie die Einstellungen ersetzt haben, erstellen Sie die Projektmappe neu und starten die Anwendung.

![app settings](media/azure-stack-kv-sample-app/appconfig.png)
 
Die Anwendung meldet sich bei Azure AD an und verwendet dann dieses Token zur Authentifizierung beim Schlüsseltresor in Azure Stack. Die Anwendung führt Vorgänge wie Erstellen, Verschlüsseln, Umschließen, Löschen usw. für die Schlüssel und Geheimnisse des Schlüsseltresors durch. Sie können auch bestimmte Parameter wie „verschlüsseln“, „entschlüsseln“ usw. an die Anwendung übergeben, um sicherzustellen, dass die Anwendung nur diese Vorgänge für den Tresor ausführt. 


## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen eines virtuellen Computers mit einem Key Vault-Kennwort](azure-stack-kv-deploy-vm-with-secret.md)

[Bereitstellen eines virtuellen Computers mit einem Key Vault-Zertifikat](azure-stack-kv-push-secret-into-vm.md)


