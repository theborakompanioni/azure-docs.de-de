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
	ms.date="04/25/2016"
	ms.author="maheshu"/>

# Azure AD-Domänendienste *(Vorschau)* – Aktivieren der Kennwortsynchronisierung für Azure AD-Domänendienste

## Aufgabe 5: Aktivieren der Kennwortsynchronisierung für AAD-Domänendienste bei einem synchronisierten Azure AD-Mandanten
Nachdem Sie die Azure AD-Domänendienste für Ihr Azure AD-Verzeichnis aktiviert haben, besteht die nächste Aufgabe darin, die Kennwortsynchronisierung für Azure AD-Domänendienste zu aktivieren. Dadurch können Benutzer sich mithilfe ihrer Unternehmensanmeldeinformationen bei der Domäne anmelden.

Die einzelnen Schritte unterscheiden sich je nachdem, ob es sich bei Ihrer Organisation um ein reines Cloud-Azure AD-Verzeichnis handelt oder ob die Synchronisierung mit Ihrem lokalen Verzeichnis über Azure AD Connect festgelegt ist.

<br>

> [AZURE.SELECTOR]
- [Reines Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)
- [Synchronisiertes Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Synchronisierte Mandanten: Aktivieren der Synchronisierung von Hashes für NTLM- und Kerberos-Anmeldeinformationen mit Azure AD
Wenn der Azure AD-Mandant für Ihre Organisation so festgelegt ist, dass er über Azure AD Connect mit Ihrem lokalen Verzeichnis synchronisiert wird, müssen Sie Azure AD Connect so konfigurieren, dass die für die NTLM- und Kerberos-Authentifizierung erforderlichen Anmeldeinformationshashes synchronisiert werden. Diese Hashes werden standardmäßig nicht mit Azure AD synchronisiert. Anhand der folgenden Schritte können Sie die Synchronisierung der Hashes mit Ihrem Azure AD-Mandanten aktivieren.

#### Installieren oder Aktualisieren von Azure AD Connect

Sie müssen die neueste empfohlene Version von Azure AD Connect auf einem in die Domäne eingebundenen Computer installieren. Wenn Sie über eine vorhandene Instanz von Azure AD Connect-Setup verfügen, müssen Sie sie aktualisieren, damit der Azure AD Connect-GA-Build verwendet wird. Stellen Sie sicher, dass Sie die neueste Version von Azure AD Connect verwenden, um bekannte Probleme oder Fehler zu vermeiden, die inzwischen möglicherweise behoben wurden.

**[Azure AD Connect herunterladen](http://www.microsoft.com/download/details.aspx?id=47594)**

Empfohlene Version: **1.1.130.0** – veröffentlicht am 12. April 2016.

  > [AZURE.WARNING] Sie müssen die neueste empfohlene Version von Azure AD Connect installieren, um ältere Anmeldeinformationen zu aktivieren (erforderlich für NTLM- und Kerberos-Authentifizierung) und Ihren Azure AD-Mandanten zu synchronisieren. Diese Funktionalität ist in früheren Versionen von Azure AD Connect oder im DirSync-Legacytool nicht verfügbar.

Installationshinweise für Azure AD Connect finden Sie im folgenden Artikel: [Erste Schritte mit Azure AD Connect](../active-directory/active-directory-aadconnect.md)


#### Erzwingen der vollständigen Kennwortsynchronisierung mit Azure AD

Um eine vollständige Kennwortsynchronisierung zu erzwingen und die Synchronisierung der Kennworthashes aller lokalen Benutzer (einschließlich der Anmeldeinformationshashes, die für die NTLM-/Kerberos-Authentifizierung erforderlich sind) mit Ihrem Azure AD-Mandanten zu aktivieren, führen Sie für jede Active Directory-Gesamtstruktur das folgende PowerShell-Skript aus.

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

Abhängig von der Größe Ihres Verzeichnisses (Anzahl der Benutzer, Gruppen usw.) nimmt die Synchronisierung von Anmeldeinformationen mit Azure AD etwas Zeit in Anspruch. Die Kennwörter können kurz nach der Synchronisierung der Hashes für die Anmeldeinformationen mit Azure AD in der verwalteten Domäne der Azure AD-Domänendienste verwendet werden.


<br>

## Verwandte Inhalte

- [Aktivieren der Synchronisierung von Kennwörtern für AAD-Domänendienste bei einem reinen Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)

- [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)

- [Einbinden eines virtuellen Windows-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)

- [Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

<!---HONumber=AcomDC_0427_2016-->