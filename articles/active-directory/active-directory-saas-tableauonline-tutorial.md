<properties
	pageTitle="Tutorial: Azure Active Directory-Integration mit Tableau Online | Microsoft Azure"
	description="Erfahren Sie, wie Sie das einmalige Anmelden für Azure Active Directory und Tableau Online konfigurieren."
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


# Tutorial: Azure Active Directory-Integration mit Tableau Online

In diesem Tutorial erfahren Sie, wie Sie Tableau Online in Azure Active Directory (Azure AD) integrieren.

Die Integration von Tableau Online in Azure AD bietet die folgenden Vorteile:

- Sie können in Azure AD steuern, wer Zugriff auf Tableau Online hat.
- Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Tableau Online anzumelden (einmaliges Anmelden).
- Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## Voraussetzungen

Um die Azure AD-Integration mit Tableau Online konfigurieren zu können, benötigen Sie Folgendes:

- Ein Azure AD-Abonnement
- Ein **Tableau Online**-Abonnement, für das einmaliges Anmelden aktiviert ist


> [AZURE.NOTE] Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden.


Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

- Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
- Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/) eine einmonatige Testversion anfordern.


## Beschreibung des Szenarios
In diesem Tutorial testen Sie das einmalige Anmelden für Azure AD in einer Testumgebung. Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von Tableau Online aus dem Katalog
2. Konfigurieren und Testen der einmaligen Anmeldung von Azure AD


## Hinzufügen von Tableau Online aus dem Katalog
Zum Konfigurieren der Integration von Tableau Online in Azure AD müssen Sie Tableau Online aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Tableau Online aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.

	![Active Directory][1]

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Anwendungen][2]

4. Klicken Sie unten auf der Seite auf **Hinzufügen**.

	![Anwendungen][3]

5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.

	![Anwendungen][4]

6. Geben Sie im Suchfeld den Suchbegriff **Tableau Online** ein.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_01.png)

7. Wählen Sie im Ergebnisbereich **Tableau Online** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_02.png)

##  Konfigurieren und Testen der einmaligen Anmeldung von Azure AD
In diesem Abschnitt konfigurieren und testen Sie das einmalige Anmelden von Azure AD bei Tableau Online basierend auf einem Testbenutzer mit dem Namen Britta Simon.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in Tableau Online als Gegenstück zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in Tableau Online muss eine Linkbeziehung eingerichtet werden. Diese Linkbeziehung wird hergestellt, indem Sie den **Benutzernamen** in Azure AD als Wert für den **Benutzernamen** in Tableau Online zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens in Azure AD bei Tableau Online müssen Sie die folgenden Schritte ausführen:

1. **[Konfigurieren von Azure AD – einmaliges Anmelden](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieser Funktion zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)**, um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
4. **[Erstellen eines Tableau Online-Testbenutzers](#creating-a-Tableau-Online-test-user)**, um eine Entsprechung von Britta Simon in Tableau Online zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
5. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)**, um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### Konfigurieren des einmaligen Anmeldens von Azure AD

Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer Tableau Online-Anwendung zu konfigurieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in Tableau Online die folgenden Schritte aus:**

1. Klicken Sie im oberen Menü auf **Schnellstart**.

	![Einmaliges Anmelden konfigurieren][6]
2. Klicken Sie im klassischen Portal auf der Anwendungsintegrationsseite für **Tableau Online** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.

	![Einmaliges Anmelden konfigurieren][7]

3. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei Tableau Online anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
 	
	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_06.png)

4. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_07.png)


    a. Geben Sie im Textfeld „Anmelde-URL“ eine URL im folgenden Format ein: `https://sso.online.tableau.com`

	c. Klicken Sie auf **Weiter**.

5. Klicken Sie auf der Seite **Einmaliges Anmelden konfigurieren für Tableau Online** auf **Metadaten herunterladen**, und speichern Sie die Metadatendatei auf Ihrem Computer.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_08.png)

6. Wählen Sie die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**.
	
	![Azure AD – einmaliges Anmelden][10]

7. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.
  	
	![Azure AD – einmaliges Anmelden][11]
8. Melden Sie sich in einem anderen Browserfenster in Ihrer Tableau Online-Anwendung als Administrator an. Wechseln Sie zu **Einstellungen** und dann zu **Authentifizierung**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_09.png)

9. Gehen Sie im Abschnitt **Authentifizierungstypen** wie folgt vor. Aktivieren Sie das Kontrollkästchen **Single sign-on with SAML** (Einmaliges Anmelden mit SAML), um SAML zu aktivieren.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_12.png)

10. Scrollen Sie nach unten bis zum Abschnitt **Import metadata file into Tableau Online** (Metadatendatei in Tableau Online importieren). Klicken Sie auf „Durchsuchen“, und importieren Sie die Metadatendatei, die Sie aus Azure AD heruntergeladen haben. Klicken Sie anschließend auf **Übernehmen**.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_13.png)

11. Fügen Sie im Abschnitt **Match assertions** (Assertionen abgleichen) den entsprechenden Identitätsanbieter-Assertionsnamen für E-Mail-Adresse, Vorname und Nachname ein. Gehen Sie wie folgt vor, um diese Informationen aus Azure AD abzurufen:

	a. Wechseln Sie zurück zu Azure AD. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite von **Tableau Online** im Menü oben auf **Attribute**. Kopieren Sie den Namen für die Werte „userprincipalname“, „givenname“ und „surname“.
     
    ![Azure AD – einmaliges Anmelden](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_10.png)

	b. Wechseln Sie zur Tableau Online-Anwendung, und legen Sie den Abschnitt **Tableau Online Attributes** (Tableau Online-Attribute) dann wie folgt fest:
	
	-  E-Mail: **mail** oder **userprincipalname**
	-  Vorname: **givenname**
	-  Nachname: **surname**

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_14.png)

### Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt erstellen Sie im klassischen Portal einen Testbenutzer mit dem Namen Britta Simon.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_09.png)

2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.

3. Klicken Sie im Menü oben auf **Benutzer**, um die Liste der Benutzer anzuzeigen.
	
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_03.png)

4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_04.png)

5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus:
 
	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_05.png)

    a. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.

    b. Geben Sie in das Textfeld **Benutzername** den Text **BrittaSimon** ein.

    c. Klicken Sie auf **Weiter**.

6.  Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_06.png)

    a. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.

    b. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.

    c. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.

    d. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.

    e. Klicken Sie auf **Weiter**.

7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_07.png)

8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/create_aaduser_08.png)

    a. Notieren Sie den Wert von **Neues Kennwort**.

    b. Klicken Sie auf **Fertig stellen**.



### Erstellen eines Tableau Online-Testbenutzers

In diesem Abschnitt erstellen Sie in Tableau Online einen Benutzer namens Britta Simon.

1. Klicken Sie in **Tableau Online** auf **Settings** (Einstellungen) und dann auf den Abschnitt **Authentication** (Authentifizierung). Scrollen Sie nach unten zum Abschnitt **Select Users** (Benutzer auswählen). Klicken Sie auf **Add Users** (Benutzer hinzufügen) und dann auf **Enter Email Addresses** (E-Mail-Adressen eingeben).

	![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_15.png)
2. Wählen Sie **Add users for single sign-on (SSO) authentication** (Benutzer für SSO-Authentifizierung (einmaliges Anmelden) hinzufügen). Fügen Sie im Textfeld **Enter Email Addresses** (E-Mail-Adressen eingeben) den Eintrag britta.simon@contoso.com hinzu.

	![Erstellen einesAzure AD-Testbenutzers](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_11.png)

3.  Klicken Sie auf **Erstellen**.


### Zuweisen des Azure AD-Testbenutzers

In diesem Abschnitt ermöglichen Sie Britta Simon die Verwendung des einmaligen Anmeldens von Azure, indem Sie ihr Zugriff auf Tableau Online gewähren.

![Benutzer zuweisen][200]

**Zum Zuweisen von Britta Simon zu Tableau Online führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Portal in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen**.

	![Benutzer zuweisen][201]

3. Wählen Sie in der Anwendungsliste **Tableau Online** aus.

	![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-tableauonline-tutorial/tutorial_tableauonline_50.png)

4. Klicken Sie im oberen Menü auf **Benutzer**.

	![Benutzer zuweisen][203]

5. Wählen Sie in der Liste „Alle Benutzer“ den Eintrag **Britta Simon** aus.

6. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.

	![Benutzer zuweisen][205]


### Testen der einmaligen Anmeldung

Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden über den Zugriffsbereich.

Wenn Sie im Zugriffsbereich auf die Kachel „Tableau Online“ klicken, sollten Sie automatisch bei Ihrer Tableau Online-Anwendung angemeldet werden.

## Weitere Ressourcen

* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_06.png
[7]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-tableauonline-tutorial/tutorial_general_205.png

<!---HONumber=AcomDC_0817_2016-->