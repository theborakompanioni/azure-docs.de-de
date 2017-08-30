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
ms.date: 07/25/2017
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 3148088c88236c64e089fd25c98eb8ac7cdcbfea
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="azure-key-vault-storage-account-keys"></a>Azure Key Vault-Speicherkontoschlüssel

Vor Azure Key Vault-Speicherkontoschlüsseln mussten Entwickler ihre eigenen Azure-Speicherkontoschlüssel (ASA, Azure Storage Account) verwalten und sie manuell oder durch externe Automatisierung rotieren. Key Vault-Speicherkontoschlüssel werden jetzt als [Key Vault-Geheimnisse](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates#BKMK_WorkingWithSecrets) für die Authentifizierung bei einem Azure Storage-Konto implementiert. 

Das Hauptfeature ASA verwaltet die Geheimnisrotation für Sie und macht Ihren direkten Kontakt mit einem ASA-Schlüssel überflüssig, indem es Shared Access Signatures (SAS) als Methode anbietet. 

Allgemeinere Informationen zu Azure-Speicherkonten finden Sie unter [Informationen zu Azure-Speicherkonten](https://docs.microsoft.com/azure/storage/storage-create-storage-account) .

## <a name="supporting-interfaces"></a>Unterstützende Schnittstellen

Das Azure-Speicherkontoschlüssel-Feature ist zunächst über die REST-, .NET/C#- und PowerShell-Schnittstellen verfügbar. Weitere Informationen finden Sie in der [Dokumentation zu Key Vault](https://docs.microsoft.com/azure/key-vault/).


## <a name="storage-account-keys-behavior"></a>Verhalten von Speicherkontoschlüsseln

### <a name="what-key-vault-manages"></a>Was Key Vault verwaltet

Key Vault führt mehrere interne Verwaltungsfunktionen in Ihrem Auftrag aus, wenn Sie Speicherkontoschlüssel verwenden.

1. Azure Key Vault verwaltet Schlüssel eines Azure Storage-Kontos. 
    - Intern kann Azure Key Vault die Schlüssel mit einem Azure-Speicherkonto auflisten (synchronisieren).  
    - Azure Key Vault generiert (rotiert) die Schlüssel in regelmäßigen Abständen. 
    - Schlüsselwerte werden nie als Antwort an den Aufrufer zurückgegeben. 
    - Azure Key Vault verwaltet Schlüssel von Speicherkonten und klassischen Speicherkonten. 
2. Azure Key Vault ermöglicht Ihnen, dem Tresor-/Objektbesitzer, SAS-Definitionen (Konto oder SAS-Dienst) zu erstellen. 
    - Der mithilfe der SAS-Definition erstellte SAS-Wert wird über den REST-URI-Pfad als Geheimnis zurückgegeben. Weitere Informationen finden Sie unter [Azure Key Vault storage account operations](https://docs.microsoft.com/rest/api/keyvault/storage-account-key-operations) (Azure Key Vault-Speicherkontobetrieb).

### <a name="naming-guidance"></a>Benennungsrichtlinien

Speicherkontonamen müssen zwischen 3 und 24 Zeichen lang sein und dürfen nur Zahlen und Kleinbuchstaben enthalten.  
 
Ein SAS-Definitionsname muss 1-102 Zeichen lang sein und darf nur 0-9, a-Z und A-Z enthalten.

## <a name="developer-experience"></a>Entwicklerumgebung 

### <a name="before-azure-key-vault-storage-keys"></a>Vor Azure Key Vault-Speicherschlüsseln 

Entwickler mussten die folgenden Methoden auf einen Speicherkontoschlüssel anwenden, um auf Azure-Speicher zuzugreifen. 
 
 ```
//create storage account using connection string containing account name 
// and the storage key 

var storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));
var blobClient = storageAccount.CreateCloudBlobClient();
 ```
 
### <a name="after-azure-key-vault-storage-keys"></a>Ab Azure Key Vault-Speicherschlüsseln 

```
//Please make sure to set storage permissions appropriately on your key vault
Set-AzureRmKeyVaultAccessPolicy -VaultName 'yourVault' -ObjectId yourObjectId -PermissionsToStorage all

//Use PowerShell command to get Secret URI 

Set-AzureKeyVaultManagedStorageSasDefinition -Service Blob -ResourceType Container,Service -VaultName yourKV  
-AccountName msak01 -Name blobsas1 -Protocol HttpsOnly -ValidityPeriod ([System.Timespan]::FromDays(1)) -Permission Read,List

//Get SAS token from Key Vault

var secret = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token. 

var accountSasCredential = new StorageCredentials(secret.Value); 

// Use credentials and the Blob storage endpoint to create a new Blob service client. 

var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null); 

var blobClientWithSas = accountWithSas.CreateCloudBlobClient(); 
 
// If SAS token is about to expire then Get sasToken again from Key Vault and update it.

accountSasCredential.UpdateSASToken(sasToken);

  ```
 
 ### <a name="developer-best-practices"></a>Bewährte Entwicklermethoden 

- Lassen Sie Ihre ASA-Schlüssel nur durch Key Vault verwalten. Versuchen Sie nicht, sie selbst zu verwalten, da Sie mit den Prozessen von Key Vault in Konflikt geraten. 
- Lassen Sie ASA-Schlüssel nicht durch mehrere Key Vault-Objekte verwalten. 
- Wenn Sie Ihre ASA-Schlüssel manuell erneut generieren müssen, sollten Sie sie über Key Vault erneut generieren. 

## <a name="getting-started"></a>Erste Schritte

### <a name="setup-for-role-based-access-control-rbac-permissions"></a>Setup für rollenbasierte Zugriffssteuerungsberechtigungen (RBAC)

Key Vault benötigt die Berechtigungen *list* und *regenerate* für die Schlüssel für ein Speicherkonto. Richten Sie diese Berechtigungen mithilfe der folgenden Schritte ein:

- Abrufen der ObjectId von Key Vault: 

    `Get-AzureRmADServicePrincipal -SearchString "AzureKeyVault"`

- Zuweisen der Speicherschlüsseloperator-Rolle zur Azure Key Vault-Identität: 

    `New-AzureRmRoleAssignment -ObjectId <objectId of AzureKeyVault from previous command> -RoleDefinitionName 'Storage Account Key Operator Service Role' -Scope '<azure resource id of storage account>'`

    >[!NOTE]
    > Legen Sie für einen klassischen Kontotyp den Rollenparameter auf *Klassische Speicherkontoschlüssel-Operator-Dienstrolle* fest.

### <a name="storage-account-onboarding"></a>Onboarding für Speicherkonto 

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

