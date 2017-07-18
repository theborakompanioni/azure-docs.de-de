---
title: 'Azure AD Domain Services: Aktivieren der Kennwortsynchronisierung | Microsoft Docs'
description: Erste Schritte mit Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: 947ea3c9d789ecf5a754001aafcda6f8bcd41047
ms.contentlocale: de-de
ms.lasthandoff: 07/08/2017


---
# <a name="enable-password-synchronization-to-azure-active-directory-domain-services"></a>Aktivieren der Kennwortsynchronisierung für Azure Active Directory Domain Services
In vorherigen Aufgaben haben Sie Azure Active Directory Domain Services für Ihren Azure AD-Mandanten (Azure Active Directory) aktiviert. Die nächste Aufgabe besteht darin, die Synchronisierung der Anmeldeinformationshashes, die für die NTLM- (NT LAN Manager) und Kerberos-Authentifizierung erforderlich sind, mit Azure AD Domain Services zu ermöglichen. Nach der Einrichtung der Synchronisierung von Anmeldeinformationen können sich Benutzer mit ihren Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden.

Für reine Cloudbenutzerkonten müssen andere Schritte ausgeführt werden als für Benutzerkonten, die aus Ihrem lokalen Verzeichnis mit Azure AD Connect synchronisiert werden. Wenn Ihr Azure AD-Mandant über eine Kombination aus reinen Cloudbenutzern und Benutzern aus Ihrem lokalen AD verfügt, müssen beide Schritte ausgeführt werden.

<br>

> [!div class="op_single_selector"]
> * **Reine Cloudbenutzerkonten:** [Synchronisieren der Kennwörter reiner Cloudbenutzerkonten mit Ihrer verwalteten Domäne](active-directory-ds-getting-started-password-sync.md)
> * **Lokale Benutzerkonten:** [Synchronisieren der Kennwörter für synchronisierte Benutzerkonten aus Ihrem lokalen AD mit Ihrer verwalteten Domäne](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-to-your-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>Aufgabe 5: Aktivieren der Kennwortsynchronisierung mit Ihrer verwalteten Domäne für Benutzerkonten, die mit Ihrem lokalen AD synchronisiert werden
Ein synchronisierter Azure AD-Mandant wird für die Synchronisierung mit dem lokalen Verzeichnis Ihres Unternehmens per Azure AD Connect festgelegt. Standardmäßig synchronisiert Azure AD Connect keine NTLM- und Kerberos-Anmeldeinformationshashes mit Azure AD. Zum Verwenden der Azure AD-Domänendienste müssen Sie Azure AD Connect so konfigurieren, dass Hashes von Anmeldeinformationen, die für die NTLM- und Kerberos-Authentifizierung benötigt werden, synchronisiert werden. Die folgenden Schritte ermöglichen die Synchronisierung der erforderlichen Anmeldeinformationshashes aus Ihrem lokalen Verzeichnis mit Ihrem Azure AD-Mandanten.

> [!NOTE]
> Falls Ihre Organisation über Benutzerkonten verfügt, die aus Ihrem lokalen Verzeichnis synchronisiert werden, müssen Sie die Synchronisierung von NTLM- und Kerberos-Hashes aktivieren, um die verwaltete Domäne verwenden zu können. Ein synchronisiertes Benutzerkonto ist ein Konto, das in Ihrem lokalen Verzeichnis erstellt wurde und unter Verwendung von Azure AD Connect mit Ihrem Azure AD-Mandanten synchronisiert wird.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Installieren oder Aktualisieren von Azure AD Connect
Installieren Sie die neueste empfohlene Version von Azure AD Connect auf einem in die Domäne eingebundenen Computer. Wenn Sie über eine vorhandene Instanz von Azure AD Connect-Setup verfügen, müssen Sie sie aktualisieren, damit die aktuelle Version von Azure AD Connect verwendet wird. Stellen Sie sicher, dass Sie stets die neueste Version von Azure AD Connect verwenden, um bekannte Probleme oder Fehler zu vermeiden, die inzwischen unter Umständen behoben wurden.

**[Azure AD Connect herunterladen](http://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.553.0** – veröffentlicht am 27. Juni 2017.

> [!WARNING]
> Sie müssen die neueste empfohlene Version von Azure AD Connect installieren, um ältere Anmeldeinformationen zu aktivieren (erforderlich für NTLM- und Kerberos-Authentifizierung) und Ihren Azure AD-Mandanten zu synchronisieren. Diese Funktionalität ist in früheren Versionen von Azure AD Connect oder im DirSync-Legacytool nicht verfügbar.
>
>

Installationshinweise für Azure AD Connect finden Sie im folgenden Artikel: [Erste Schritte mit Azure AD Connect](../active-directory/active-directory-aadconnect.md)

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>Aktivieren der Synchronisierung von Hashes für NTLM- und Kerberos-Anmeldeinformationen mit Azure AD
Führen Sie das folgende PowerShell-Skript für jede AD-Gesamtstruktur aus, um eine vollständige Kennwortsynchronisierung zu erzwingen. Ermöglichen Sie außerdem für die Hashes von Anmeldeinformationen aller lokalen Benutzer die Synchronisierung mit Ihrem Azure AD-Mandanten. Mit diesem Skript wird es ermöglicht, dass die für die NTLM/Kerberos-Authentifizierung erforderlichen Hashes der Anmeldeinformationen mit Ihrem Azure AD-Mandanten synchronisiert werden.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Je nach Größe Ihres Verzeichnisses (Anzahl von Benutzern, Gruppen usw.) nimmt die Synchronisierung der Hashes von Anmeldeinformationen mit Azure AD mehr oder weniger Zeit in Anspruch. Die Kennwörter können kurz nach der Synchronisierung der Hashes für die Anmeldeinformationen mit Azure AD in der verwalteten Domäne der Azure AD-Domänendienste verwendet werden.

<br>

## <a name="related-content"></a>Verwandte Inhalte
* [Aktivieren der Synchronisierung von Kennwörtern für AAD-Domänendienste bei einem reinen Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)
* [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)
* [Einbinden eines virtuellen Windows-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)
* [Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

