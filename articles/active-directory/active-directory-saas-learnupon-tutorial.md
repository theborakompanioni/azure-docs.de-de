<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Novatus | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und LearnUpon konfigurieren."
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
	ms.date="08/15/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit LearnUpon

Dieses Tutorial soll Ihnen zeigen, wie Sie LearnUpon in Azure Active Directory (Azure AD) integrieren können. Die Integration von LearnUpon in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf LearnUpon hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei LearnUpon anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten: im klassischen Azure Active Directory-Portal.


Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit LearnUpon konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein LearnUpon-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von LearnUpon aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von LearnUpon aus dem Katalog
Zum Konfigurieren der Integration von LearnUpon in Azure AD müssen Sie LearnUpon aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um LearnUpon aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld als Suchbegriff **LearnUpon** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_01.png)

7. Wählen Sie im Ergebnisbereich **LearnUpon** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand einer Testbenutzerin namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in LearnUpon konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in LearnUpon als Gegenbenutzer zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in LearnUpon muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert dem **Benutzernamen** in LearnUpon zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei LearnUpon müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines LearnUpon-Testbenutzers](#creating-a-learnupon-test-user)**, um eine Entsprechung von Britta Simon in LearnUpon zu erhalten, die mit ihrer Entsprechung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer LearnUpon-Anwendung zu konfigurieren.



**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in LearnUpon die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **LearnUpon** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei LearnUpon anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_03.png)

3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_04.png)


    a. Geben Sie im Textfeld **Antwort-URL** die Assertion Consumer Service-URL im folgenden Format ein: `https://<companyname>.learnupon.com/saml/consumer`

    b. Klicken Sie auf **Next**.


4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für LearnUpon** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen** und speichern Sie die Datei auf Ihrem Computer. Dieses Zertifikat und die Metadaten-URLs (Entitäts-ID, Anmelde-URL für SSO und Abmelde-URL für SSO) werden benötigt, um SSO (Single Sign-On, einmaliges Anmelden) aufseiten von LearnUpon einzurichten.

    b. Klicken Sie auf **Next**.




1. Öffnen Sie eine weitere Browserinstanz, und melden Sie sich mit einem Administratorkonto bei LearnUpon an.

1. Klicken Sie auf die Registerkarte **Einstellungen**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_06.png)


1. Klicken Sie auf **Single Sign-On - SAML** (Einmaliges Anmelden – SAML) und anschließend auf **Allgemeine Einstellungen**, um SAML-Einstellungen konfigurieren.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_07.png)


5. Führen Sie im Abschnitt **Allgemeine Einstellungen** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_08.png)

	a. Wählen Sie **Aktiviert**.

	b. Wählen Sie **2.0** als **Version** aus.

	c. Wählen Sie für **Skip conditions** (Bedingungen überspringen) die Option **Nein**.

	d. Geben Sie im Textfeld **SAML Token Post param name** (POST-Parametername für das SAML-Token) den Namen des POST-Parameters zur oben angegebenen SAML-Verbraucher-URL ein, die die zu überprüfende und authentifizierende SAML-Assertion enthält. Beispiel **SAMLResponse**.

	e. Geben Sie im Textfeld **Namensbezeichnerformat** den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich die Benutzer-ID (E-Mail-Adresse) befindet. Beispiel: **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.

	f. Geben Sie im Textfeld **Identify Provider Location** (Anbieterstandort identifizieren) den Wert ein, der angibt, an welche Adresse die Benutzer weitergeleitet werden, wenn sie auf dem Anmeldebildschirm des klassischen Azure-Portals auf das von Ihnen hochgeladene Symbol klicken.

	g. Kopieren Sie im klassischen Azure-Portal die **Dienst-URL für einmalige Abmeldung**, und fügen Sie sie in das Textfeld **Abmelde-URL** ein.

	h. Klicken Sie auf **Manage finger prints** (Fingerabdrücke verwalten), und laden Sie dann den Fingerabdruck des heruntergeladenen Zertifikats hoch.


1. Klicken Sie auf **Benutzereinstellungen**, und führen Sie die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_11.png)

	a. Geben Sie im Textfeld **First Name Identifier Format** (Format der Vornamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Vorname des Benutzers befindet, z.B. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**.

	b. Geben Sie im Textfeld **Last Name Identifier Format** (Format der Nachnamen-ID) den Wert ein, der angibt, an welcher Stelle in der SAML-Assertion sich der Nachname des Benutzers befindet, z.B. **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**.


6. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.

	![Azure AD – einmaliges Anmelden][11]




### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-learnupon-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines LearnUpon-Testbenutzers

Das Ziel dieses Abschnitts ist das Erstellen einer Benutzerin namens Britta Simon in LearnUpon. LearnUpon unterstützt die Just-in-Time-Bereitstellung, die standardmäßig aktiviert ist.

Für Sie steht in diesem Abschnitt kein Aktionselement zur Verfügung. Wenn noch kein Benutzer vorhanden ist, wird beim Zugreifen auf LearnUpon ein neuer Benutzer erstellt. [Konfigurieren der einmaligen Anmeldung in Azure AD](#configuring-azure-ad-single-single-sign-on)

> [AZURE.NOTE] Wenn Sie einen Benutzer manuell erstellen müssen, setzen Sie sich mit dem Supportteam von LearnUpon in Verbindung.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf LearnUpon erhält.

![Benutzer zuweisen][200]

**Um Britta Simon LearnUpon zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste **LearnUpon** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-learnupon-tutorial/tutorial_learnupon_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel „LearnUpon“ klicken, sollten Sie automatisch bei Ihrer LearnUpon-Anwendung angemeldet werden.


## Zusätzliche Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-learnupon-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0817_2016-->