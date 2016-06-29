<properties
	pageTitle="Veröffentlichen von Apps mit Azure AD-Anwendungsproxy | Microsoft Azure"
	description="Es wird beschrieben, wie Sie lokale Anwendungen mit dem Azure AD-Anwendungsproxy in der Cloud veröffentlichen."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="06/17/2016"
	ms.author="kgremban"/>


# Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy


Nachdem Sie den Microsoft Azure Active Directory (AD)-Anwendungsproxy aktiviert haben, können Sie lokale Anwendungen veröffentlichen, damit Remotebenutzer darauf von außerhalb des privaten Netzwerks zugreifen können.

In diesem Artikel werden Sie durch die Schritte zum Veröffentlichen von Anwendungen geführt, die in Ihrem lokalen Netzwerk ausgeführt werden, sowie zum Ermöglichen des sicheren Remotezugriffs von außerhalb Ihres Netzwerks. Führen Sie vor dem Fortfahren die Schritte unter [Aktivieren des Anwendungsproxys im Azure-Portal](active-directory-application-proxy-enable.md) aus, falls Sie den Anwendungsproxy noch nicht eingerichtet und noch keine Connectors installiert haben.

Wenn Sie den Azure AD-Anwendungsproxy zum ersten Mal verwenden, sollten Sie den Connector testen, indem Sie vor dem Veröffentlichen von Anwendungen eine Website aus Ihrem privaten Netzwerk veröffentlichen.

> [AZURE.NOTE] Das Feature "Anwendungsproxy" ist nur verfügbar, wenn Sie Azure Active Directory auf die Premium oder Basic Edition aktualisiert haben. Weitere Informationen finden Sie unter [Azure Active Directory-Editionen](active-directory-editions.md).

## Veröffentlichen einer App mithilfe des Assistenten

1. Melden Sie sich als Administrator beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.
2. Navigieren Sie zu Active Directory, und wählen Sie das Verzeichnis aus, in dem Sie den Anwendungsproxy aktiviert haben.

	![Active Directory – Symbol](./media/active-directory-application-proxy-publish/ad_icon.png)

3. Klicken Sie auf die Registerkarte **Anwendungen** und dann am unteren Bildschirmrand auf **Hinzufügen**.

	![Anwendung hinzufügen](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. Wählen Sie **Eine Anwendung veröffentlichen, die von außerhalb Ihres Netzwerks aufgerufen werden kann**.

	![Eine Anwendung veröffentlichen, die von außerhalb Ihres Netzwerks aufgerufen werden kann](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Geben Sie die folgenden Informationen zur Anwendung an:

	- **Name**: Dies ist der benutzerfreundliche Namen für Ihre Anwendung. Er muss in Ihrem Verzeichnis eindeutig sein.
	- **Interne URL**: Die Adresse, die vom Anwendungsproxy-Connector verwendet wird, um aus dem internen privaten Netzwerk auf die Anwendung zuzugreifen. Sie können einen bestimmten Pfad auf dem Back-End-Server für die Veröffentlichung angeben, während der Rest des Servers nicht veröffentlicht wird. Auf diese Weise können Sie unterschiedliche Websites auf demselben Server veröffentlichen und jeweils einen eigenen Namen und Zugriffsregeln vergeben.
	- **Präauthentifizierungsmethode**: Gibt das Verfahren an, wie der Anwendungsproxy Benutzer überprüft, bevor diese Zugriff auf Ihre Anwendung erhalten. Wählen Sie im Dropdownmenü eine Option aus.

		- Azure Active Directory: Der Anwendungsproxy leitet Benutzer an die Anmeldung mit Azure AD um. Hierbei werden deren Berechtigungen für das Verzeichnis und die Anwendung authentifiziert.
		- Pass-Through: Benutzer müssen sich nicht authentifizieren, um Zugriff auf die Anwendung zu erhalten.

	![Anwendungseigenschaften](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)

6. Um den Assistenten zu beenden, klicken Sie auf das Häkchen unten im Bildschirm. Die Anwendung ist jetzt in Azure AD definiert.


## Zuweisen von Benutzern und Gruppen zur Anwendung

Damit Benutzer auf die von Ihnen veröffentlichte Anwendung zugreifen können, müssen Sie diese entweder einzeln oder in Gruppen zuweisen. Bei Apps, für die eine Präauthentifizierung erforderlich ist, werden hierbei Berechtigungen zum Verwenden der App erteilt. Bei Apps, für die keine Präauthentifizierung erforderlich ist, müssen Benutzer der App trotzdem zugewiesen sein, damit sie in der Anwendungsliste angezeigt wird.

1. Nach dem Beenden des Assistenten für das Hinzufügen von Apps wird die Schnellstartseite für die Anwendung angezeigt. Wählen Sie die Option **Benutzer und Gruppen**, um zu verwalten, wer Zugriff auf die App hat.

	![Anwendungsproxy-Schnellstart – Benutzer zuweisen – Screenshot](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Suchen Sie in Ihrem Verzeichnis nach bestimmten Gruppen, oder zeigen Sie alle Benutzer an. Klicken Sie zum Anzeigen der Suchergebnisse auf das Häkchen.

  	![Nach Gruppen oder Benutzern suchen – Screenshot](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Wählen Sie die einzelnen Benutzer oder Gruppen aus, die Sie dieser App zuweisen möchten, und klicken Sie auf **Zuweisen**. Sie werden aufgefordert, die Aktion zu bestätigen.

> [AZURE.NOTE] Für Apps mit „Integrierter Windows-Authentifizierung“ können Sie nur Benutzer und Gruppen zuweisen, die über Ihr lokales Active Directory synchronisiert werden. Benutzer, die sich über ein Microsoft-Konto anmelden, und Gäste können nicht für Apps zugewiesen werden, die mit dem Azure Active Directory-Anwendungsproxy veröffentlicht werden. Stellen Sie sicher, dass Ihre Benutzer sich mit Anmeldeinformationen anmelden, die derselben Domäne wie die von Ihnen veröffentlichte App angehören.


## Erweiterte Konfiguration

Auf der Seite „Konfigurieren“ können Sie veröffentlichte Apps ändern oder erweiterte Optionen einrichten. Auf dieser Seite können Sie Ihre App anpassen, indem Sie den Namen ändern oder ein Logo hochladen. Außerdem können Sie Zugriffsregeln verwalten, z.B. die Präauthentifizierungsmethode oder die Multi-Factor Authentication.

![Erweiterte Konfiguration](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Nachdem Sie Anwendungen mit dem Azure Active Directory-Anwendungsproxy veröffentlicht haben, werden sie in der Liste der Anwendungen in Azure AD aufgeführt, und Sie können sie dort verwalten.

Wenn Sie Anwendungsproxydienste deaktivieren, nachdem Sie Anwendungen veröffentlicht haben, ist der Zugriff darauf von außerhalb Ihres privaten Netzwerks nicht mehr möglich. Die Anwendungen werden hierbei nicht gelöscht.

Um eine Anwendung anzuzeigen und den Zugriff darauf sicherzustellen, doppelklicken Sie auf den Namen der Anwendung. Wenn der Anwendungsproxydienst deaktiviert und die Anwendung nicht verfügbar ist, wird am oberen Bildschirmrand eine Warnmeldung angezeigt.

Um eine Anwendung zu löschen, wählen Sie eine Anwendung in der Liste aus, und klicken Sie dann auf **Löschen**.

## Nächste Schritte

- [Veröffentlichen von Anwendungen mit Ihrem eigenen Domänennamen](active-directory-application-proxy-custom-domains.md)
- [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
- [Arbeiten mit Anwendungen, die Ansprüche unterstützen](active-directory-application-proxy-claims-aware-apps.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/).

<!---HONumber=AcomDC_0622_2016-->