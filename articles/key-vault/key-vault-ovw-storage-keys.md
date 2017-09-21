---
ms.assetid: 
title: "Azure Key Vault-Speicherkontoschlüssel"
description: "Speicherkontoschlüssel bieten eine nahtlose Integration zwischen Azure Key Vault und dem schlüsselbasierten Zugriff auf das Azure Storage-Konto."
ms.topic: article
services: key-vault
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 09/14/2017
ms.translationtype: HT
ms.sourcegitcommit: d24c6777cc6922d5d0d9519e720962e1026b1096
ms.openlocfilehash: b7b8583e8923e65ff068a2bec060a27a14905485
ms.contentlocale: de-de
ms.lasthandoff: 09/14/2017

---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Speicherkontoschlüssel

Vor Azure Key Vault-Speicherkontoschlüsseln mussten Entwickler ihre eigenen Azure-Speicherkontoschlüssel (ASA, Azure Storage Account) verwalten und sie manuell oder durch externe Automatisierung rotieren. Key Vault-Speicherkontoschlüssel werden jetzt als [Key Vault-Geheimnisse](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) für die Authentifizierung bei einem Azure Storage-Konto implementiert. 

Das Hauptfeature ASA (Azure Storage Account, Azure-Speicherkonto) verwaltet die Geheimnisrotation für Sie. Es macht außerdem Ihren direkten Kontakt mit einem ASA-Schlüssel überflüssig, indem es Shared Access Signatures (SAS) als Methode anbietet. 

Allgemeinere Informationen zu Azure-Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/storage-create-storage-account) .

## <a name="supporting-interfaces"></a>Unterstützende Schnittstellen

Das Azure-Speicherkontoschlüssel-Feature ist zunächst über die REST-, .NET/C#- und PowerShell-Schnittstellen verfügbar. Weitere Informationen finden Sie in der [Dokumentation zu Key Vault](https://docs.microsoft.com/azure/key-vault/).


## <a name="what-key-vault-manages"></a>Was Key Vault verwaltet

Key Vault führt mehrere interne Verwaltungsfunktionen in Ihrem Auftrag aus, wenn Sie verwaltete Speicherkontoschlüssel verwenden.

- Azure Key Vault verwaltet Schlüssel eines Azure Storage-Kontos.
    - Intern kann Azure Key Vault die Schlüssel mit einem Azure-Speicherkonto auflisten (synchronisieren).  
    - Azure Key Vault generiert (rotiert) die Schlüssel in regelmäßigen Abständen. 
    - Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben. 
    - Azure Key Vault verwaltet Schlüssel von Speicherkonten und klassischen Speicherkonten. 
- Azure Key Vault ermöglicht Ihnen, dem Tresor-/Objektbesitzer, SAS-Definitionen (Konto oder SAS-Dienst) zu erstellen.
    - Der mithilfe der SAS-Definition erstellte SAS-Wert wird über den REST-URI-Pfad als Geheimnis zurückgegeben. Weitere Informationen finden Sie unter [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations) (Azure Key Vault-Speicherkontobetrieb).

## <a name="naming-guidance"></a>Benennungsrichtlinien

- Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.  
- Ein SAS-Definitionsname muss 1-102 Zeichen lang sein und darf nur 0-9, a-Z und A-Z enthalten.

## <a name="developer-experience"></a>Entwicklerumgebung

### <a name="before-azure-key-vault-storage-keys"></a>Vor Azure Key Vault-Speicherschlüsseln 

Entwickler mussten die folgenden Methoden auf einen Speicherkontoschlüssel anwenden, um auf Azure-Speicher zuzugreifen. 
 
```powershell
//create an Azure Storage Account using a connection string containing an account name and a storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Ab Azure Key Vault-Speicherschlüsseln 

```powershell
//Make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Get secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  

-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get a SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If your SAS token is about to expire, Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);
```

 ### <a name="developer-guidance"></a>Anleitung für Entwickler

- Lassen Sie Ihre ASA-Schlüssel nur durch Key Vault verwalten. Versuchen Sie nicht, sie selbst zu verwalten, da Sie mit den Prozessen von Key Vault in Konflikt geraten. 
- Lassen Sie ASA-Schlüssel nicht durch mehrere Key Vault-Objekte verwalten. 
- Wenn Sie Ihre ASA-Schlüssel manuell erneut generieren müssen, sollten Sie sie über Key Vault erneut generieren. 

## <a name="getting-started"></a>Erste Schritte

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Setup für rollenbasierte Zugriffssteuerungsberechtigungen (RBAC)

Key Vault benötigt die Berechtigungen *list* und *regenerate* für die Schlüssel für ein Speicherkonto. Richten Sie diese Berechtigungen mithilfe der folgenden Schritte ein:

- Abrufen der ObjectId von Key Vault: 

    `Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093`
    
     oder
     
    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Zuweisen der Speicherschlüsseloperator-Rolle zur Azure Key Vault-Identität: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Legen Sie für einen klassischen Kontotyp den Rollenparameter auf *Klassische Speicherkontoschlüssel-Operator-Dienstrolle* fest.

## <a name="working-example"></a>Beispiel

Im folgenden Beispiel wird die Erstellung eines durch Key Vault verwalteten Azure-Speicherkontos und der zugehörigen SAS-Definitionen (Shared Access Signature) veranschaulicht.

### <a name="assumptions"></a>Annahmen

Für dieses Beispiel sind folgende Anweisungen vorgegeben.

- Ihre Speicherressource befindet sich unter */subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1*.

- Der Name Ihres Schlüsseltresors lautet *yourtest1*.

### <a name="get-a-service-principal"></a>Abrufen eines Dienstprinzipals

```powershell
Get-AzureRmADServicePrincipal -ServicePrincipalName cfa8b339-82a2-471a-a3c9-0fc0be7a4093
```

Die Ausgabe des obigen Befehls enthält Ihren Dienstprinzipal, der *yourServicePrincipalId* genannt wird. 

### <a name="set-permissions"></a>Festlegen von Berechtigungen

Stellen Sie sicher, dass für die Speicherberechtigungen *Alle* festgelegt ist.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourtest1' -ObjectId yourServicePrincipalId -PermissionsToStorage all
```

### <a name="allow-access"></a>Zugriff zulassen

Sie müssen dem Key Vault-Dienst Zugriff auf die Speicherkonten gewähren, damit Sie ein verwaltetes Speicherkonto und SAS-Definitionen erstellen können.

```powershell
New-AzureRmRoleAssignment -ObjectId yourServicePrincipalId -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '/subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1'
```

### <a name="create-storage-account"></a>Speicherkonto erstellen

Erstellen Sie nun ein verwaltetes Speicherkonto und zwei SAS-Definitionen. Die Konto-SAS gewährt Zugriff auf den Blobdienst mit verschiedenen Berechtigungen.

```powershell
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -DisableAutoRegenerateKey
```

### <a name="regeneration"></a>Erneute Erstellung

Legen Sie mit den folgenden Befehlen den Zeitraum für die erneute Erstellung fest:

```powershell
$regenPeriod = [System.Timespan]::FromDays(3)
Add-AzureKeyVaultManagedStorageAccount -VaultName yourtest1 -Name msak01 -AccountResourceId /subscriptions/subscriptionId/resourceGroups/yourresgroup1/providers/Microsoft.Storage/storageAccounts/yourtest1 -ActiveKeyName key2 -RegenerationPeriod $regenPeriod
```

### <a name="set-sas-definitions"></a>Festlegen von SAS-Definitionen

Legen Sie für Ihr verwaltetes Speicherkonto die SAS-Definitionen in Key Vault fest.

```powershell
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourtest1  -AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List
Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service,Object -VaultName yourtest1  -AccountName msak01 -Name blobsas2 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List,Write
```

### <a name="get-token"></a>Abrufen von Token

Rufen Sie die entsprechenden SAS-Token ab, und führen Sie Aufrufe an den Speicher aus.

```powershell
$sasToken1 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas1).SecretValueText
$sasToken2 = (Get-AzureKeyVaultSecret -VaultName yourtest1 -SecretName msak01-blobsas2).SecretValueText
```

### <a name="create-storage"></a>Erstellen des Speichers

Beachten Sie, dass beim Zugriff mit *$sastoken1* ein Fehler auftritt, der Zugriff mit *$sastoken2* ist jedoch möglich. 

```powershell
$context1 = New-AzureStorageContext -SasToken $sasToken1 -StorageAccountName yourtest1
$context2 = New-AzureStorageContext -SasToken $sasToken2 -StorageAccountName yourtest1
Set-AzureStorageBlobContent -Container containertest1 -File "abc.txt"  -Context $context1
Set-AzureStorageBlobContent -Container cont1-file "file.txt"  -Context $context2
```

### <a name="example-summary"></a>Beispielzusammenfassung

Sie können auf den Speicherblobinhalt mit dem SAS-Token mit Schreibzugriff zugreifen.

### <a name="relevant-powershell-cmdlets"></a>Relevante PowerShell-Cmdlets

- [Get-AzureKeyVaultManagedStorageAccount ](https://docs.microsoft.com/powershell/module/azurerm.keyvault/get-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Add-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Add-AzureKeyVaultManagedStorageAccount?view=azurermps-4.3.1)
- [Get-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Get-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Update-AzureKeyVaultManagedStorageAccountKey](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Update-AzureKeyVaultManagedStorageAccountKey?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageAccount](https://docs.microsoft.com/powershell/module/azurerm.keyvault/remove-azurekeyvaultmanagedstorageaccount?view=azurermps-4.3.1)
- [Remove-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Remove-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)
- [Set-AzureKeyVaultManagedStorageSasDefinition](https://docs.microsoft.com/powershell/module/AzureRM.KeyVault/Set-AzureKeyVaultManagedStorageSasDefinition?view=azurermps-4.3.1)

## <a name="storage-account-onboarding"></a>Onboarding für Speicherkonto 

Beispiel: Als Key Vault-Objektbesitzer fügen Sie Azure Key Vault zur Integration eines Speicherkontos ein Speicherkontoobjekt hinzu.

Während des Onboardings muss Key Vault sicherstellen, dass die Identität des integrierten Kontos über die Speicherschlüssel *list* und *regenerate* verfügt. Zur Überprüfung dieser Berechtigungen ruft Key Vault ein OBO-Token (On Behalf Of, im Auftrag von) vom Authentifizierungsdienst ab, legt die Zielgruppe auf Azure Resource Manager fest und führt einen Aufruf des *list*-Schlüssels an den Azure Storage-Dienst durch. Wenn beim *list*-Aufruf ein Fehler auftritt, tritt bei der Key Vault-Objekterstellung ein Fehler mit dem HTTP-Statuscode *Forbidden* auf. Die auf diese Weise aufgelisteten Schlüssel werden mit Ihrer Schlüsseltresorentitäts-Speicherung zwischengespeichert. 

Key Vault muss sicherstellen, dass die Identität über *regenerate*-Berechtigungen verfügt, bevor Key Vault den Besitz der erneuten Generierung Ihrer Schlüssel übernehmen kann. So wird überprüft, ob sowohl die Identität, über das OBO-Token, als auch die Key Vault-Erstanbieteridentität über diese Berechtigungen verfügt:

- Key Vault listet RBAC-Berechtigungen für die Speicherkontoressource auf.
- Key Vault überprüft die Antwort über den Abgleich von Aktionen und Nichtaktionen mit regulären Ausdrücken. 

Einige unterstützende Beispiele finden Sie unter [Key Vault – Beispiele für verwaltete Speicherkontoschlüssel](https://github.com/Azure/azure-sdk-for-net/blob/psSdkJson6/src/SDKs/KeyVault/dataPlane/Microsoft.Azure.KeyVault.Samples/samples/HelloKeyVault/Program.cs#L167).

Wenn die Identität nicht über die *regenerate*-Berechtigung oder die Key Vault-Erstanbieteridentität nicht über die Berechtigung *list* oder *regenerate* verfügt, tritt bei der Onboardinganforderung ein Fehler auf, und ein entsprechender Fehlercode und eine Nachricht werden zurückgegeben. 

Das OBO-Token funktioniert nur bei Verwendung von nativen Erstanbieter-Clientanwendungen von PowerShell oder CLI.

## <a name="other-applications"></a>Andere Anwendungen

- Mit Key Vault-Speicherkontoschlüsseln erstellte SAS-Token bieten sogar noch stärker kontrollierten Zugriff auf ein Azure-Speicherkonto. Weitere Informationen finden Sie unter [Verwenden von Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1).

## <a name="see-also"></a>Weitere Informationen

- [Informationen zu Schlüsseln, Geheimnissen und Zertifikaten](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault-Teamblog](https://blogs.technet.microsoft.com/kv/)

