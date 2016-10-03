<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit RightScale | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und RightScale konfigurieren."
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
	ms.date="09/19/2016"
	ms.author="jeedes"/>


# Tutorial: Azure Active Directory-Integration mit RightScale

Dieses Tutorial soll Ihnen zeigen, wie Sie RightScale in Azure Active Directory (Azure AD) integrieren können. Die Integration von RightScale in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf RightScale hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei RightScale anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit RightScale konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein RightScale-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden von Azure AD in einer Testumgebung zu testen. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von RightScale aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von RightScale aus dem Katalog
Zum Konfigurieren der Integration von RightScale in Azure AD müssen Sie RightScale aus dem Katalog zur Liste der verwalteten SaaS-Apps hinzufügen.

**Um RightScale aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
 
	![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **RightScale** ein.
 
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_01.png)

7. Wählen Sie im Ergebnisbereich **RightScale** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Auswählen der App im Katalog](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in RightScale konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, wer das entsprechende Gegenstück in RightScale zu einem Benutzer in Azure AD ist. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in RightScale muss eine Linkbeziehung eingerichtet werden.


Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei RightScale müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines RightScale-Testbenutzers](#creating-a-rightscale-test-user)**, um eine Entsprechung von Britta Simon in RightScale zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer RightScale-Anwendung zu konfigurieren.



**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei RightScale die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **RightScale** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][6]

2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei RightScale anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_03.png)

3. Führen Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** die folgenden Schritte aus, wenn Sie die Anwendung im **IdP-initiierten Modus** konfigurieren möchten, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_04.png)


    a. Geben Sie im Textfeld „Antwort-URL“ die URL nach folgendem Muster ein: `https://login.rightscale.com/login/saml2/consume`

	b. Klicken Sie auf **Weiter**.

4. Wenn die Anwendung im **SP-initiierten Modus** konfiguriert werden soll, klicken Sie auf der Dialogfeldseite **App-Einstellungen konfigurieren** auf **Zeigen Sie die erweiterten Einstellungen an (optional)**, geben Sie die **Anmelde-URL** ein, und klicken Sie dann auf **Weiter**.
 
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_06.png)

	a. Geben Sie im Textfeld „Anmelde-URL“ die URL, die von Ihren Benutzern zur Anmeldung bei der RightScale-Anwendung verwendet wird, nach folgendem Muster ein: `https://login.rightscale.com/`

	b. Klicken Sie auf **Weiter**.

5. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für RightScale** die folgenden Schritte aus, und klicken Sie dann auf **Weiter**:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_05.png)

    a. Klicken Sie auf **Zertifikat herunterladen**, und speichern Sie das Base64-codierte Zertifikat auf Ihrem Computer.

    b. Klicken Sie auf **Weiter**.


5. Zum Konfigurieren des einmaligen Anmeldens für Ihre Anwendung müssen Sie sich als Administrator bei Ihrem RightScale-Mandanten anmelden.

	a. Klicken Sie im Menü oben auf die Registerkarte **Settings**, und wählen Sie **Single Sign-On**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_001.png)

	b. Klicken Sie auf die Schaltfläche **new**, um **Your SAML Identity Providers** hinzuzufügen.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_002.png)

	c. Geben Sie im Textfeld **Display Name** den Namen Ihres Unternehmens ein.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_003.png)

	d. Wählen Sie in unten stehendem Textfeld **Allow RightScale-initiated SSO using a discovery hint** aus, und geben Sie Ihren **Domänennamen** in das unten stehende Textfeld ein.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_004.png)

	e. Kopieren Sie die SAML-SSO-URL von Azure AD in das Feld **SAML SSO Endpoint** in RightScale.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_005.png)
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_006.png)

	f. Kopieren Sie die Entitäts-ID von Azure AD in das Feld **SAML EntityID** in RightScale.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_007.png)
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_008.png)

	g. Klicken Sie auf die Schaltfläche **Browser**, um das Zertifikat hochzuladen, das Sie in Schritt 4 heruntergeladen haben.
	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_009.png)

	h. Klicken Sie auf **Speichern**.
	


6. Wählen Sie im klassischen Portal die Bestätigung zur Konfiguration der einmaligen Anmeldung aus, und klicken Sie dann auf **Weiter**.

	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
  
	![Azure AD – einmaliges Anmelden][11]




### Erstellen eines Azure AD-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Testbenutzers namens Britta Simon im klassischen Azure-Portal.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im **klassischen Azure-Portal** im linken Navigationsbereich auf **Active Directory**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_05.png)

    a. Wählen Sie als **Benutzertyp** die Option **Neuer Benutzer in Ihrer Organisation** aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-rightscale-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines RightScale-Testbenutzers

In diesem Abschnitt erstellen Sie in RightScale einen Benutzer mit dem Namen Britta Simon. Arbeiten Sie über support@rightscale.com mit dem RightScale-Supportteam zusammen, um Benutzer in der RightScale-Plattform hinzuzufügen.


### Zuweisen des Azure AD-Testbenutzers

Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens bei Azure zu ermöglichen, indem sie Zugriff auf RightScale erhält.

![Benutzer zuweisen][200]

**Um Britta Simon zu RightScale zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

2. Wählen Sie in der Anwendungsliste den Eintrag **RightScale** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-rightscale-tutorial/tutorial_rightscale_50.png)

1. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

1. Wählen Sie in der Benutzerliste **Britta Simon** aus.

2. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]



### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich. Wenn Sie im Zugriffsbereich auf die Kachel "RightScale" klicken, sollten Sie automatisch in Ihrer RightScale-Anwendung angemeldet werden.
	

## Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-rightscale-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0921_2016-->