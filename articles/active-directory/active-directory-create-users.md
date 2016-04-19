<properties
	pageTitle="Hinzufügen von Benutzern oder Ändern von Benutzerinformationen in Azure Active Directory | Microsoft Azure"
	description="Es wird beschrieben, wie Sie in Azure Active Directory Benutzer hinzufügen oder Benutzerinformationen ändern, z.B. externe Benutzer und Gastbenutzer."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="03/31/2016"
	ms.author="curtand;viviali"/>

# Hinzufügen oder Ändern von Benutzern in Azure Active Directory

Fügen Sie für jeden Benutzer, der auf einen Microsoft-Clouddienst zugreift, dem Verzeichnis Ihres Mandanten ein Konto hinzu. Hinzugefügte Benutzer verfügen nicht standardmäßig über Administratorberechtigungen, aber Sie können ihnen jederzeit Rollen zuweisen.

## Hinzufügen eines Benutzers

1. Melden Sie sich am [klassischen Azure-Portal](https://manage.windowsazure.com) mit einem Konto an, bei dem es sich um einen globalen Administrator für das Verzeichnis handelt.
2. Wählen Sie **Active Directory** und dann den Namen des Verzeichnisses Ihrer Organisation.
3. Wählen Sie die Registerkarte **Benutzer** und dann in der Befehlsleiste die Option **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Informationen über diesen Benutzer** unter **Art des Benutzers** eine der folgenden Optionen:

	- **Neuer Benutzer in Ihrem Unternehmen:** Dient zum Hinzufügen eines neuen Benutzerkontos in Ihrem Verzeichnis.
	- **Benutzer mit einem vorhandenen Microsoft-Konto:** Dient zum Hinzufügen eines vorhandenen Microsoft-Kundenkontos (z.B. eines Outlook-Kontos) zu Ihrem Verzeichnis.
	- **Benutzer in einem anderen Azure AD-Verzeichnis:** Dient zum Hinzufügen eines Benutzerkontos zu Ihrem Verzeichnis, das aus einem anderen Azure AD-Verzeichnis stammt. Sie können einen Benutzer nur dann in einem anderen Verzeichnis auswählen, wenn Sie auch ein Mitglied dieses Verzeichnisses sind.
	- **Benutzer in Partnerunternehmen:** Dient zum Einladen und Autorisieren eines Partnerunternehmens für Ihr Verzeichnis (siehe [Azure Active Directory B2B-Zusammenarbeit](active-directory-b2b-what-is-azure-ad-b2b.md)).


5. Geben Sie je nach **Art des Benutzers** einen Benutzernamen und eine E-Mail-Adresse ein, oder laden Sie eine CSV-Datei mit E-Mail-Adressen hoch.
6. Geben Sie auf der Seite **Profil** des Benutzers den Vornamen und Nachnamen, einen benutzerfreundlichen Namen und über die Liste **Rollen** eine Benutzerrolle an. Weitere Informationen zu Benutzer- und Administratorrollen finden Sie unter [Zuweisen von Administratorrollen in Azure AD](active-directory-assign-admin-roles.md). Geben Sie an, ob **Mehrstufige Authentifizierung aktivieren** verwendet werden soll.
7. Wählen Sie auf der Seite **Temporäres Kennwort abrufen** die Option **Erstellen**.

> [AZURE.IMPORTANT] Achten Sie auf die folgenden Probleme, die beim Hinzufügen eines Benutzerkontos auftreten können, wenn Ihre Organisation mehr als eine Domäne verwendet:
>
> - Um Benutzerkonten mit dem gleichen Benutzerprinzipalnamen (User Principal Name, UPN) domänenübergreifend hinzuzufügen, können Sie beispielsweise **zuerst** geoffgrisso@contoso.onmicrosoft.com und **dann** geoffgrisso@contoso.com hinzufügen.
> - Fügen Sie **nicht** geoffgrisso@contoso.com hinzu, bevor Sie geoffgrisso@contoso.onmicrosoft.com hinzufügen. Diese Reihenfolge ist wichtig, und es kann umständlich sein, diesen Vorgang rückgängig zu machen.

## Ändern von Benutzerinformationen

Sie können alle Benutzerattribute ändern, mit Ausnahme der Objekt-ID.

1. Öffnen Sie Ihr Verzeichnis.
2. Wählen Sie die Registerkarte **Benutzer**, und wählen Sie dann den Anzeigenamen des Benutzers aus, den Sie ändern möchten.
3. Geben Sie die Änderungen ein, und klicken Sie auf **Speichern**.

Wenn der Benutzer, den Sie ändern, mit Ihrem lokalen Active Directory-Dienst synchronisiert wird, können Sie die Benutzerinformationen mit diesem Verfahren nicht ändern. Verwenden Sie zum Ändern des Benutzers Ihre lokalen Active Directory-Verwaltungstools.

## Zurücksetzen des Benutzerkennworts

1. Öffnen Sie Ihr Verzeichnis.
2. Wählen Sie die Registerkarte **Benutzer**, und wählen Sie dann den Anzeigenamen des Benutzers aus, den Sie ändern möchten.
3. Wählen Sie in der Befehlsleiste die Option **Kennwort zurücksetzen**.
4. Klicken Sie im Dialogfeld zum Zurücksetzen des Kennworts auf **Zurücksetzen**.
5. Aktivieren Sie das Kontrollkästchen, um das Zurücksetzen des Kennworts abzuschließen.

## Hinzufügen von externen Benutzern

Sie können auch Benutzer aus einem anderen Azure AD-Verzeichnis hinzufügen, dem Sie angehören, oder von Partnerunternehmen, indem Sie eine CSV-Datei hochladen. Zum Hinzufügen eines externen Benutzers geben Sie unter **Art des Benutzers** die Option **Benutzer in einem anderen Microsoft Azure AD-Verzeichnis** oder **Benutzer in Partnerunternehmen** an.

Benutzer dieser Typen stammen aus einem anderen Verzeichnis und werden als **externe Benutzer** hinzugefügt. Externe Benutzer können mit anderen Benutzern in einem Verzeichnis zusammenarbeiten, ohne dass die Anforderung besteht, neue Konten und Anmeldeinformationen hinzuzufügen. Externe Benutzer werden über ihr Basisverzeichnis authentifiziert, wenn sie sich anmelden. Diese Authentifizierung funktioniert auch für alle anderen Verzeichnisse, denen sie hinzugefügt wurden.

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
- **Dynamics AX:** Ermöglicht einem externen Benutzer den Zugriff auf autorisierte Ressourcen in Dynamics AX, wenn der Benutzer per PowerShell lizenziert ist. Die Einschränkungen für [externe Azure AD-Benutzer](#known-limitations-of-azure-ad-external-users) und [Gastbenutzer](#guest-user-management-and-limitations) gelten auch für externe Benutzer in Dynamics AX.

### Bekannte Einschränkungen für externe Azure AD-Benutzer

- Externe Benutzer, bei denen es sich um Administratoren handelt, können Benutzer von Partnerunternehmen nicht Verzeichnissen (B2B-Zusammenarbeit) außerhalb ihres Basisverzeichnisses hinzufügen.
- Externe Benutzer können mehrinstanzenfähigen Anwendungen in Verzeichnissen außerhalb ihres Basisverzeichnisses nicht ihre Zustimmung geben.
- PowerBI unterstützt den Zugriff durch externe Benutzer derzeit nicht.
- Das Office-Portal unterstützt die Lizenzierung externer Benutzer nicht.

## Verwaltung von Gastbenutzern und Einschränkungen

Gastkonten sind Benutzer aus anderen Verzeichnissen, die in Ihr Verzeichnis eingeladen wurden, um Zugriff auf SharePoint-Dokumente, Anwendungen oder andere Azure-Ressourcen zu erhalten. Für ein Gastkonto in Ihrem Verzeichnis ist das zugrunde liegende UserType-Attribut auf „Gast“ festgelegt. Reguläre Benutzer (Mitglieder Ihres Verzeichnisses) verfügen über das UserType-Attribut „Member“.

Gäste verfügen im Verzeichnis über eingeschränkte Berechtigungen. Mit diesen Berechtigungen wird für Gäste die Möglichkeit eingeschränkt, Informationen zu anderen Benutzern im Verzeichnis zu ermitteln. Gastbenutzer können aber trotzdem mit den Benutzern und Gruppen interagieren, die den verwendeten Ressourcen zugeordnet sind. Gastbenutzer haben folgende Möglichkeiten:

- Anzeigen anderer Benutzer und Gruppen eines Azure-Abonnements, dem sie zugewiesen sind
- Anzeigen der Mitglieder von Gruppen, denen sie angehören
- Suchen nach anderen Benutzern im Verzeichnis, sofern die vollständige E-Mail-Adresse des Benutzers bekannt ist
- Anzeigen einer begrenzten Gruppe von Attributen der gesuchten Benutzer (Anzeigename, E-Mail-Adresse, Benutzerprinzipalname (UPN) und Miniaturbild)
- Abrufen einer Liste mit den überprüften Domänen im Mandantenverzeichnis
- Erteilen der Zustimmung für Anwendungen und Gewähren des gleichen Zugriffs, der für Mitglieder in Ihrem Verzeichnis gilt

## Festlegen von Richtlinien für den Benutzerzugriff

Die Registerkarte **Konfigurieren** eines Verzeichnisses enthält Optionen zur Zugriffssteuerung für externe Benutzer. Diese Optionen können nur im klassischen Azure-Portal von einem globalen Verzeichnisadministrator geändert werden. Es gibt derzeit keine PowerShell- oder API-Methode.

Wählen Sie zum Öffnen im klassischen Azure-Portal die Registerkarte **Konfigurieren** und dann **Active Directory** und den Namen des Verzeichnisses.

![Registerkarte „Konfigurieren“ in Azure Active Directory][1]

Anschließend können Sie die Optionen zur Zugriffssteuerung für externe Benutzer ändern.

![][2]


## Nächste Schritte

- [Verwalten von Azure AD](active-directory-administer.md)
- [Verwalten von Kennwörtern in Azure AD](active-directory-manage-passwords.md)
- [Verwalten von Gruppen in Azure AD](active-directory-manage-groups.md)

<!--Image references-->
[1]: ./media/active-directory-create-users/RBACDirConfigTab.png
[2]: ./media/active-directory-create-users/RBACGuestAccessControls.png

<!---HONumber=AcomDC_0413_2016-->