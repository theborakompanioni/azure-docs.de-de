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

## Aufgabe 5: Aktivieren der Kennwortsynchronisierung für AAD-Domänendienste bei einem reinen Cloud-Azure AD-Verzeichnis
Nachdem Sie Azure AD-Domänendienste für Ihren Azure AD-Mandanten aktiviert haben, besteht die nächste Aufgabe darin, die Synchronisierung von Anmeldeinformationen für Azure AD-Domändendienste zu aktivieren. Dadurch können Benutzer sich mithilfe ihrer Unternehmensanmeldeinformationen bei der verwalteten Domäne anmelden.

Die einzelnen Schritte unterscheiden sich je nachdem, ob es sich bei Ihrer Organisation um ein reines Cloud-Azure AD-Verzeichnis handelt oder ob die Synchronisierung mit Ihrem lokalen Verzeichnis über Azure AD Connect festgelegt ist.

<br>

> [AZURE.SELECTOR]
- [Reines Cloud-Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync.md)
- [Synchronisiertes Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>

### Aktivieren der Hashgenerierung für NTLM- und Kerberos-Anmeldeinformationen für ein reines Cloud-Azure AD-Verzeichnis
Wenn Ihre Organisation ein reines Cloud-Azure AD-Verzeichnis besitzt, müssen Benutzer, die Azure AD-Domänendienste verwenden, ihre Kennwörter ändern. Diese Kennwortänderung führt dazu, dass die Anmeldeinformationshashes, die von den Azure AD-Domänendiensten für die Kerberos- und NTLM-Authentifizierung benötigt werden, in Azure AD generiert werden. Sie können entweder die Kennwörter für alle Benutzer im Mandanten ablaufen lassen, die Azure AD-Domänendienste verwenden müssen, oder diese Benutzer zum Ändern ihrer Kennwörter anweisen.

Hier sind Anweisungen, die Sie den Endbenutzern zur Änderung ihrer Kennwörter bereitstellen müssen:

1. Navigieren Sie zur Seite "Azure AD-Zugriffsbereich" für Ihre Organisation. Diese steht in der Regel unter [http://myapps.microsoft.com](http://myapps.microsoft.com) zur Verfügung.

2. Wählen Sie auf dieser Seite die Registerkarte **Profil**.

3. Klicken Sie auf dieser Seite auf die Kachel **Kennwort ändern**, um eine Kennwortänderung einzuleiten.

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password.png)

4. Die Seite **Kennwort ändern** wird geöffnet. Benutzer können das vorhandene (alte) Kennwort eingeben und es dann ändern.

    ![Erstellen Sie ein virtuelles Netzwerk für Azure Active Directory-Domänendienste.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Nachdem die Benutzer ihr Kennwort geändert haben, kann das neue Kennwort bald in den Azure AD-Domänendiensten verwendet werden. Nach einigen Minuten können sich Benutzer mit ihren neuen geänderten Kennwörtern bei Computern in der verwalteten Domäne anmelden.


<br>

## Verwandte Inhalte

- [Aktivieren der Synchronisierung von Kennwörtern für AAD-Domänendienste bei einem synchronisierten Azure AD-Verzeichnis](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)

- [Einbinden eines virtuellen Windows-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-windows-vm.md)

- [Einbinden eines virtuellen Red Hat Enterprise Linux-Computers in eine verwaltete Domäne der Azure AD-Domänendienste](active-directory-ds-admin-guide-join-rhel-linux-vm.md)

<!---HONumber=AcomDC_0427_2016-->