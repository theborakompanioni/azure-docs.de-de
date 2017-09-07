---
title: "Erstellen einer Identität für die Azure-App mit PowerShell | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie mithilfe von Azure PowerShell eine Azure Active Directory-Anwendung und einen Dienstprinzipal erstellen sowie mittels rollenbasierter Zugriffssteuerung Zugriff auf Ressourcen gewähren. Es wird gezeigt, wie eine Anwendung per Kennwort oder Zertifikat authentifiziert wird."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/28/2017
ms.author: tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 8351217a29af20a10c64feba8ccd015702ff1b4e
ms.openlocfilehash: 9d4ab890c35eebb2e59a9f4fa96843c854636272
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a>Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe von Azure PowerShell

Wenn eine App oder ein Skript Zugriff auf Ressourcen benötigt, können Sie eine Identität für die App einrichten und sie mit ihren eigenen Anmeldeinformationen authentifizieren. Diese Identität wird als Dienstprinzipal bezeichnet. Dieser Ansatz ermöglicht Ihnen Folgendes:

* Sie können der App-Identität Berechtigungen zuweisen, die sich von Ihren eigenen Berechtigungen unterscheiden. In der Regel sind diese Berechtigungen genau auf die Aufgaben der App beschränkt.
* Sie können ein Zertifikat für die Authentifizierung beim Ausführen eines unbeaufsichtigten Skripts verwenden.

In diesem Thema erfahren Sie, wie Sie mithilfe von [Azure PowerShell](/powershell/azure/overview) alle Komponenten und Einstellungen einrichten, die Sie benötigen, um eine Anwendung mit eigenen Anmeldeinformationen und einer eigenen Identität auszuführen.

## <a name="required-permissions"></a>Erforderliche Berechtigungen
Zum Abschließen dieses Themas benötigen Sie sowohl in der Azure Active Directory-Instanz als auch im Azure-Abonnement ausreichende Berechtigungen. Insbesondere müssen Sie eine App in der Azure Active Directory-Instanz erstellen und den Dienstprinzipal einer Rolle zuweisen können. 

Die einfachste Möglichkeit zum Überprüfen, ob Ihr Konto über die erforderlichen Berechtigungen verfügt, ist über das Portal. Siehe [Überprüfen der erforderlichen Berechtigung](resource-group-create-service-principal-portal.md#required-permissions).

Fahren Sie nun mit dem Abschnitt für die gewünschte Authentifizierungsmethode fort:

* [password](#create-service-principal-with-password)
* [Selbstsigniertes Zertifikat](#create-service-principal-with-self-signed-certificate)
* [Zertifikat von einer Zertifizierungsstelle](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a>PowerShell-Befehle

Zum Einrichten eines Dienstprinzipals verwenden Sie diese Befehle:

| Befehl | Beschreibung |
| ------- | ----------- | 
| [New-AzureRmADServicePrincipal](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | Erstellt einen Azure Active Directory-Dienstprinzipal. |
| [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) | Weist dem angegebenen Dienstprinzipal die angegebene RBAC-Rolle im angegebenen Bereich zu. |


## <a name="create-service-principal-with-password"></a>Erstellen eines Dienstprinzipals mit Kennwort

Um einen Dienstprinzipal mit der Rolle „Mitwirkender“ für Ihr Abonnement zu erstellen, verwenden Sie Folgendes: 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Das Beispiel befindet sich 20 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „PrincipalNotFound: Principal {ID} does not exist in the directory.“ (PrincipalNotFound: Prinzipal {ID} ist im Verzeichnis nicht vorhanden.).

Mit dem folgenden Skript können Sie einen anderen Bereich als das standardmäßige Abonnement angeben und die Rollenzuweisung erneut versuchen, falls ein Fehler auftritt:

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Bitte beachten Sie bezüglich des Skripts folgende Punkte:

* Um der Identität Zugriff auf das Standardabonnement zu gewähren, müssen Sie weder den Parameter ResourceGroup noch SubscriptionId angeben.
* Geben Sie den Parameter ResourceGroup nur an, wenn Sie den Bereich der Rollenzuweisung auf eine Ressourcengruppe begrenzen möchten.
*  In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle „Mitwirkender“ hinzu. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).
* Das Skript befindet sich 15 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „PrincipalNotFound: Principal {ID} does not exist in the directory.“ (PrincipalNotFound: Prinzipal {ID} ist im Verzeichnis nicht vorhanden.).
* Wenn Sie dem Dienstprinzipal Zugriff auf weitere Abonnements oder Ressourcengruppen erteilen müssen, führen Sie das `New-AzureRMRoleAssignment`-Cmdlet erneut mit unterschiedlichen Bereichen aus.


### <a name="provide-credentials-through-powershell"></a>Bereitstellen von Anmeldeinformationen über PowerShell
Jetzt müssen Sie sich als Anwendung anmelden, um Vorgänge durchzuführen. Verwenden Sie als Benutzernamen die `ApplicationId`, die Sie für die Anwendung erstellt haben. Verwenden Sie das Kennwort, das Sie beim Erstellen des Kontos angegeben haben. 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-ID}
```

Die Mandanten-ID ist nicht empfindlich, Sie können sie also direkt in Ihr Skript einbetten. Wenn Sie die Mandanten-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a>Erstellen eines Dienstprinzipals mit selbstsigniertem Zertifikat

Um einen Dienstprinzipal mit einem selbstsignierten Zertifikat und der Rolle „Mitwirkender“ für Ihr Abonnement zu erstellen, verwenden Sie Folgendes: 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

Das Beispiel befindet sich 20 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „PrincipalNotFound: Principal {ID} does not exist in the directory.“ (PrincipalNotFound: Prinzipal {ID} ist im Verzeichnis nicht vorhanden.).

Mit dem folgenden Skript können Sie einen anderen Bereich als das standardmäßige Abonnement angeben und die Rollenzuweisung erneut versuchen, falls ein Fehler auftritt. Sie müssen über Azure PowerShell 2.0 unter Windows 10 oder Windows Server 2016 verfügen.

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
```

Bitte beachten Sie bezüglich des Skripts folgende Punkte:

* Um der Identität Zugriff auf das Standardabonnement zu gewähren, müssen Sie weder den Parameter ResourceGroup noch SubscriptionId angeben.
* Geben Sie den Parameter ResourceGroup nur an, wenn Sie den Bereich der Rollenzuweisung auf eine Ressourcengruppe begrenzen möchten.
* In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle „Mitwirkender“ hinzu. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).
* Das Skript befindet sich 15 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „PrincipalNotFound: Principal {ID} does not exist in the directory.“ (PrincipalNotFound: Prinzipal {ID} ist im Verzeichnis nicht vorhanden.).
* Wenn Sie dem Dienstprinzipal Zugriff auf weitere Abonnements oder Ressourcengruppen erteilen müssen, führen Sie das `New-AzureRMRoleAssignment`-Cmdlet erneut mit unterschiedlichen Bereichen aus.

Wenn Sie **nicht mit Windows 10 oder Windows Server 2016 Technical Preview arbeiten**, müssen Sie den [Generator für selbstsignierte Zertifikate](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) aus dem Skriptcenter von Microsoft herunterladen. Extrahieren Sie den Inhalt, und importieren Sie das benötigte Cmdlet.

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
Ersetzen Sie im Skript die folgenden zwei Zeilen, um das Zertifikat zu generieren.
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript
Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Azure Active Directory. Wenn Sie nur über ein einziges Abonnement verfügen, können Sie Folgendes verwenden:

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Anwendungs-ID und Mandanten-ID sind nicht empfindlich, Sie können sie also direkt in Ihr Skript einbetten. Wenn Sie die Mandanten-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Wenn Sie die Anwendungs-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a>Erstellen eines Dienstprinzipals mit Zertifikat von der Zertifizierungsstelle
Um mit einem von einer Zertifizierungsstelle ausgestellten Zertifikat einen Dienstprinzipal zu erstellen, verwenden Sie das folgende Skript:

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId
 
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName
 New-AzureRmADSpCredential -ObjectId $ServicePrincipal.Id -CertValue $KeyValue -StartDate $PFXCert.NotBefore -EndDate $PFXCert.NotAfter
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ObjectId $ServicePrincipal.Id -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

Bitte beachten Sie bezüglich des Skripts folgende Punkte:

* Der Zugriff ist auf das Abonnement beschränkt.
* In diesem Beispiel fügen Sie den Dienstprinzipal der Rolle „Mitwirkender“ hinzu. Informationen zu den anderen Rollen finden Sie unter [RBAC: Integrierte Rollen](../active-directory/role-based-access-built-in-roles.md).
* Das Skript befindet sich 15 Sekunden lang im Ruhezustand und schafft damit Zeit für die Verteilung des neuen Dienstprinzipals in Azure Active Directory. Falls das Skript nicht lange genug wartet, sehen Sie folgende Fehlermeldung: „PrincipalNotFound: Principal {ID} does not exist in the directory.“ (PrincipalNotFound: Prinzipal {ID} ist im Verzeichnis nicht vorhanden.).
* Wenn Sie dem Dienstprinzipal Zugriff auf weitere Abonnements oder Ressourcengruppen erteilen müssen, führen Sie das `New-AzureRMRoleAssignment`-Cmdlet erneut mit unterschiedlichen Bereichen aus.

### <a name="provide-certificate-through-automated-powershell-script"></a>Bereitstellen eines Zertifikats über automatisiertes PowerShell-Skript
Bei jeder Anmeldung als Dienstprinzipal müssen Sie die Mandanten-ID des Verzeichnisses für Ihre AD-App angeben. Ein Mandant ist eine Instanz von Azure Active Directory.

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

Anwendungs-ID und Mandanten-ID sind nicht empfindlich, Sie können sie also direkt in Ihr Skript einbetten. Wenn Sie die Mandanten-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

Wenn Sie die Anwendungs-ID abrufen möchten, verwenden Sie Folgendes:

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a>Ändern von Anmeldeinformationen

Wenn Sie die Anmeldeinformationen für eine AD-App entweder aufgrund einer Gefährdung der Sicherheit oder aufgrund des Ablaufs der Anmeldeinformationen ändern müssen, verwenden Sie die Cmdlets [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) und [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential).

Zum Entfernen aller Anmeldeinformationen für eine Anwendung verwenden Sie Folgendes:

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

Verwenden Sie Folgendes zum Hinzufügen eines Kennworts:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

Zum Hinzufügen eines Zertifikatwerts erstellen Sie ein selbstsigniertes Zertifikat, wie in diesem Thema gezeigt. Verwenden Sie anschließend Folgendes:

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a>Speichern von Zugriffstoken zum Vereinfachen der Anmeldung
Damit Sie die Anmeldeinformationen für den Dienstprinzipal nicht bei jeder Anmeldung angeben müssen, können Sie das Zugriffstoken speichern.

Speichern Sie das Profil, wenn Sie das aktuelle Zugriffstoken in einer späteren Sitzung verwenden möchten.
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
Öffnen Sie das Profil, und untersuchen Sie seinen Inhalt. Beachten Sie, dass es ein Zugriffstoken enthält. Laden Sie das Profil, statt sich manuell anzumelden.
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> Das Zugriffstoken läuft nach einer bestimmten Zeit ab. Die Verwendung eines gespeicherten Profils funktioniert also nur so lange, wie das Token gültig ist.
>  

Alternativ können Sie die REST-Vorgänge über PowerShell aufrufen, um sich anzumelden. Sie können das Zugriffstoken aus der Authentifizierungsantwort abrufen, um es in anderen Vorgängen zu verwenden. Ein Beispiel zum Abrufen des Zugriffstokens durch Aufrufen der REST-Vorgänge finden Sie unter [Generieren eines Zugriffstokens](resource-manager-rest-api.md#generating-an-access-token).

## <a name="debug"></a>Debuggen

Wenn Sie einen Dienstprinzipal erstellen, können folgende Fehler auftreten:

* **„Authentication_Unauthorized“** oder **„Kein Abonnement in diesem Kontext gefunden.“** Dieser Fehler wird angezeigt, wenn Ihr Konto nicht über die [erforderlichen Berechtigungen](#required-permissions) verfügt, um eine App in Azure Active Directory zu registrieren. In der Regel wird dieser Fehler angezeigt, wenn in Ihrem Azure Active Directory nur Administratorbenutzer Apps registrieren können und Ihr Konto kein Administratorkonto ist. Bitten Sie Ihren Administrator, Sie entweder einer Administratorrolle zuzuweisen, oder Benutzern zu ermöglichen, Apps zu registrieren.

* Ihr Konto **„hat keine Berechtigung zum Ausführen der Aktion 'Microsoft.Authorization/roleAssignments/write' über Bereich '/subscriptions/{guid}'.“**  – Dieser Fehler wird angezeigt, wenn Ihr Konto nicht über ausreichende Berechtigungen verfügt, um eine Rolle einer Identität zuzuweisen. Bitten Sie Ihren Abonnementadministrator, Sie der Rolle „Benutzerzugriffsadministrator“ hinzuzufügen.

## <a name="sample-applications"></a>Beispielanwendungen
Informationen zum Anmelden als Anwendung über verschiedene Plattformen finden Sie hier:

* [.NET](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [Java](/java/azure/java-sdk-azure-authenticate)
* [Node.js](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [Python](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a>Nächste Schritte
* Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).
* Eine ausführlichere Erläuterung zu Anwendungen und Dienstprinzipalen finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](../active-directory/active-directory-application-objects.md). 
* Weitere Informationen zur Azure Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](../active-directory/active-directory-authentication-scenarios.md).
* Eine Liste der verfügbaren Aktionen, die Benutzern erteilt oder verweigert werden können, finden Sie unter [Vorgänge für Azure Resource Manager-Ressourcenanbieter](../active-directory/role-based-access-control-resource-provider-operations.md).


