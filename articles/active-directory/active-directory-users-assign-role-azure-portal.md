<properties
	pageTitle="Zuweisen eines Benutzers zu Administratorrollen in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Erläutert, wie in Azure Active Directory Administratorinformationen für Benutzer geändert werden."
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

# Zuweisen eines Benutzers zu Administratorrollen in der Azure Active Directory-Vorschau

In diesem Artikel wird erläutert, wie Sie einem Benutzer in der Azure Active Directory-Vorschau einer Administratorrolle zuweisen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Informationen dazu, wie Sie neue Benutzer in Ihrer Organisation hinzufügen, finden Sie unter [Hinzufügen neuer Benutzer zu Azure Active Directory](active-directory-users-create-azure-portal.md). Hinzugefügte Benutzer verfügen nicht standardmäßig über Administratorberechtigungen, aber Sie können ihnen jederzeit Rollen zuweisen.

## Zuweisen einer Rolle zu einem Benutzer

1.  Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2.  Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

    ![Öffnen der Benutzerverwaltung](./media/active-directory-users-assign-role-azure-portal/create-users-user-management.png)

3.  Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

    ![Öffnen des Blatts „Alle Benutzer“](./media/active-directory-users-assign-role-azure-portal/create-users-opde-DEers-blade.png)

4. Wählen Sie auf dem Blatt **Benutzer und Gruppen – Alle Benutzer** einen Benutzer aus der Liste aus.

5. Wählen Sie auf dem Blatt für den ausgewählten Benutzer die Option **Verzeichnisrolle** aus, und weisen Sie den Benutzer dann über die Liste **Verzeichnisrolle** einer Rolle zu. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md).

	  ![Zuweisen einer Rolle zu einem Benutzer](./media/active-directory-users-assign-role-azure-portal/create-users-assign-role.png)

6. Wählen Sie **Speichern** aus.


## Nächste Schritte

- [Hinzufügen eines Benutzers](active-directory-users-create-azure-portal.md)
- [Zurücksetzen des Kennworts für einen Benutzer im neuen Azure-Portal](active-directory-users-reset-password-azure-portal.md)
- [Ändern der Arbeitsinformationen für einen Benutzer](active-directory-users-work-info-azure-portal.md)
- [Verwalten von Benutzerprofilen](active-directory-users-profile-azure-portal.md)
- [Löschen eines Benutzers in Azure AD](active-directory-users-delete-user-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->