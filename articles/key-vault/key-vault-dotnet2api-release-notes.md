---
title: "Key Vault .NET 2.x-API – Versionshinweise | Microsoft-Dokumentation"
description: ".NET-Entwickler verwenden diese API für die Programmierung für Azure Key Vault"
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
editor: bruceper
ms.assetid: 1cccf21b-5be9-4a49-8145-483b695124ba
ms.service: key-vault
ms.devlang: CSharp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/02/2017
ms.author: bruceper
ms.translationtype: Human Translation
ms.sourcegitcommit: db034a8151495fbb431f3f6969c08cb3677daa3e
ms.openlocfilehash: 21b34e30a7e07b37ccae3f101f2ab4dfadfad9bf
ms.contentlocale: de-de
ms.lasthandoff: 04/29/2017


---
# <a name="azure-key-vault-net-20---release-notes-and-migration-guide"></a>Azure Key Vault .NET 2.0 – Versionshinweise und Migrationshandbuch
Die folgenden Hinweise und Anweisungen richten sich an Entwickler, die die .NET-/C#-Bibliothek für Azure Key Vault verwenden. Beim Übergang von Version 1.0 zu Version 2.0 wurde eine Reihe von Updates vorgenommen, die Migrationsschritte in Ihrem Code erforderlich machen, damit Sie von den verbesserten und zusätzlichen Funktionen (etwa der Unterstützung von **Key Vault-Zertifikaten**) profitieren.

## <a name="key-vault-certificates"></a>Key Vault-Zertifikate

Die Unterstützung von Key Vault-Zertifikaten ermöglicht die Verwaltung Ihrer x509-Zertifikate sowie folgende Szenarien:  

* Ermöglicht einem Zertifikatbesitzer die Erstellung eines Zertifikats über einen Key Vault-Erstellungsvorgang oder durch den Import eines vorhandenen Zertifikats. Dies schließt sowohl selbstsignierte als auch von einer Zertifizierungsstelle generierte Zertifikate ein.
* Ermöglicht dem Besitzer eines Key Vault-Zertifikats die Implementierung von sicherem Speicher sowie die Verwaltung von X509-Zertifikaten ohne Interaktion mit privaten Schlüsseln.  
* Ermöglicht einem Zertifikatbesitzer die Erstellung einer Richtlinie, die Key Vault anweist, den Lebenszyklus eines Zertifikats zu verwalten.  
* Ermöglicht Zertifikatbesitzern die Angabe von Kontaktinformationen zur Benachrichtigung über Lebenszyklusereignisse wie Ablauf und Verlängerung eines Zertifikats.  
* Unterstützt die automatische Verlängerung mit dem ausgewählten Aussteller – Key Vault-Partneranbieter oder -Partnerzertifizierungsstellen für X509-Zertifikate.
  
  * HINWEIS: Anbieter/Zertifizierungsstellen, mit denen keine Partnerschaft besteht, sind ebenfalls zulässig. Das Feature für die automatische Verlängerung wird von ihnen jedoch nicht unterstützt.

## <a name="net-support"></a>Unterstützung von .NET

* **.NET 4.0** wird von Version 2.0 der .NET-/C#-Bibliothek für Azure Key Vault nicht unterstützt.
* **.NET Core** wird von Version 2.0 der .NET-/C#-Bibliothek für Azure Key Vault unterstützt.

## <a name="namespaces"></a>Namespaces

* Der Namespace für **Modelle** wurde von **Microsoft.Azure.KeyVault** in **Microsoft.Azure.KeyVault.Models** geändert.
* Der Namespace **Microsoft.Azure.KeyVault.Internal** wurde gelöscht.
* Die Namespaces der Azure SDK-Abhängigkeiten wurden von **Hyak.Common** und **Hyak.Common.Internals** in **Microsoft.Rest** und **Microsoft.Rest.Serialization** geändert.

## <a name="type-changes"></a>Typänderungen

* *Secret* wurde in *SecretBundle* geändert.
* *Dictionary* wurde in *IDictionary* geändert.
* *List<T>, string []* wurde in *IList<T>* geändert.
* *NextList* wurde in *NextPageLink* geändert.

## <a name="return-types"></a>Rückgabetypen

* **KeyList** und **SecretList** geben *IPage<T>* anstelle von *ListKeysResponseMessage* zurück.
* Das generierte **BackupKeyAsync**-Element gibt *BackupKeyResult* mit *Value* (Backupblob) zurück. Zuvor wurde die Methode eingeschlossen, und es wurde nur der Wert zurückgegeben.

## <a name="exceptions"></a>Ausnahmen

* *KeyVaultClientException* wird in *KeyVaultErrorException* geändert.
* Der Dienstfehler wird von *exception.Error* in *exception.Body.Error.Message* geändert.
* Aus der Fehlermeldung für **[JsonExtensionData]** wurden zusätzliche Informationen entfernt.

## <a name="constructors"></a>Konstruktoren

* Der Konstruktor akzeptiert statt eines *HttpClient*-Elements als Konstruktorargument nur *HttpClientHandler* oder *DelegatingHandler[]*.

## <a name="downloaded-packages"></a>Heruntergeladene Pakete

Bei der Verarbeitung einer Abhängigkeit für Key Vault durch einen Client wurden die folgenden Pakete heruntergeladen:

### <a name="previous-package-list"></a>Vorherige Paketliste

* package id="Hyak.Common" version="1.0.2" targetFramework="net45"
* package id="Microsoft.Azure.Common" version="2.0.4" targetFramework="net45"
* package id="Microsoft.Azure.Common.Dependencies" version="1.0.0" targetFramework="net45"
* package id="Microsoft.Azure.KeyVault" version="1.0.0" targetFramework="net45"
* package id="Microsoft.Bcl" version="1.1.9" targetFramework="net45"
* package id="Microsoft.Bcl.Async" version="1.0.168" targetFramework="net45"
* package id="Microsoft.Bcl.Build" version="1.0.14" targetFramework="net45"
* package id="Microsoft.Net.Http" version="2.2.22" targetFramework="net45"

### <a name="current-package-list"></a>Aktuelle Paketliste

* package id="Microsoft.Azure.KeyVault" version="2.0.0-preview" targetFramework="net45"
* package id="Microsoft.Rest.ClientRuntime" version="2.2.0" targetFramework="net45"
* package id="Microsoft.Rest.ClientRuntime.Azure" version="3.2.0" targetFramework="net45"

## <a name="class-changes"></a>Klassenänderungen

* Die **UnixEpoch**-Klasse wurde entfernt.
* Die **Base64UrlConverter**-Klasse wurde in **Base64UrlJsonConverter** umbenannt.

## <a name="other-changes"></a>Weitere Änderungen

* Dieser API-Version wurde Unterstützung für die Konfiguration der KV-Vorgangswiederholungsrichtlinie für vorübergehende Fehler hinzugefügt.

## <a name="microsoftazuremanagementkeyvault-nuget"></a>Microsoft.Azure.Management.KeyVault NuGet

* Bei den Vorgängen, die ein *vault*-Element zurückgegeben haben, handelt es sich beim Rückgabetyp um eine Klasse, die eine Vault-Eigenschaft enthielt. Der Rückgabetyp lautet jetzt *Vault*.
* *PermissionsToKeys* und *PermissionsToSecrets* heißen jetzt *Permissions.Keys* und *Permissions.Secrets*.
* Einige der Rückgabetypänderungen gelten auch für die Steuerungsebene.

## <a name="microsoftazurekeyvaultextensions-nuget"></a>Microsoft.Azure.KeyVault.Extensions NuGet

* Für Kryptografievorgänge wurde das Paket in **Microsoft.Azure.KeyVault.Extensions** und **Microsoft.Azure.KeyVault.Cryptography** unterteilt.


