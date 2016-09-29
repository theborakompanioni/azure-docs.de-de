<properties
	pageTitle="Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App in der Azure Active Directory-Vorschau | Microsoft Azure"
	description="Entfernen der Zugriffszuweisung eines Benutzers oder einer Gruppe aus einer Unternehmens-App in Azure Active Directory"
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


# Entfernen einer Benutzer- oder Gruppenzuweisung aus einer Unternehmens-App in der Azure Active Directory-Vorschau

In der Azure Active Directory-Vorschau (Azure AD) können Sie auf einfache Weise eine Benutzer- oder Gruppenzuweisung für den Zugriff auf eine Ihrer Unternehmensanwendungen entfernen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Sie müssen über die geeigneten Berechtigungen zum Verwalten der Unternehmens-App verfügen. In der aktuellen Vorschau müssen Sie als globaler Administrator für das Verzeichnis konfiguriert sein.

## Wie entferne ich eine Benutzer- oder Gruppenzuweisung?

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.

2. Wählen Sie **Weitere Dienste** aus, geben Sie **Azure Active Directory** in das Textfeld ein, und drücken Sie die **EINGABETASTE**.

3. Wählen Sie auf dem Blatt **Azure Active Directory – *Verzeichnisname*** (d.h. auf dem Azure AD-Blatt für das Verzeichnis, das Sie verwalten) **Unternehmensanwendungen** aus.

	![Öffnen von Unternehmens-Apps](./media/active-directory-coreapps-remove-assignment-user-azure-portal/open-enterprise-apps.png)

4. Wählen Sie auf dem Blatt **Unternehmensanwendungen** die Einstellung **Alle Anwendungen** aus. Es wird eine Liste aller Apps angezeigt, die Sie verwalten können.

5. Wählen Sie auf dem Blatt **Unternehmensanwendungen – Alle Anwendungen** eine App aus.

6. Wählen Sie auf dem Blatt ***App-Name*** (dem Blatt mit dem Namen der ausgewählten App im Titel) die Einstellung **Benutzer und Gruppen**.

	![Auswählen von Benutzern oder Gruppen](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-app-users.png)

7. Wählen Sie auf dem Blatt ***App-Name*** **– Benutzer- und Gruppenzuweisung** mindestens einen Benutzer oder eine Gruppe aus, und klicken Sie dann auf den Befehl **Entfernen**. Bestätigen Sie Ihre Entscheidung, wenn Sie dazu aufgefordert werden.

	![Auswählen des Befehls „Entfernen“](./media/active-directory-coreapps-remove-assignment-user-azure-portal/remove-users.png)

## Nächste Schritte

- [Alle meine Gruppen anzeigen](active-directory-groups-view-azure-portal.md)
- [Zuweisen eines Benutzers oder einer Gruppe zu einer Unternehmens-App](active-directory-coreapps-assign-user-azure-portal.md)
- [Deaktivieren von Benutzeranmeldungen für eine Unternehmens-App](active-directory-coreapps-disable-app-azure-portal.md)
- [Ändern des Namens oder Logos einer Unternehmens-App](active-directory-coreapps-change-app-logo-azure-portal.md)

<!---HONumber=AcomDC_0914_2016-->