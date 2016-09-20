<properties
	pageTitle="Azure AD-Domänendienste: Aktivieren der Kennwortsynchronisierung | Microsoft Azure"
	description="Erste Schritte mit Azure Active Directory-Domänendiensten"
	services="active-directory-ds"
	documentationCenter=""
	authors="mahesh-unnikrishnan"
	manager="stevenpo"
	editor="curtand"/>

<tags
	ms.service="active-directory-ds"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/07/2016"
	ms.author="maheshu"/>

# Azure AD-Domänendienste *(Vorschau)* – Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste

## Aufgabe 5: Aktivieren der Kennwortsynchronisierung für AAD-Domänendienste bei einem synchronisierten Azure AD-Mandanten
Nachdem Sie die Azure AD-Domänendienste für Ihr Azure AD-Verzeichnis aktiviert haben, besteht die nächste Aufgabe darin, die Kennwortsynchronisierung für Azure AD-Domänendienste zu aktivieren. Danach können sich Benutzer mit ihren Unternehmensanmeldeinformationen bei der Domäne anmelden.

Die einzelnen Schritte unterscheiden sich je nachdem, ob es sich bei Ihrer Organisation um ein reines Cloud-Azure AD-Verzeichnis handelt oder ob die Synchronisierung mit Ihrem lokalen Verzeichnis über Azure AD Connect festgelegt ist.

<br>

> [AZURE.SELECTOR]
- [Reines Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)
- [Synchronisiertes Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Synchronisierte Mandanten: Aktivieren der Synchronisierung von Hashes für NTLM- und Kerberos-Anmeldeinformationen mit Azure AD
Ein synchronisierter Azure AD-Mandant wird für die Synchronisierung mit dem lokalen Verzeichnis Ihres Unternehmens per Azure AD Connect festgelegt. Standardmäßig synchronisiert Azure AD Connect keine Hashes von NTLM- und Kerberos-Anmeldeinformationen für Azure AD. Zum Verwenden der Azure AD-Domänendienste müssen Sie Azure AD Connect so konfigurieren, dass Hashes von Anmeldeinformationen, die für die NTLM- und Kerberos-Authentifizierung benötigt werden, synchronisiert werden. Die folgenden Schritte ermöglichen die Synchronisierung der erforderlichen Hashes von Anmeldeinformationen für Ihren Azure AD-Mandanten.

#### Installieren oder Aktualisieren von Azure AD Connect

Sie müssen die neueste empfohlene Version von Azure AD Connect auf einem in die Domäne eingebundenen Computer installieren. Wenn Sie über eine vorhandene Instanz von Azure AD Connect-Setup verfügen, müssen Sie sie aktualisieren, damit die aktuelle Version von Azure AD Connect verwendet wird. Stellen Sie sicher, dass Sie stets die neueste Version von Azure AD Connect verwenden, um bekannte Probleme oder Fehler zu vermeiden, die inzwischen unter Umständen behoben wurden.

**[Azure AD Connect herunterladen](http://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.281.0** – veröffentlicht am 7. September 2016.

  > [AZURE.WARNING] Sie müssen die neueste empfohlene Version von Azure AD Connect installieren, um ältere Anmeldeinformationen zu aktivieren (erforderlich für NTLM- und Kerberos-Authentifizierung) und Ihren Azure AD-Mandanten zu synchronisieren. Diese Funktionalität ist in früheren Versionen von Azure AD Connect oder im DirSync-Legacytool nicht verfügbar.

Installationshinweise für Azure AD Connect finden Sie im folgenden Artikel: [Erste Schritte mit Azure AD Connect](../active-directory/active-directory-aadconnect.md)


#### Erzwingen der vollständigen Kennwortsynchronisierung mit Azure AD

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

Je nach Größe Ihres Verzeichnisses (Anzahl von Benutzern, Gruppen usw.) nimmt die Synchronisierung der Hashes von Anmeldeinformationen mit Azure AD mehr oder weniger Zeit in Anspruch. Die Kennwörter können kurz nach der Synchronisierung der Hashes für die Anmeldeinformationen mit Azure AD in der verwalteten Domäne der Azure AD-Domänendienste verwendet werden.


<br>

## Verwandte Inhalte

- [Aktivieren der Synchronisierung von Kennwörtern für AAD-Domänendienste bei einem reinen Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)

- [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)

- [Einbinden eines virtuellen Windows-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)

- [Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

<!---HONumber=AcomDC_0914_2016-->