<properties
	pageTitle="Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen in Azure Active Directory | Microsoft Azure"
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
	ms.topic="get-started-article"
	ms.date="09/22/2016"
	ms.author="curtand"/>

# Hinzufügen von Benutzern aus anderen Verzeichnissen oder Partnerunternehmen zu Azure Active Directory

> [AZURE.SELECTOR]
- [Azure-Portal](active-directory-users-create-external-azure-portal.md)
- [Klassisches Azure-Portal](active-directory-create-users-external.md)

In diesem Artikel wird beschrieben, wie Sie Benutzer von anderen Verzeichnissen in Azure Active Directory sowie Benutzer von Partnerunternehmen hinzufügen. Informationen dazu, wie Sie neue Benutzer in Ihrer Organisation sowie Benutzer mit Microsoft-Konten hinzufügen, finden Sie unter [Hinzufügen neuer Benutzer zu Azure Active Directory](active-directory-create-users.md). Hinzugefügte Benutzer verfügen nicht standardmäßig über Administratorberechtigungen, aber Sie können ihnen jederzeit Rollen zuweisen.

## Hinzufügen eines Benutzers

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com) mit einem Konto an, bei dem es sich um einen globalen Administrator für das Verzeichnis handelt.

2. Wählen Sie **Active Directory**, und öffnen Sie Ihr Verzeichnis.

3. Wählen Sie die Registerkarte **Benutzer** und anschließend auf der Befehlsleiste die Option **Benutzer hinzufügen**.

4. Wählen Sie auf der Seite **Informationen über diesen Benutzer** unter **Art des Benutzers** eine der folgenden Optionen aus:

	- **Benutzer in einem anderen Azure AD-Verzeichnis**: Dient zum Hinzufügen eines Benutzerkontos zu Ihrem Verzeichnis, das aus einem anderen Azure AD-Verzeichnis erstellt wurde. Sie können einen Benutzer nur dann in einem anderen Verzeichnis auswählen, wenn Sie auch ein Mitglied dieses Verzeichnisses sind.
	- **Benutzer in Partnerunternehmen**: Dient zum Einladen und Autorisieren eines Partnerunternehmens für Ihr Verzeichnis (siehe [Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md)). Sie müssen dabei [eine CSV-Datei hochladen und die E-Mail-Adressen angeben](active-directory-b2b-references-csv-file-format.md).

6. Geben Sie auf der Seite **Profil** des Benutzers den Vornamen und Nachnamen, einen benutzerfreundlichen Namen und über die Liste **Rollen** eine Benutzerrolle an. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md). Geben Sie an, ob Sie für den Benutzer die **Multi-Factor Authentication aktivieren** möchten.

7. Wählen Sie auf der Seite **Temporäres Kennwort abrufen** die Option **Erstellen**.

> [AZURE.IMPORTANT] Achten Sie auf die folgenden Probleme, die beim Hinzufügen eines Benutzerkontos auftreten können, wenn Ihre Organisation mehr als eine Domäne verwendet:
>
> - Um Benutzerkonten mit dem gleichen Benutzerprinzipalnamen (User Principal Name, UPN) domänenübergreifend hinzuzufügen, können Sie beispielsweise **zuerst** geoffgrisso@contoso.onmicrosoft.com und **anschließend** geoffgrisso@contoso.com hinzufügen.
> - Fügen Sie geoffgrisso@contoso.com **nicht** vor geoffgrisso@contoso.onmicrosoft.com hinzu. Diese Reihenfolge ist wichtig, und es kann umständlich sein, diesen Vorgang rückgängig zu machen.

Wenn Sie Informationen für einen Benutzer ändern, dessen Identität mit Ihrem lokalen Active Directory-Dienst synchronisiert ist, können Sie die Benutzerinformationen im klassischen Azure-Portal nicht ändern. Verwenden Sie zum Ändern der Benutzerinformationen Ihre lokalen Active Directory-Verwaltungstools.

## Hinzufügen von externen Benutzern

Sie können auch Benutzer aus einem anderen Azure AD-Verzeichnis hinzufügen, dem Sie angehören, oder von Partnerunternehmen, indem Sie eine CSV-Datei hochladen. Zum Hinzufügen eines externen Benutzers geben Sie unter **Art des Benutzers** die Option **Benutzer in einem anderen Microsoft Azure AD-Verzeichnis** oder **Benutzer in Partnerunternehmen** an.

Diese Benutzertypen werden aus einem anderen Verzeichnis erstellt und als **externe Benutzer** hinzugefügt. Externe Benutzer können mit anderen Benutzern in einem Verzeichnis zusammenarbeiten, ohne dass die Anforderung besteht, neue Konten und Anmeldeinformationen hinzuzufügen. Externe Benutzer werden über ihr Basisverzeichnis authentifiziert, wenn sie sich anmelden. Diese Authentifizierung funktioniert auch für alle anderen Verzeichnisse, denen sie hinzugefügt wurden.

## Verwaltung externer Benutzer und Einschränkungen

Wenn Sie einen Benutzer aus einem anderen Verzeichnis Ihrem Verzeichnis hinzufügen, wird er in Ihrem Verzeichnis zu einem externen Benutzer. Der Anzeigename und der Benutzername werden aus dem Basisverzeichnis kopiert und für den externen Benutzer in Ihrem Verzeichnis verwendet. Ab diesem Punkt sind die Eigenschaften des externen Benutzerkontos vollkommen unabhängig. Wenn Eigenschaftsänderungen für den Benutzer im Basisverzeichnis vorgenommen werden, werden diese Änderungen nicht an das externe Benutzerkonto in Ihrem Verzeichnis weitergegeben.

Die einzige Verbindung zwischen den beiden Konten ist, dass der Benutzer immer anhand seines Basisverzeichnisses oder über sein Microsoft-Konto authentifiziert wird. Aus diesem Grund wird keine Option zum Zurücksetzen des Kennworts oder Aktivieren der Multi-Factor Authentication für einen externen Benutzer angezeigt. Die Authentifizierungsrichtlinie des Basisverzeichnisses oder des Microsoft-Kontos ist derzeit die einzige Richtlinie, die beim Anmelden des Benutzers ausgewertet wird.

> [AZURE.NOTE]
Sie können den Benutzer im externen Verzeichnis aber trotzdem deaktivieren, wodurch der Zugriff auf Ihr Verzeichnis blockiert wird.

Wenn ein Benutzer in seinem Basisverzeichnis gelöscht wird oder sein Microsoft-Konto kündigt, ist der externe Benutzer weiterhin in Ihrem Verzeichnis vorhanden. Der Benutzer in Ihrem Verzeichnis kann aber nicht auf die Ressourcen zugreifen, da die Authentifizierung mit einem Basisverzeichnis oder Microsoft-Konto nicht möglich ist.

### Dienste, die den Zugriff durch externe Azure AD-Benutzer derzeit unterstützen:

- **Klassisches Azure-Portal:** Ermöglicht einem externen Benutzer, der Administrator mehrerer Verzeichnisse ist, das Verwalten dieser Verzeichnisse.
- **SharePoint Online:** Ermöglicht einem externen Benutzer den Zugriff auf autorisierte Ressourcen von SharePoint Online, wenn die externe Freigabe aktiviert ist.
- **Dynamics CRM:** Ermöglicht einem externen Benutzer den Zugriff auf autorisierte Ressourcen in Dynamics CRM, wenn der Benutzer per PowerShell lizenziert ist.
- **Dynamics AX:** Ermöglicht einem externen Benutzer den Zugriff auf autorisierte Ressourcen in Dynamics AX, wenn der Benutzer per PowerShell lizenziert ist. Die Einschränkungen für [externe Azure AD-Benutzer](#known-limitations-of-azure-ad-external-users) gelten auch für externe Benutzer in Dynamics AX.

### Bekannte Einschränkungen für externe Azure AD-Benutzer

- Externe Benutzer, bei denen es sich um Administratoren handelt, können Benutzer von Partnerunternehmen nicht Verzeichnissen (B2B-Zusammenarbeit) außerhalb ihres Basisverzeichnisses hinzufügen.
- Externe Benutzer können mehrinstanzenfähigen Anwendungen in Verzeichnissen außerhalb ihres Basisverzeichnisses nicht ihre Zustimmung geben.
- PowerBI unterstützt den Zugriff durch externe Benutzer derzeit nicht.
- Das Office-Portal unterstützt die Lizenzierung externer Benutzer nicht.
- In Bezug auf Azure AD PowerShell werden externe Benutzer bei ihrem Basisverzeichnis angemeldet. Sie können Verzeichnisse, in denen sie externe Benutzer sind, nicht verwalten.


## Nächste Schritte

- [Hinzufügen neuer Benutzer zu Azure Active Directory](active-directory-create-users.md)
- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure AD](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure AD](active-directory-manage-groups.md)

<!---HONumber=AcomDC_0928_2016-->