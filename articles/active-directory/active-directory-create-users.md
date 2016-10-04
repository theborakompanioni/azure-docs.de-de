<properties
	pageTitle="Hinzufügen neuer Benutzer in Azure Active Directory | Microsoft Azure"
	description="Erklärt, wie neue Benutzer hinzugefügt oder Benutzerinformationen in Azure Active Directory geändert werden."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="09/22/2016"
	ms.author="curtand"/>

# Hinzufügen von neuen Benutzer oder Benutzern mit Microsoft-Konten zu Azure Active Directory

Fügen Sie Benutzer hinzu, um Ihr Verzeichnis aufzufüllen. In diesem Artikel wird beschrieben, wie Sie neue Benutzer in Ihrer Organisation hinzufügen und wie Benutzer hinzugefügt werden, die über Microsoft-Konten verfügen. Weitere Informationen zum Hinzufügen von Benutzern aus anderen Verzeichnissen in Azure Active Directory oder zum Hinzufügen von Benutzern von Partnerunternehmen finden Sie unter [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md). Hinzugefügte Benutzer verfügen nicht standardmäßig über Administratorberechtigungen, aber Sie können ihnen jederzeit Rollen zuweisen.

## Hinzufügen eines Benutzers

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) mit einem Konto an, bei dem es sich um einen globalen Administrator für das Verzeichnis handelt.
2. Wählen Sie **Active Directory** und dann den Namen des Verzeichnisses Ihrer Organisation.
3. Wählen Sie die Registerkarte **Benutzer** und anschließend auf der Befehlsleiste die Option **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Informationen über diesen Benutzer** unter **Art des Benutzers** eine der folgenden Optionen aus:

	- **Neuer Benutzer in Ihrem Unternehmen**: Dient zum Hinzufügen eines neuen Benutzerkontos zu Ihrem Verzeichnis.
	- **Benutzer mit einem vorhandenen Microsoft-Konto**: Dient zum Hinzufügen eines vorhandenen Microsoft-Kundenkontos (z.B. eines Outlook-Kontos) zu Ihrem Verzeichnis.

5. Geben Sie je nach **Benutzertyp** einen Benutzernamen (für neue Benutzer) oder eine E-Mail-Adresse (für einen Benutzer mit einem Microsoft-Konto) ein.
6. Geben Sie auf der Seite **Profil** des Benutzers den Vornamen und Nachnamen, einen benutzerfreundlichen Namen und über die Liste **Rollen** eine Benutzerrolle an. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md). Geben Sie an, ob Sie für den Benutzer die **Multi-Factor Authentication aktivieren** möchten.
7. Wählen Sie auf der Seite **Temporäres Kennwort abrufen** die Option **Erstellen**.

> [AZURE.IMPORTANT] Achten Sie auf die folgenden Probleme, die beim Hinzufügen eines Benutzerkontos auftreten können, wenn Ihre Organisation mehr als eine Domäne verwendet:
>
> - Um Benutzerkonten mit dem gleichen Benutzerprinzipalnamen (User Principal Name, UPN) domänenübergreifend hinzuzufügen, können Sie beispielsweise **zuerst** geoffgrisso@contoso.onmicrosoft.com und **anschließend** geoffgrisso@contoso.com hinzufügen.
> - Fügen Sie geoffgrisso@contoso.com **nicht** vor geoffgrisso@contoso.onmicrosoft.com hinzu. Diese Reihenfolge ist wichtig, und es kann umständlich sein, diesen Vorgang rückgängig zu machen.

## Ändern von Benutzerinformationen

Sie können alle Benutzerattribute ändern, mit Ausnahme der Objekt-ID.

1. Öffnen Sie Ihr Verzeichnis.
2. Wählen Sie die Registerkarte **Benutzer**, und wählen Sie dann den Anzeigenamen des Benutzers aus, den Sie ändern möchten.
3. Geben Sie die Änderungen ein, und klicken Sie auf **Speichern**.

Wenn der Benutzer, den Sie ändern, mit Ihrem lokalen Active Directory-Dienst synchronisiert wird, können Sie die Benutzerinformationen mit diesem Verfahren nicht ändern. Verwenden Sie zum Ändern des Benutzers Ihre lokalen Active Directory-Verwaltungstools.

## Verwaltung von Gastbenutzern und Einschränkungen

Gastkonten sind Benutzer aus anderen Verzeichnissen, die in Ihr Verzeichnis eingeladen wurden, um Zugriff auf SharePoint-Dokumente, Anwendungen oder andere Azure-Ressourcen zu erhalten. Für ein Gastkonto in Ihrem Verzeichnis ist das zugrunde liegende UserType-Attribut auf „Gast“ festgelegt. Reguläre Benutzer (Mitglieder Ihres Verzeichnisses) verfügen über das UserType-Attribut „Member“.

Gäste verfügen im Verzeichnis über eingeschränkte Berechtigungen. Mit diesen Berechtigungen wird für Gäste die Möglichkeit eingeschränkt, Informationen zu anderen Benutzern im Verzeichnis zu ermitteln. Gastbenutzer können aber trotzdem mit den Benutzern und Gruppen interagieren, die den verwendeten Ressourcen zugeordnet sind. Gastbenutzer haben folgende Möglichkeiten:

- Anzeigen anderer Benutzer und Gruppen eines Azure-Abonnements, dem sie zugewiesen sind
- Anzeigen der Mitglieder von Gruppen, denen sie angehören
- Suchen nach anderen Benutzern im Verzeichnis, sofern die vollständige E-Mail-Adresse des Benutzers bekannt ist
- Anzeigen einer begrenzten Gruppe von Attributen der gesuchten Benutzer (Anzeigename, E-Mail-Adresse, Benutzerprinzipalname (UPN) und Miniaturbild)
- Abrufen einer Liste der überprüften Domänen im Verzeichnis
- Erteilen der Zustimmung für Anwendungen und Gewähren des gleichen Zugriffs, der für Mitglieder in Ihrem Verzeichnis gilt

## Festlegen von Richtlinien für den Gastbenutzerzugriff

Die Registerkarte **Konfigurieren** eines Verzeichnisses enthält Optionen zur Zugriffssteuerung für Gastbenutzer. Diese Optionen können nur im klassischen Azure-Portal von einem globalen Verzeichnisadministrator geändert werden. Es gibt derzeit keine PowerShell- oder API-Methode.

Wählen Sie **Active Directory** und den Namen des Verzeichnisses, um die Registerkarte **Konfigurieren** im klassischen Azure-Portal zu öffnen.

![Registerkarte „Konfigurieren“ in Azure Active Directory][1]

Anschließend können Sie die Optionen zur Zugriffssteuerung für Gastbenutzer ändern.

![Zugriffssteuerungsoptionen für Gastbenutzer][2]


## Nächste Schritte

- [Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory](active-directory-create-users-external.md)
- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure AD](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png

<!---HONumber=AcomDC_0928_2016-->