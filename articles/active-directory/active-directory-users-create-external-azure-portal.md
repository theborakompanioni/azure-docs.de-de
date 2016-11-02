<properties
	pageTitle="Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Erklärt, wie neue Benutzer hinzugefügt oder Benutzerinformationen in Azure Active Directory geändert werden, z.B. externe Benutzer und Gastbenutzer."
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
	ms.topic="article"
	ms.date="09/12/2016"
	ms.author="curtand"/>

# Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in der Azure Active Directory-Vorschau

> [AZURE.SELECTOR]
- [Azure-Portal](active-directory-users-create-external-azure-portal.md)
- [Klassisches Azure-Portal](active-directory-create-users-external.md)

In diesem Artikel wird beschrieben, wie Sie in der Azure Active Directory-Vorschau (Azure AD) Benutzer aus anderen Verzeichnissen sowie Benutzer von Partnerunternehmen hinzufügen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Informationen dazu, wie Sie neue Benutzer in Ihrer Organisation sowie Benutzer mit Microsoft-Konten hinzufügen, finden Sie unter [Hinzufügen neuer Benutzer zu Azure Active Directory](active-directory-users-create-azure-portal.md). Hinzugefügte Benutzer verfügen nicht standardmäßig über Administratorberechtigungen, aber Sie können ihnen jederzeit Rollen zuweisen.

## Hinzufügen eines Benutzers

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2.  Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

    ![Öffnen der Benutzerverwaltung](./media/active-directory-users-create-external-azure-portal/create-users-user-management.png)

3.  Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Benutzer** aus, und klicken Sie dann auf **Hinzufügen**.

    ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-users-create-external-azure-portal/create-users-add-command.png)

4. Geben Sie auf dem Blatt **Benutzer** im Feld **Name** einen Anzeigenamen und im Feld **Benutzername** den Anmeldenamen des Benutzers ein.

5. Kopieren oder notieren Sie sich das generierte Benutzerkennwort, damit sie es nach Abschluss des Vorgangs dem Benutzer mitteilen können.

6. Wählen Sie optional **Profil** aus, um den Vor- und Nachnamen des Benutzers, eine Position und einen Abteilungsnamen hinzuzufügen.

	![Opening the user profile](./media/active-directory-users-create-external-azure-portal/create-users-user-profile.png)

	- Wählen Sie **Gruppen** aus, um den Benutzer mindestens einer Gruppe hinzuzufügen.

		![Hinzufügen eines Benutzers zu Gruppen](./media/active-directory-users-create-external-azure-portal/create-users-user-groups.png)

	- Wählen Sie **Organisationsrolle** aus, um den Benutzer über die Liste **Rollen** einer Rolle zuzuweisen. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).

		![Zuweisen eines Benutzers zu einer Rolle](./media/active-directory-users-create-external-azure-portal/create-users-assign-role.png)

7. Klicken Sie auf **Erstellen**.

8. Sorgen Sie für eine sichere Übermittlung des generierten Kennworts an den neuen Benutzer, damit sich dieser anmelden kann.

> [AZURE.IMPORTANT] Achten Sie auf die folgenden Probleme, die beim Hinzufügen eines Benutzerkontos auftreten können, wenn Ihre Organisation mehr als eine Domäne verwendet:
>
> - Um Benutzerkonten mit dem gleichen Benutzerprinzipalnamen (User Principal Name, UPN) domänenübergreifend hinzuzufügen, können Sie beispielsweise **zuerst** geoffgrisso@contoso.onmicrosoft.com und **anschließend** geoffgrisso@contoso.com hinzufügen.
> - Fügen Sie geoffgrisso@contoso.com **nicht** vor geoffgrisso@contoso.onmicrosoft.com hinzu. Diese Reihenfolge ist wichtig, und es kann umständlich sein, diesen Vorgang rückgängig zu machen.

Wenn Sie Informationen für einen Benutzer ändern, dessen Identität mit Ihrem lokalen Active Directory-Dienst synchronisiert ist, können Sie die Benutzerinformationen im klassischen Azure-Portal nicht ändern. Verwenden Sie zum Ändern der Benutzerinformationen Ihre lokalen Active Directory-Verwaltungstools.


## Nächste Schritte

- [Hinzufügen eines Benutzers](active-directory-users-create-azure-portal.md)
- [Zurücksetzen des Kennworts für einen Benutzer im neuen Azure-Portal](active-directory-users-reset-password-azure-portal.md)
- [Zuweisen eines Benutzers zu einer Rolle in Azure AD](active-directory-users-assign-role-azure-portal.md)
- [Ändern der Arbeitsinformationen für einen Benutzer](active-directory-users-work-info-azure-portal.md)
- [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
- [Löschen eines Benutzers in Azure AD](active-directory-users-delete-user-azure-portal.md)

<!----HONumber=AcomDC_0914_2016-->