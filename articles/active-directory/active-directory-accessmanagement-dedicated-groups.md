<properties
	pageTitle="Dedizierte Gruppen in Azure Active Directory | Microsoft Azure"
	description="Übersicht über die Funktionsweise dedizierter Gruppen in Azure Active Directory und ihrer Erstellung."
	services="active-directory"
	documentationCenter=""
	authors="curtand"
	manager="femila"
	editor=""
	/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/10/2016"
	ms.author="curtand"/>

# Dedizierte Gruppen in Azure Active Directory

Die Funktion „dedizierte Gruppen“ erstellt und befüllt in Azure Active Directory (Azure AD) die Mitgliedschaft für vordefinierte Azure AD-Gruppen. Mitglieder dedizierter Gruppen können weder über das klassische Azure-Portal, noch mit Windows PowerShell-Cmdlets oder programmgesteuert hinzugefügt oder entfernt werden.

>[AZURE.NOTE] Dedizierte Gruppen erfordern, dass folgenden Benutzern eine Azure AD Premium-Lizenz zugewiesen wurde:
>- Dem Administrator, der die Gruppenregel verwaltet
>- Allen Benutzern, die durch die Regel als Gruppenmitglieder ausgewählt wurden

**Dedizierte Gruppen aktivieren**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) die Option **Active Directory**, und öffnen Sie dann das Verzeichnis Ihrer Organisation.

2. Wählen Sie die Registerkarte **Gruppen**, und öffnen Sie dann die Gruppe, die Sie bearbeiten möchten.

3. Wählen Sie die Registerkarte **Konfigurieren**, und legen Sie dann **Zugeordnete Gruppen aktivieren** auf **Ja** fest.

Sobald der Schalter "Zugeordnete Gruppen aktivieren" auf **Ja** festgelegt ist, können Sie das Verzeichnis für das automatische Erstellen der dedizierten Gruppe **"Alle Benutzer" aktivieren**, indem Sie den Schalter "Gruppe 'Alle Benutzer' aktivieren" auf** Ja** festlegen. Sie können den Namen dieser dedizierten Gruppe bearbeiten, indem Sie Text in das Feld **Anzeigename für Gruppe "Alle Benutzer "** eingeben.

Mithilfe der Gruppe „Alle Benutzer“ können allen Benutzern in Ihrem Verzeichnis die gleichen Berechtigungen zugewiesen werden. Beispielsweise können Sie allen Benutzern in Ihrem Verzeichnis Zugriff auf eine SaaS-Anwendung gewähren, indem Sie der dedizierten Gruppe "Alle Benutzer" Zugriff auf diese Anwendung zuweisen.

Die dedizierte Gruppe „Alle Benutzer“ umfasst alle Benutzer im Verzeichnis, also auch Gäste und externe Benutzer. Wenn Sie eine Gruppe benötigen, die externe Benutzer ausschließt, können Sie dazu eine Gruppe mit einer attributbasierten, dynamischen Regel erstellen, wie z.B.:

				(user.userPrincipalName -notContains "#EXT#@")

Verwenden Sie für eine Gruppe, die alle Gäste ausschließt, eine Regel wie:

				(user.userType -ne "Guest")

Weitere Informationen zur Erstellung *erweiterter* Regeln (mit mehreren Vergleichen) für dynamische Gruppenmitgliedschaften finden Sie unter [Verwenden von Attributen zum Erstellen erweiterter Regeln](active-directory-accessmanagement-groups-with-advanced-rules.md).


Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Was ist Azure Active Directory?](active-directory-whatis.md)
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0817_2016-->