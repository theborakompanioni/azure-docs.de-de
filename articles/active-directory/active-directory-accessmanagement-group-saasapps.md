
<properties
	pageTitle="Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen | Microsoft Azure"
	description="Erfahren Sie, wie Sie Gruppen in Azure Active Directory Premium oder Basic den Zugriff auf SaaS-Anwendungen zuweisen, die in Azure Active Directory integriert sind."
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
	ms.date="08/10/2016"
	ms.author="curtand"/>


# Verwenden einer Gruppe zum Verwalten des Zugriffs auf SaaS-Anwendungen

Mit Azure Active Directory (Azure AD) mit einer Azure AD Premium- oder Azure AD-Basic-Lizenz können Sie Gruppen verwenden, um Zugriff auf eine SaaS-Anwendung zu gewähren, die in Azure AD integriert ist. Wenn Sie beispielsweise der Marketingabteilung Zugriff auf fünf verschiedene SaaS-Anwendungen erteilen möchten, können Sie eine Gruppe erstellen, die die Benutzer der Marketingabteilung enthält, und diese Gruppe dann diesen fünf SaaS-Anwendungen zuweisen, die von der Marketingabteilung benötigt werden. Auf diese Weise sparen Sie Zeit, da Sie die Mitgliedschaft für die Marketingabteilung zentral verwalten können. Benutzer werden der Anwendung zugewiesen, wenn sie als Mitglieder der Marketinggruppe hinzugefügt werden. Ebenso wird die Zuweisung der Benutzer zur Anwendung entfernt, wenn sie als Mitglieder aus der Marketinggruppe entfernt werden.

Diese Funktion kann für Hunderte von Anwendungen verwendet werden, die Sie aus dem Azure AD-Anwendungskatalog hinzufügen können.

**So weisen Sie einer Gruppe den Zugriff auf eine SaaS-Anwendung zu**

1. Wählen Sie im [klassischen Azure-Portal](https://manage.windowsazure.com) auf der Navigationsleiste links die Option **Active Directory** aus.

2. Klicken Sie auf die Registerkarte **Verzeichnis**, und öffnen Sie dann das Verzeichnis, in dem Sie einer Gruppe den Zugriff auf eine SaaS-Anwendung zuweisen möchten.

3. Wählen Sie die Registerkarte **Anwendungen** aus. Wählen Sie eine der aus dem Anwendungskatalog hinzugefügten Anwendungen aus, und klicken Sie dann auf die Registerkarte **Benutzer und Gruppen**.

4. Geben Sie auf der Registerkarte **Benutzer und Gruppen** im Feld **Beginnend mit** den Namen der Gruppe ein, der Sie Zugriff zuweisen möchten, und wählen Sie dann das Häkchen oben rechts aus. Sie müssen nur den ersten Teil des Gruppennamens eingeben.

5. Wählen Sie die Gruppe aus, und klicken Sie dann auf die Schaltfläche **Zugriff zuweisen**. Wählen Sie **Ja** aus, wenn die Bestätigungsmeldung angezeigt wird. Geschachtelte Gruppenmitgliedschaften werden für die gruppenbasierte Zuordnung zu Anwendungen derzeit nicht unterstützt.

6. Sie können auch sehen, welche Benutzer der Anwendung entweder direkt oder durch die Mitgliedschaft in einer Gruppe zugewiesen sind. Ändern Sie hierzu die Anzeige der Dropdownliste **Gruppen** zu **Alle Benutzer**. Die Liste zeigt die Benutzer im Verzeichnis und zeigt an, ob ein Benutzer der Anwendung zugewiesen ist oder nicht. Die Liste zeigt außerdem an, ob die Benutzer der Anwendung direkt (angezeigter Zuweisungstyp "Direkt") oder aufgrund der Gruppenmitgliedschaft (angezeigter Zuweisungstyp "Geerbt") zugewiesen sind.


> [AZURE.NOTE]
Die Registerkarte „Benutzer und Gruppen“ wird nur angezeigt, wenn Sie Azure AD Premium oder Azure AD Basic aktiviert haben.

##Verwandte Artikel

Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Verwalten des Zugriffs auf Ressourcen mit Azure Active Directory-Gruppen](active-directory-manage-groups.md)

* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)

* [Azure Active Directory-Cmdlets zum Konfigurieren von Gruppeneinstellungen](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Was ist Azure Active Directory?](active-directory-whatis.md)

* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)

<!---HONumber=AcomDC_0817_2016-->