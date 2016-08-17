<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit DocuSign | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und DocuSign konfigurieren."
	services="active-directory"
	documentationCenter=""
	authors="jeevansd"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit DocuSign

In diesem Tutorial wird die Integration von Azure und DocuSign erläutert. Das in diesem Lernprogramm verwendete Szenario setzt voraus, dass Sie bereits über die folgenden Elemente verfügen:

- Ein gültiges Azure-Abonnement
- Einen Mandanten in DocuSign



Das in diesem Lernprogramm beschriebene Szenario besteht aus den folgenden Bausteinen:

1. [Aktivieren der Anwendungsintegration für DocuSign](#enabling-the-application-integration-for-docusign)


2. [Konfigurieren der einmaligen Anmeldung](#configuring-single-sign-on)


3. [Konfigurieren der Kontobereitstellung](#configuring-account-provisioning)


4. [Zuweisen von Benutzern](#assigning-users)

    ![Konfigurieren der einmaligen Anmeldung][0]
 

## Aktivieren der Anwendungsintegration für DocuSign

In diesem Abschnitt wird beschrieben, wie Sie die Anwendungsintegration für DocuSign aktivieren.

### Führen Sie die folgenden Schritte aus, um die Anwendungsintegration für DocuSign zu aktivieren:

1. Klicken Sie im klassischen Azure-Portal im linken Navigationsbereich auf **Active Directory**.

	![Konfigurieren der einmaligen Anmeldung][1]

2. Wählen Sie in der Liste Verzeichnis das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Konfigurieren der einmaligen Anmeldung][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld „Was möchten Sie tun?“ auf **Anwendung aus dem Katalog hinzufügen**.

	![Konfigurieren der einmaligen Anmeldung][4]


6. Geben Sie im Suchfeld das Wort **DocuSign** ein.

	![Konfigurieren der einmaligen Anmeldung][5]

7. Wählen Sie im Ergebnisbereich **DocuSign** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Konfigurieren der einmaligen Anmeldung][6]


## Konfigurieren der einmaligen Anmeldung

In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Azure AD-Konto bei DocuSign zu authentifizieren.


### So konfigurieren Sie einmaliges Anmelden

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **DocuSign** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld „Einmaliges Anmelden konfigurieren“ zu öffnen.

	![Konfigurieren der einmaligen Anmeldung][7]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei DocuSign anmelden?** die Option **Microsoft Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf „Weiter“.

	![Konfigurieren der einmaligen Anmeldung][8]

3. Führen Sie auf der Seite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Konfigurieren der einmaligen Anmeldung][9]

	a. Geben Sie im Textfeld **Anmelde-URL** die URL Ihres DocuSign-Mandanten im folgenden Format ein: Für die Produktionsumgebung lautet das URL-Muster **"https://account.docusign.com/organizations/<ORGANISATIONS-ID>/saml2/login/sp/<IDP-ID>"**. Für die Demoumgebung lautet das URL-Muster **"https://account-d.docusign.com/organizations/<ORGANISATIONS-ID>/saml2/login/sp/<IDP-ID>"**.

	b. Geben Sie im Textfeld **Bezeichner** die URL des DocuSign-Ausstellers im folgenden Format ein: Für die Produktionsumgebung lautet das URL-Muster **"https://account.docusign.com/organizations/<ORGANISATIONS-ID>/saml2"**. Für die Demoumgebung lautet das URL-Muster **"https://account-d.docusign.com/organizations/<ORGANISATIONS-ID>/saml2"**.

	c. Klicken Sie auf **Weiter**.


    > [AZURE.TIP] Falls Sie die App-URL für Ihren Mandanten nicht kennen, können Sie sich unter [SSOSetup@Docusign.com](emailTo:SSOSetup@Docusign.com) an DocuSign wenden, um die vom Service Provider initiierte SSO-URL für den Mandanten zu erfragen.
 

4. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für DocuSign** auf **Zertifikat herunterladen**, und speichern Sie die Zertifikatsdatei lokal auf Ihrem Computer.

	![Konfigurieren der einmaligen Anmeldung][10]


5. Melden Sie sich in einem anderen Webbrowserfenster im **DocuSign-Verwaltungsportal** als Administrator an.


6. Klicken Sie im Navigationsmenü auf der linken Seite auf **Domänen**.

	![Konfigurieren der einmaligen Anmeldung][51]

7. Klicken Sie im rechten Bereich jetzt auf die Schaltfläche **CLAIM DOMAIN** (DOMÄNE BEANSPRUCHEN).

	![Konfigurieren der einmaligen Anmeldung][52]

8. Geben Sie im Popupfenster den Domänennamen Ihres Unternehmens ein, und klicken Sie auf „Claim“ (Beanspruchen). Achten Sie darauf, die Domäne zu überprüfen, und vergewissern Sie sich, dass der Status als „Aktiv“ angezeigt wird.

	![Konfigurieren der einmaligen Anmeldung][53]

9. Klicken Sie im Navigationsmenü auf der linken Seite auf **Identitätsanbieter**.

	![Konfigurieren der einmaligen Anmeldung][54]

10. Klicken Sie im rechten Bereich auf die Schaltfläche „ADD IDENTITY PROVIDER“ (IDENTITÄTSANBIETER HINZUFÜGEN). Die Seite mit den SSO-Einstellungen wird geöffnet.
	
	![Konfigurieren der einmaligen Anmeldung][55]

11. Führen Sie auf der Seite mit den Identitätsanbietereinstellungen die unten angegebenen Aktionen durch.

	a. Geben Sie den eindeutigen Namen für Ihre Konfiguration ein. Verwenden Sie keine Leerzeichen zwischen den Wörtern.

	b. Geben Sie im Textfeld **Aussteller des Identitätsanbieters** den Wert für die **Aussteller-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	c. Geben Sie im Textfeld **Anmelde-URL für Identitätsanbieter** den Wert für die **Remoteanmelde-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	d. Geben Sie im Textfeld **Abmelde-URL für Identitätsanbieter** den Wert für die **Remoteabmelde-URL** aus dem Konfigurations-Assistenten der Azure AD-Anwendung ein.

	e. Aktivieren Sie das Kontrollkästchen **Sign AuthN Request** (Authentifizierungsanforderung signieren).

	f. Stellen Sie sicher, dass die Option **Send AuthN request by:** (Authentifizierungsanforderung senden per:) auf **POST** festgelegt ist.

	g. Stellen Sie sicher, dass die Option **Send logout request by:** (Abmeldeanforderung senden per:) auf **POST** festgelegt ist.

	![Konfigurieren der einmaligen Anmeldung][56]

12. Wählen Sie im Abschnitt **Benutzerdefinierte Attributzuordnung** das Feld aus, das Sie dem Azure AD-Anspruch zuordnen möchten. Wir haben beispielsweise den Anspruch **emailaddress** verwendet, der dem Wert als **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** zugeordnet ist. Dies ist der Standard-Anspruchsname aus Azure AD für den E-Mail-Anspruch.

	> [AZURE.NOTE] Verwenden Sie den entsprechenden Benutzerbezeichner, um den Benutzer aus Azure AD der DocuSign-Benutzerzuordnung zuzuordnen. Wählen Sie das richtige Feld, und geben Sie den entsprechenden Wert basierend auf den Einstellungen Ihrer Organisation ein.

	![Konfigurieren der einmaligen Anmeldung][57]

13. Klicken Sie im Abschnitt **Identity Provider Certificate** (Identitätsanbieterzertifikat) auf die Schaltfläche **ZERTIFIKAT HINZUFÜGEN**, und laden Sie das Zertifikat hoch, das Sie aus dem Konfigurations-Assistenten der Azure AD-Anwendung heruntergeladen haben.

	![Konfigurieren der einmaligen Anmeldung][58]

14. Klicken Sie jetzt auf die Schaltfläche **Speichern**, um alle Einstellungen zu speichern.

15. Klicken Sie im Abschnitt **Identitätsanbieter** auf die Schaltfläche **Aktionen** und dann auf **Endpunkte**.

	![Konfigurieren der einmaligen Anmeldung][59]

16. Führen Sie im Abschnitt **SAML 2.0-Endpunkte** die folgenden Schritte aus:

	a. Kopieren Sie die **Service Provider Issuer URL** (Service Provider-Aussteller-URL), und geben Sie sie im Textfeld **Bezeichner** des Azure AD-Konfigurations-Assistenten ein.

	b. Kopieren Sie die **Service Provider Login URL** (Service Provider-Anmelde-URL), und geben Sie sie im Textfeld **Anmelde-URL** des Azure AD-Konfigurations-Assistenten ein.

	![Konfigurieren der einmaligen Anmeldung][60]

	c. Klicken Sie auf **Schließen**.

15. Wählen Sie im klassischen Azure-Portal die **Bestätigung zur Konfiguration des einmaligen Anmeldens** aus, und klicken Sie dann auf **Weiter**.

	![Anwendungen][14]

10. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Anwendungen][15]
 

## Konfigurieren der Kontobereitstellung

In diesem Abschnitt wird erläutert, wie Sie die Bereitstellung von Active Directory-Benutzerkonten für DocuSign aktivieren.

### So konfigurieren Sie die Benutzerbereitstellung

1. Klicken Sie im **klassischen Azure-Portal** auf der **Anwendungsintegrationsseite für DocuSign** auf **Kontobereitstellung konfigurieren**, um das Dialogfeld „Benutzerbereitstellung konfigurieren“ zu öffnen.

	![Konfigurieren der Kontobereitstellung][30]

2. Geben Sie auf der Seite **Einstellungen und Administrator-Anmeldeinformationen** zum Aktivieren der automatischen Benutzerbereitstellung die Anmeldeinformationen eines DocuSign-Kontos mit ausreichenden Rechten ein, und klicken Sie auf **Weiter**.

	![Konfigurieren der Kontobereitstellung][31]

3. Klicken Sie im Dialogfeld **Verbindung testen** auf **Test starten**, und klicken Sie auf **Weiter**, wenn der Test erfolgreich ist.

	![Konfigurieren der Kontobereitstellung][32]

3. Klicken Sie auf der Bestätigungsseite auf **Abschließen**.

	![Konfigurieren der Kontobereitstellung][33]
 

## Zuweisen von Benutzern

Um Ihre Konfiguration zu testen, müssen Sie den Azure AD-Benutzern, denen Sie die Verwendung Ihrer Anwendung ermöglichen möchten, Zugriff auf die Anwendung gewähren. Weisen Sie dazu der Anwendung Benutzer zu.

### Führen Sie die folgenden Schritte aus, um DocuSign Benutzer zuzuweisen:

1. Erstellen Sie im **klassischen Azure-Portal** ein Testkonto.

2. Klicken Sie auf der **Anwendungsintegrationsseite für DocuSign** auf **Benutzer zuweisen**.

	![Zuweisen von Benutzern][40]
 

3. Wählen Sie den Testbenutzer aus, klicken Sie auf **Zuweisen** und anschließend auf **Ja**, um die Zuweisung zu bestätigen.

	![Zuweisen von Benutzern][41]


Nach 10 Minuten können Sie überprüfen, ob das Konto mit DocuSign synchronisiert wurde.

Als ersten Überprüfungsschritt können Sie den Bereitstellungsstatus überprüfen, indem Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für DocuSign auf „Dashboard“ klicken.

![Zuweisen von Benutzern][42]

Ein erfolgreich abgeschlossener Benutzerbereitstellungszyklus wird durch einen entsprechenden Status angezeigt:

![Zuweisen von Benutzern][43]


Wenn Sie die Einstellungen für einmaliges Anmelden testen möchten, öffnen Sie den Zugriffsbereich.

Weitere Informationen zum Zugriffsbereich finden Sie unter „Einführung in den Zugriffsbereich“.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[0]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_00.png
[1]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_01.png
[6]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_02.png
[7]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_03.png
[8]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_04.png
[9]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_05.png
[10]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_06.png

[14]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_10.png
[15]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_11.png

[30]: ./media/active-directory-saas-docusign-tutorial/tutorial_general_400.png
[31]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_12.png
[32]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_13.png
[33]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_14.png



[40]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_15.png
[41]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_16.png
[42]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_17.png
[43]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_18.png

[51]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/active-directory-saas-docusign-tutorial/tutorial_docusign_28.png

<!---HONumber=AcomDC_0803_2016-->